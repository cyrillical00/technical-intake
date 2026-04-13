# Output 2 — Implementation Prompt Template

Load this file when generating Output 2 after scope lock confirmation.
Also load `references/stack-defaults.md` to infer the suggested tech stack.

This prompt is designed to be pasted directly into any LLM (Claude, GPT, etc.) to begin
code generation. It should be self-contained — the recipient should not need the spec to
start building.

---

## Template

```
You are a senior software developer. Build [product name].

## North Star
[The single outcome this product must deliver, verbatim from the client]

## What it does
[1 paragraph, plain language]

## v1 (MVP) — Build this first
[The smallest version that delivers the North Star. Be explicit about what is NOT in v1.]

## v2+ (Vision) — Do not build yet
[Future capabilities — listed so architecture decisions don't accidentally block them]

## Users
[bullet list of user types and their needs]

## Must-have features (MVP)
[numbered list, P0 first]

## Data & integrations
[list each data source, type, and direction]

## Security requirements
[auth model, data sensitivity level, compliance flags — or "none identified"]

## Platform
[web/mobile/desktop, key constraints]

## Constraints & non-goals
[what NOT to build, compliance notes, budget posture]

## Success criteria
[how to verify correctness against the North Star]

## Suggested stack
[infer best-fit technologies from references/stack-defaults.md.
If security flags present, note specific security requirements per stack choice.]

## Client technical context
[T0: "Client is non-technical. Use plain language in all client-facing UI copy and documentation.
Avoid exposing technical errors directly to users."
T1: "Client uses standard tools (spreadsheets/Notion). Familiarity with product concepts but not code."
T2: "Client is tech-adjacent. May review decisions and has worked with developers before."
T3: "Client is technical. Can participate in stack decisions, review code, and contribute."]

## Handoff readiness checklist
Before considering this build complete, verify:
- [ ] Core user flow works end-to-end without developer assistance
- [ ] Error states are handled gracefully (user sees a clear message, not a stack trace)
- [ ] Data can be backed up or exported by the client without developer involvement
- [ ] Auth and access controls are enforced at the server layer (not just hidden in UI)
- [ ] All third-party API keys are stored in environment variables, not in code
- [ ] Basic logging is in place so errors can be diagnosed after launch
- [ ] Client has been trained on how to perform common admin tasks
- [ ] A README or admin guide exists for whoever maintains this after launch
- [ ] Scope lock items marked "deferred" are not partially built (no half-built features)
- [ ] All screens have loading states (skeleton loaders or spinners, not blank)
- [ ] All screens have empty states (helpful message when no data exists yet)
- [ ] UI is responsive — tested on mobile viewport even if primary target is desktop
- [ ] Color contrast meets accessibility tier specified in spec §7
- [ ] Primary user flow is completable without any tooltips, modals, or help text required

Begin by asking any clarifying questions before writing code.
```

---

## Notes

- The "Suggested stack" section should be populated using `references/stack-defaults.md`.
  Infer from spec — do not ask the client to choose a stack (unless T3, in which case
  include their stated preferences alongside your recommendation).
- The "Client technical context" section should include only the tier-appropriate paragraph.
- If security flags were raised, add to the prompt: *"Never hardcode credentials. Use
  environment variables for all secrets."*
