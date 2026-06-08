---
name: ai-agent-projects
description: Use when the user wants to build an AI agent, voice agent, chat agent, multi-agent system, RAG pipeline, or MCP server. Also use when the user mentions 'AI agent', 'voice agent', 'chat agent', 'RAG', 'retrieval-augmented generation', 'MCP server', 'multi-agent', 'OpenRouter', 'ElevenLabs', 'Deepgram', or 'pgvector'.
---

# AI Agent Projects

Expert knowledge for architecting and building AI agent systems: voice agents, chat agents, multi-agent orchestration, RAG pipelines, and MCP servers.

## Pattern Selection Guide

| You want to build | Use this pattern |
|---|---|
| Voice assistant / phone bot | Voice Agent (Telnyx + ElevenLabs + Deepgram) |
| Web chatbot with memory | Chat Agent (Next.js + Supabase + OpenRouter) |
| Autonomous task runner | Multi-Agent (orchestrator + workers) |
| Semantic search over docs/data | RAG Pipeline (pgvector) |
| Claude tool integration | MCP Server |

---

## 1. Voice Agent

**Stack:** Telnyx (telephony) + Deepgram (STT) + OpenRouter (LLM) + ElevenLabs (TTS)

**Architecture:**
```
Inbound call → Telnyx webhook → Deepgram (STT)
                                  ↓
                         OpenRouter (LLM + tools)
                                  ↓
                         ElevenLabs (TTS) → Telnyx (play audio)
```

**File structure:**
```
src/
  voice/
    webhook.ts          # Telnyx webhook handler
    stt.ts              # Deepgram STT client
    llm.ts              # OpenRouter client with tools
    tts.ts              # ElevenLabs TTS client
    session-store.ts    # Per-call conversation memory
```

