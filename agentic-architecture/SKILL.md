---
name: agentic-architecture
description: Use when the user wants to design the architecture of an AI-powered or agentic application. Also use when the user mentions 'state machine,' 'deterministic vs indeterministic,' 'memory layer,' 'dead letter queue,' 'circuit breaker,' 'agent loop,' 'retry logic,' 'event queue,' 'LLM orchestration,' 'tool use architecture,' or 'when should I use an LLM vs a rule.'
---

# Agentic Architecture

Designs AI-powered systems that are reliable, debuggable, and extensible — by choosing the right pattern for each decision point before writing a line of code.

## The Core Decision: Deterministic vs Indeterministic

Every step in an agentic system is either **deterministic** (rule-based, predictable, auditable) or **indeterministic** (LLM-driven, flexible, variable). Getting this wrong is the most common architectural mistake.

| Axis | Deterministic | Indeterministic |
|---|---|---|
| **Engine** | Code, rules, regex, lookup tables | LLM call |
| **Output** | Same input → same output, always | Same input → variable output |
| **Debuggability** | Trivial — log the input/output | Hard — requires eval suite |
| **Cost** | Near zero per call | Token cost per call |
| **Latency** | Milliseconds | Seconds |
| **Best for** | Routing, validation, formatting, filtering | Understanding, generation, judgment |

**Decision rule:** Use an LLM only when the task requires understanding, judgment, or generation that cannot be expressed as a rule. Everything else should be deterministic code.

**The test:** Can you write a unit test that covers every valid input? If yes — write the code, not the prompt.

### Common Misclassifications

| Task | Wrong choice | Right choice |
|---|---|---|
| Classify input into 3 known categories | LLM | `if/switch` with enum |
| Extract a date from a user message | LLM | Regex + date parser |
| Decide which tool to call next | Deterministic rule | LLM with structured output |
| Generate a personalized email | Deterministic template | LLM |
| Validate that required fields are present | LLM | Schema validator |
| Summarize a 10,000-word document | Deterministic | LLM |

## State Machine Design

Agentic loops are state machines. Model them explicitly before coding.

**States every agent needs:**
- `IDLE` — waiting for input
- `RUNNING` — executing a step
- `WAITING_FOR_TOOL` — tool call dispatched, awaiting result
- `WAITING_FOR_HUMAN` — blocked on human approval or input
- `COMPLETED` — terminal success
- `FAILED` — terminal failure (with reason)
- `RETRYING` — in backoff loop

**Transitions to design explicitly:**
- What triggers each state change?
- What is persisted at each transition? (never rely on in-memory state for long-running agents)
- What happens if the process crashes mid-transition?

**Rule:** If your agent cannot be paused, serialized to disk, and resumed from any state — it will fail in production.

## Memory Layers

Design memory explicitly. Agents that mix memory layers produce inconsistent, hard-to-debug behavior.

| Layer | Scope | Storage | Example |
|---|---|---|---|
| **Working memory** | Current step only | In-process variable | Tool call result being processed |
| **Session memory** | Current conversation | In-memory or session store | Previous messages in a chat thread |
| **Episodic memory** | Past interactions | Database | "Last time this user asked X, they meant Y" |
| **Semantic memory** | Facts and knowledge | Vector store | Product documentation, company policies |
| **Procedural memory** | How to do things | Code or prompt | System prompt, tool definitions |

**Design questions for each layer:**
1. What goes in? (write contract)
2. What comes out, and when? (read contract)
3. When is it cleared? (TTL or explicit eviction)
4. What happens when it's empty? (cold-start behavior)

## Event Queue + Dead Letter Queue

For multi-step or background agents, use an event queue — not direct function calls.

**Why queues:** Decouples producer from consumer, enables retry, survives process restarts, gives you an audit trail.

**Queue anatomy:**
```
Producer → Queue → Consumer (agent step)
               ↓ (on max retries exceeded)
           Dead Letter Queue (DLQ)
               ↓
           Alert / manual review
```

