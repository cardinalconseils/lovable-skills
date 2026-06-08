---
name: video-ai-direction
description: Use when the user wants to create AI-generated video content, ads, social clips, or product demos using Kling or similar video generation APIs. Also use when the user mentions 'AI video', 'Kling', 'video generation', 'text to video', 'image to video', 'video ad', 'social video', or 'video direction'.
---

# Video AI Direction

Expert knowledge for directing and generating AI video content using Kling API — ads, social clips, product demos, and brand films.

## Kling Model Selection

| Model | Quality | Speed | Best for |
|---|---|---|---|
| `kling-v1-5` | High | Fast | Default. Social content, ads, iteration. |
| `kling-v2` | Highest | Slower | Hero shots, premium creative, brand films. |

**Default to `kling-v1-5` for all work except final hero creative.**

## 8-Element Prompt Structure

Every video prompt needs all 8 elements for consistent, predictable output.

```
OPENING_FRAME:  [Describe the first frame in detail — same precision as a photo prompt]
MOTION:         [What moves, how, and at what speed. Be specific.]
SUBJECT:        [Primary subject — person, product, or scene]
STYLE:          [Visual style: cinematic, UGC, editorial, minimal, etc.]
CAMERA:         [Camera movement: static, push in, pull back, pan left, orbit, handheld]
LIGHTING:       [Lighting quality and direction]
MOOD:           [Emotional tone: urgent, serene, playful, bold, luxurious]
END_FRAME:      [What the final frame should show]
```

**Always include a negative prompt** to prevent common artifacts:
```
negative_prompt: blurry, distorted faces, watermark, text overlay, duplicate subjects, flickering, jump cuts
```

## Platform Specs

| Platform | Ratio | Duration | Notes |
|---|---|---|---|
| TikTok / Reels / Shorts | 9:16 | 5–15s | Hook in first 2 seconds |
| LinkedIn | 16:9 | 5–30s | Sound-off safe: add captions |
| YouTube Pre-roll | 16:9 | 15–30s | Skip button at 5s: hook before then |
| Twitter / X | 16:9 or 1:1 | 5–15s | Sound-off common |
| Product hero (web) | 16:9 | 5–10s | Loop seamlessly |

## Text-to-Video Example

```bash
curl -X POST https://api.klingai.com/v1/videos/text2video \
  -H "Authorization: Bearer $KLING_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model_name": "kling-v1-5",
    "prompt": "Opening frame: matte black bottle on frosted white surface, soft studio light. Motion: slow 45-degree orbit right. Subject: premium water bottle. Style: commercial product, clean. Camera: slow orbit right. Lighting: large softbox above-left, fill right. Mood: premium, minimal. End frame: front-facing, centered.",
    "negative_prompt": "blurry, distorted, watermark, text overlay, flickering",
    "aspect_ratio": "16:9",
    "duration": "5"
  }'
```

**Poll for completion:**
```bash
curl https://api.klingai.com/v1/videos/{task_id} \
  -H "Authorization: Bearer $KLING_API_KEY"
# Poll every 15s until status = "completed"
# Response includes video_url for download
```

## Image-to-Video Example

Start from a product photo (from photo-direction skill) and animate it:

```bash
curl -X POST https://api.klingai.com/v1/videos/image2video \
  -H "Authorization: Bearer $KLING_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model_name": "kling-v1-5",
    "image_url": "https://yourcdn.com/product-shot.png",
    "prompt": "Slow push-in toward the product. Light shimmer across the surface. Steady, premium motion.",
    "negative_prompt": "distortion, flicker, blur",
    "duration": "5"
  }'
```

**Image-to-video advantages:**
- Guarantees the opening frame looks exactly as intended
- Avoids "lottery" on product appearance
- Use for products where exact appearance matters (hardware, packaging)

## The 5-Second Rule

Every video ad must accomplish one of these in the first 5 seconds:

1. **Interrupt** — Something visually unexpected that stops the scroll
2. **Communicate** — The core benefit stated or shown clearly
3. **Direct** — Tell the viewer exactly what to do or notice

If your first 5 seconds don't satisfy at least one, rethink the opening frame.

## Video Ad Formats by Funnel Stage

| Stage | Format | Length | Objective |
|---|---|---|---|
| Cold (awareness) | UGC-style, problem/hook | 15–30s | Stop scroll, create curiosity |
| Warm (consideration) | Product demo, feature highlight | 30–60s | Show capability, build trust |
| Retargeting | Testimonial, offer, urgency | 15s | Convert decision |
| Retention | Tutorial, tip, feature discovery | 30–60s | Deepen engagement |

## Quality Review Checklist

- [ ] First 5 seconds: interrupt, communicate, or direct
- [ ] No distorted faces or unnatural motion artifacts
- [ ] Sound-off version communicates the message (caption or on-screen text)
- [ ] Platform aspect ratio correct
- [ ] Duration within platform recommended range
- [ ] Loop point is seamless (for web hero loops)
- [ ] Negative prompt included in generation to suppress artifacts
- [ ] End frame is clean and CTA-ready (for ads)

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "More detail in the prompt is always better" | Long unstructured prompts produce unpredictable results. Use the 8-element structure. |
| "We'll use the same video everywhere" | 9:16 for TikTok, 16:9 for LinkedIn — different ratios, different pacing. Generate per platform. |
| "The opening can build slowly" | Cold audiences scroll past in < 2 seconds. The hook is the video. |
| "Image-to-video is overkill" | When product appearance matters (always for commercial work), guaranteed opening frame beats video lottery. |