**Key decisions:**
- Stream STT for low latency (don't wait for full utterance)
- Use sentence-boundary detection to trigger TTS before full LLM response
- Store conversation history per call session (in-memory or Redis)
- Target end-to-end latency < 1.5s (STT + LLM + TTS)

**Latency budget:**
| Component | Target |
|---|---|
| STT (streaming) | < 200ms |
| LLM first token | < 300ms |
| TTS first audio | < 300ms |
| Network round-trips | < 400ms |
| **Total** | **< 1.2s** |

---

## 2. Chat Agent

**Stack:** Next.js (frontend) + Supabase (persistence) + OpenRouter (LLM)

**File structure:**
```
app/
  api/
    chat/route.ts       # Streaming chat endpoint
src/
  lib/
    openrouter.ts       # OpenRouter client
    memory.ts           # Conversation history (Supabase)
    tools.ts            # Tool definitions
supabase/
  migrations/
    create_conversations.sql
    create_messages.sql
```

**Supabase schema:**
```sql
CREATE TABLE conversations (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid REFERENCES auth.users NOT NULL,
  title text,
  created_at timestamptz DEFAULT now()
);

CREATE TABLE messages (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  conversation_id uuid REFERENCES conversations NOT NULL,
  role text CHECK (role IN ('user', 'assistant', 'tool')) NOT NULL,
  content text NOT NULL,
  tool_calls jsonb,
  created_at timestamptz DEFAULT now()
);
```

**Memory strategy:**
- Short-term: last 20 messages in context window
- Long-term: summarize conversations > 30 messages, store summary as system message
- Semantic: embed messages + retrieve relevant history via pgvector (see RAG pattern)

**Streaming response (Next.js route):**
```typescript
import { OpenAI } from 'openai';

export async function POST(req: Request) {
  const { messages } = await req.json();
  const client = new OpenAI({ baseURL: 'https://openrouter.ai/api/v1', apiKey: process.env.OPENROUTER_API_KEY });
  
  const stream = await client.chat.completions.create({
    model: 'anthropic/claude-sonnet-4-5',
    messages,
    stream: true,
  });

  return new Response(
    new ReadableStream({
      async start(controller) {
        for await (const chunk of stream) {
          const text = chunk.choices[0]?.delta?.content || '';
          controller.enqueue(new TextEncoder().encode(text));
        }
        controller.close();
      },
    }),
    { headers: { 'Content-Type': 'text/plain; charset=utf-8' } }
  );
}
```

---

## 3. Multi-Agent System

**Pattern:** Orchestrator + specialized workers.

```
Orchestrator
  ├─ Worker A: Research
  ├─ Worker B: Code generation
  └─ Worker C: Verification
```

**Orchestrator responsibilities:**
- Parse high-level task into sub-tasks
- Route sub-tasks to the right worker
- Aggregate worker outputs
- Handle worker failures (retry, fallback, escalate)

**Worker responsibilities:**
- Single-purpose: one job, done well
- Idempotent: safe to retry
- Return structured output (JSON schema)
- Report errors explicitly (don't fail silently)

**Communication patterns:**
| Pattern | Use when |
|---|---|
| Sequential | Output of A is input to B |
| Parallel | A and B are independent; merge results |
| Map-reduce | Same task on N inputs; aggregate outputs |

**Tool design for agents:**
```typescript
const tools = [
  {
    type: 'function',
    function: {
      name: 'search_web',
      description: 'Search the web for current information. Use for facts, news, and recent events.',
      parameters: {
        type: 'object',
        properties: {
          query: { type: 'string', description: 'Search query' }
        },
        required: ['query']
      }
    }
  }
];
```

---

## 4. RAG Pipeline (pgvector)

**Stack:** Supabase pgvector + OpenAI embeddings (or any embedding model)

**Migration:**
```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE documents (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  content text NOT NULL,
  metadata jsonb,
  embedding vector(1536),  -- 1536 for text-embedding-3-small
  created_at timestamptz DEFAULT now()
);

CREATE INDEX ON documents USING ivfflat (embedding vector_cosine_ops)
  WITH (lists = 100);  -- lists ≈ sqrt(row count)
```

**Retrieval function:**
```sql
CREATE OR REPLACE FUNCTION match_documents(
  query_embedding vector(1536),
  match_threshold float DEFAULT 0.7,
  match_count int DEFAULT 5
)
RETURNS TABLE (id uuid, content text, similarity float)
LANGUAGE sql STABLE AS $$
  SELECT id, content, 1 - (embedding <=> query_embedding) AS similarity
  FROM documents
  WHERE 1 - (embedding <=> query_embedding) > match_threshold
  ORDER BY similarity DESC
  LIMIT match_count;
$$;
```

**Chunking strategy:**
- Chunk size: 512 tokens (balance between context and precision)
- Overlap: 50 tokens (prevents splitting mid-thought)
- Metadata: include source URL, section title, last updated date

**RAG prompt structure:**
```
System: You are a helpful assistant. Answer based on the context provided.
If the answer is not in the context, say so.

Context:
[Retrieved chunks here]

User: [Question]
```

---

## 5. MCP Server Pattern

**Use:** Expose tools to Claude (or any MCP-compatible client).

**File structure:**
```
src/
  index.ts              # MCP server entry point
  tools/
    search.ts           # Individual tool implementations
    database.ts
  resources/
    schema.ts           # Resource definitions
```

**Minimal MCP server (TypeScript):**
```typescript
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';

const server = new Server(
  { name: 'my-mcp-server', version: '1.0.0' },
  { capabilities: { tools: {} } }
);

server.setRequestHandler('tools/list', async () => ({
  tools: [{
    name: 'search',
    description: 'Search the knowledge base',
    inputSchema: {
      type: 'object',
      properties: { query: { type: 'string' } },
      required: ['query']
    }
  }]
}));

server.setRequestHandler('tools/call', async (request) => {
  if (request.params.name === 'search') {
    const results = await performSearch(request.params.arguments.query);
    return { content: [{ type: 'text', text: JSON.stringify(results) }] };
  }
});

const transport = new StdioServerTransport();
await server.connect(transport);
```

---

## Model Selection Guide

| Use case | Recommended model (via OpenRouter) |
|---|---|
| High reasoning, complex tasks | `anthropic/claude-opus-4-7` |
| Balanced quality/cost | `anthropic/claude-sonnet-4-6` |
| Fast, high-volume | `anthropic/claude-haiku-4-5` |
| Code generation | `anthropic/claude-sonnet-4-6` |
| Voice agent (speed critical) | `openai/gpt-4o-mini` |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll handle memory later" | Memory architecture is cheaper to design up front. Adding it to a live agent requires a migration. |
| "RAG is overkill for small datasets" | Under 1,000 documents: simple keyword search is fine. Over 1,000: pgvector pays for itself in retrieval quality. |
| "One agent can do everything" | Monolithic agents drift in behavior. Specialized workers are easier to test and replace. |
| "We don't need streaming" | Users abandon non-streaming chat UIs. Perceived latency matters more than actual latency. |
