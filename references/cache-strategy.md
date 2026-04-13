# Cache Strategy — Output Generation (Developer Reference)

This file is for developers building a programmatic integration around this skill
(e.g., a Streamlit app, CLI tool, or automated pipeline). It does NOT apply to
interactive sessions in Claude.ai, Claude Code, or Perplexity Computer — those
environments handle caching automatically.

---

## Goal

Cache the large compiled interview context so all output generation calls share one
cache write, then read from it cheaply — typically 85–90% token cost reduction.

---

## Compiled Context Block

Before any output API call, assemble the full interview results into a single block:

```
COMPILED INTAKE INTERVIEW
════════════════════════════════════════════════════════
Project: [name]
NSM: [verbatim north star]
Tech Tier: [T0/T1/T2/T3]
UI Brief: [full UI brief from consolidation step]

ROUND 1 – DISCOVERY
[full client answers, verbatim or paraphrased]

ROUND 2 – USERS & WORKFLOWS
[full client answers]

ROUND 3 – FEATURES & DATA
[full client answers + sensitivity classification]

ROUND 4 – CONSTRAINTS & CONTEXT
[full client answers + security flags]

ROUND 5 – SUCCESS & EDGE CASES
[full client answers]

PARKING LOT RESOLUTIONS: [list]
SCOPE LOCK: [confirmed items]
RESEARCH RESULTS: [summaries of any searches run]
════════════════════════════════════════════════════════
```

---

## API Call Pattern

```javascript
// First call — writes compiled interview to cache, generates Output 1 (Spec)
const response = await fetch("https://api.anthropic.com/v1/messages", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    model: "claude-sonnet-4-6",
    max_tokens: 4000,
    system: [
      {
        type: "text",
        text: "You are a senior technical writer generating a product specification.",
      },
      {
        type: "text",
        text: COMPILED_INTERVIEW_CONTEXT,
        cache_control: { type: "ephemeral" }
      }
    ],
    messages: [
      { role: "user", content: "Generate Output 1: the full Technical Specification per the template." }
    ]
  })
});

// Second call — reads from cache (10% of input cost), generates Output 2
// CRITICAL: system array must be IDENTICAL to first call for cache hit
const response2 = await fetch("https://api.anthropic.com/v1/messages", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    model: "claude-sonnet-4-6",
    max_tokens: 3000,
    system: [
      { type: "text", text: "You are a senior technical writer generating a product specification." },
      { type: "text", text: COMPILED_INTERVIEW_CONTEXT, cache_control: { type: "ephemeral" } }
    ],
    messages: [
      { role: "user", content: "Generate Output 2: the Implementation Prompt." }
    ]
  })
});

// Third call — cache hit, generates Output 3 (React Mockup)
// Same pattern for Outputs 4 and 5
```

---

## Cache Rules

- The `system` array must be **byte-for-byte identical** across all calls or cache misses
- Minimum cacheable size: **1,024 tokens** (Sonnet). A full intake interview is typically
  ~800–2,500 tokens. If under threshold, no error — just no savings.
- Default TTL: **5 minutes**. All output calls must complete within 5 minutes of the first.
  If generation takes longer, use `"ttl": "1h"` (costs 2× write but guarantees hits).
- Monitor `cache_read_input_tokens` in the response `usage` object to verify hits.
- **Never include timestamps, request IDs, or dynamic content in the cached system block.**