**Dead Letter Queue rules:**
- Every queue needs a DLQ — a queue without a DLQ silently discards failures
- DLQ messages must include: original payload, failure reason, retry count, timestamp
- DLQ must trigger an alert — an unchecked DLQ is a black hole
- DLQ messages must be replayable — fix the bug, replay the message, verify the result

**Message design:**
- Messages must be idempotent: processing the same message twice = same result as once
- Include a `message_id` for deduplication
- Include a `version` field for schema evolution

## Circuit Breaker

Protects your agent from cascading failures when a downstream service is degraded.

**States:**
- `CLOSED` — normal operation, requests pass through
- `OPEN` — downstream is failing, requests fail fast (no call made)
- `HALF-OPEN` — testing if downstream recovered (one probe request allowed)

**Configuration:**
```
failure_threshold: 5       # failures before opening
recovery_timeout: 30s      # how long to wait before half-open
success_threshold: 2       # successes in half-open before closing
```

**Where to apply:** Any call to an external API, LLM provider, database, or third-party service.

## Retry + Backoff

Retry is not a fallback for bad design — it handles transient failures (network blips, rate limits, temporary service degradation).

**Exponential backoff formula:** `delay = base_delay × 2^attempt + jitter`

**Sane defaults:**
```
max_retries: 3
base_delay: 1s
max_delay: 30s
jitter: ±20% of delay
retryable_errors: [429, 503, 504, network_timeout]
non_retryable_errors: [400, 401, 403, 404, 422]
```

**Never retry:** Validation errors, authentication failures, or any error caused by a bad request — retrying won't fix them.

## Agent Orchestration Patterns

**Sequential chain:** Step A → Step B → Step C
- Use when each step depends on the previous
- Simple but slow — total latency = sum of all steps

**Parallel fan-out:** Step A → [Step B, Step C, Step D] → Merge
- Use when steps are independent
- Total latency = slowest step
- Requires explicit merge logic

**Router:** Input → Classifier → Route to Agent A or Agent B or Agent C
- Use when different inputs need different agents
- Classifier should be deterministic when possible

**Supervisor loop:** Supervisor → Worker → Supervisor reviews → done or retry
- Use for quality-gated tasks (writing, code generation, research)
- Supervisor uses an LLM to evaluate worker output against criteria
- Cap iterations (max 3) — unbounded loops burn tokens and time

## LLM Call Design

**Structured output over prose:** Always request JSON output for machine-consumed LLM responses. Parsing prose is fragile.

**Temperature guidance:**
- `0.0` — classification, extraction, routing (determinism required)
- `0.3–0.7` — analysis, summarization (some variation acceptable)
- `0.7–1.0` — creative generation, brainstorming (variation is the point)

**Token budget discipline:**
- System prompt: < 500 tokens for focused agents
- Context window: leave 20% for output headroom
- Long documents: chunk + retrieve (RAG), don't stuff the whole thing

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll use an LLM to route between steps — it's more flexible" | Flexible routing is unpredictable routing. Use deterministic routing until you hit a case it can't handle. |
| "I don't need a DLQ — I'll just log failures" | Logs don't queue messages for replay. When the bug is fixed, the work is lost. |
| "State is in memory — it's simpler" | Memory state doesn't survive restarts. The first production crash will prove this. |
| "I'll add retry logic later" | Transient failures happen on the first deploy. Wire retry before launch. |
| "The LLM will figure out the memory" | LLMs don't manage memory — you do. Undefined memory = inconsistent agent behavior. |
| "Circuit breakers are for large systems" | Any agent calling an external API needs a circuit breaker. Size is irrelevant. |

## Verification

- [ ] Every step classified: deterministic or indeterministic, with justification
- [ ] State machine drawn with all states and transitions named
- [ ] Memory layers mapped: what's in each layer, TTL, cold-start behavior
- [ ] Every queue has a DLQ configured with alert
- [ ] Retry policy defined: max retries, backoff, retryable vs non-retryable errors
- [ ] Circuit breaker configured for every external call
- [ ] LLM calls use structured output (JSON) for machine-consumed responses
- [ ] Orchestration pattern chosen and justified (sequential / fan-out / router / supervisor)
