---
name: technical-intake
description: >
  Turn a client's half-formed software idea into a developer-ready specification. Use when anyone
  says "I want to build something," "I have an app idea," "we need a tool that does X," or asks
  for help with technical requirements gathering or project scoping. Also triggers on: "help me
  scope this," "turn this into a spec," "my client needs a build plan," or any vague product
  description from a non-technical person. Runs a 25-question adaptive interview (5 rounds x 5),
  silently calibrates to the client's technical level, detects goal drift, and produces five
  outputs: a Technical Specification, an implementation prompt, an opt-in React UI mockup, ranked
  Claude tool recommendations, and a ready-to-install project style. Covers web apps, SaaS, mobile,
  automation, and data pipelines. The go-to skill for developers, consultants, and agencies turning
  client conversations into code.
---

# Technical Intake

> Turn a vague idea into a spec, a prompt, a UI mockup, a tool kit, and a custom style — in one conversation.

---

## What This Skill Does

Most clients can't describe what they want to build — not because they don't know, but because
they don't have the vocabulary. This skill bridges that gap. It runs a structured, adaptive
interview that feels like a smart conversation, not a form. By the end, both parties are aligned,
scope is locked, and five production-ready outputs are generated.

**Outputs:**
1. **Technical Specification Document** (markdown) — with screen inventory + user flow map
2. **Implementation Prompt** — ready to paste into any LLM for code generation
3. **React/HTML UI Mockup** — a working visual skeleton of the primary user flow
4. **Tool Recommendations** — personalised Claude ecosystem setup for the project
5. **Project Style** — a custom Claude style tuned to the project's communication needs

**Domains covered:** Web apps / SaaS · Mobile apps · Automation & bots · Data pipelines & dashboards

**Tone:** Warm, clear, non-technical language. Translate everything the client says into structured
requirements internally, but never talk "dev" at them.

---

## Reference Files

Load on demand — not all at once. The right file at the right moment, nothing more.

| File | When to Load |
|------|-------------|
| `references/domain-probes.md` | Round 1 signals a specific domain (e-commerce, scheduling, SaaS, etc.) |
| `references/stack-defaults.md` | Generating Output 2 — infer tech stack from spec |
| `references/security-probes.md` | Data sensitivity flag in Round 3, compliance in Round 4, or high/critical breach consequence. Also contains the Risk Pattern Library. |
| `references/ui-patterns.md` | Before generating Output 3 (React mockup). Also when design references are captured in Step 0d or Round 1. |
| `references/spec-template.md` | When generating Output 1 (Technical Specification). |
| `references/impl-prompt-template.md` | When generating Output 2 (Implementation Prompt). |
| `references/mockup-rules.md` | When generating Output 3 (React/HTML Mockup). |
| `references/tool-recommendations.md` | When generating Output 4 (Tool Recommendations). |
| `references/style-template.md` | When generating Output 5 (Project Style). Load after all other outputs are complete. |
| `references/cache-strategy.md` | Developer reference only — for programmatic API integrations. |

---

## Execution Protocol

### Step 0 — Opening Prompt + Silent Calibration + North Star Capture

Present these four prompts as **Question Cards** using the Card Rendering Protocol below.
The second is a **covert technical calibration** — it appears to be a project context question,
but the answer reveals the client's technical literacy tier.

For Step 0 cards, use `Round 0 of 5` and badge label `✦ Setup` with no round emoji prefix.
Progress bar starts at 0%.

**Step 0a — Project Description:**
Render as Question Card with question:
> "In plain language — no technical terms needed — describe what you want to build. What should it do, and who is it for?"

**Step 0b — Covert Technical Calibration:**
Render as Question Card with question:
> "What does your current setup look like for handling this today? Even if it's just a spreadsheet, a folder of emails, or nothing at all — I want to understand where you're starting from."

Silently assign a **Tech Tier** using this rubric:

| Tier | Client language signals | What it means |
|------|------------------------|---------------|
| **T0 — Non-technical** | "nothing," "email," "in my head," "a notebook" | No existing system. Use zero jargon. |
| **T1 — Tool-aware** | "spreadsheet," "Notion," "Airtable," "Google Sheets" | Comfortable with tools, no coding. Mild product vocabulary OK. |
| **T2 — Tech-adjacent** | "we hired someone," "I've tried Zapier," "our IT guy" | Has worked with developers. Understands integrations. |
| **T3 — Technical** | "existing codebase," "API," "GitHub," "Python," "our dev team" | Developer or technical founder. Full technical language OK. |

**Store Tech Tier silently.** It is a calibration instrument, not a label.

**Hard rules — never violate these in any card, summary, or chat text:**
- Never tell the client their tier (T0, T1, T2, T3) or any equivalent label
- Never say "as a non-technical user," "since you're not a developer," "for someone at your level," or any phrase that describes or implies a judgment about their technical ability
- Never acknowledge the calibration question as a calibration question
- Never reference vocabulary choices — just apply them silently
- Never use the tier to make assumptions visible: don't say "I'll keep this simple for you" or "I'll use plain language since..." — just do it
- If asked directly "how technical do you think I am?" — respond: "I tailor the conversation to whatever's most useful for you — no assessment needed on my end."

**Step 0c — North Star:**
Render as Question Card with question:
> "If this product could only do ONE thing perfectly, what would that be?"

This is the **North Star Metric (NSM)**. Store it explicitly. Every round summary card must
reference it. If later answers drift from it, flag the drift.

**Step 0d — Covert Design Signal:**
Render as Question Card with question:
> "Is there an app you use regularly that you think is really well-designed? What do you like about it?"

This surfaces visual style preferences without asking "what should it look like?" Store the
reference(s) as **Design References** and map:

| Client describes | Design signal to capture |
|-----------------|--------------------------|
| Names a specific app (Notion, Linear, Stripe, etc.) | Record exact app → load from `references/ui-patterns.md` |
| Describes a feeling ("clean," "simple," "powerful") | Tone: minimal / utilitarian / rich |
| Describes a color or material ("dark," "white space") | Color mode: dark / light / branded |
| Can't name anything / "I don't know" | Default: clean light-mode, system font, high contrast |
| Describes something physical ("like a receipt," "dashboard in a car") | Metaphor-driven UI — note for mockup |

**Research step:** After capturing design references, use whatever web search tool is available
to look up UI patterns for the referenced app. If no search tool is available, proceed from
reference files only.

---

### Card Rendering Protocol

**Every question, round opener, summary, and scope lock must be rendered as a visual card
using the `show_widget` tool.** Never present interview content as plain markdown text.
This is the primary UX of the skill — the card interface is what makes it feel like a
product, not a chat.

#### Question Card

Render one card per question. The card contains the question number, round badge, and
question text. After the client types a response in chat, render the next question card.

```html
<div style="font-family: system-ui, -apple-system, sans-serif; max-width: 600px; margin: 0 auto; padding: 8px;">
  <!-- Progress bar -->
  <div style="display:flex; align-items:center; gap:10px; margin-bottom:16px;">
    <div style="flex:1; height:4px; background:#e5e7eb; border-radius:2px;">
      <div style="height:4px; background:#2563eb; border-radius:2px; width:[PROGRESS]%;"></div>
    </div>
    <span style="font-size:12px; color:#6b7280; white-space:nowrap;">Q[N] of ~[TOTAL]</span>
  </div>

  <!-- Round badge -->
  <div style="display:inline-flex; align-items:center; gap:6px; background:#eff6ff;
    border:1px solid #bfdbfe; border-radius:20px; padding:4px 12px;
    font-size:12px; color:#1d4ed8; font-weight:500; margin-bottom:12px;">
    <span>[ROUND_EMOJI]</span>
    <span>Round [R] of 5 — [ROUND_THEME]</span>
  </div>

  <!-- Question card -->
  <div style="background:white; border:1.5px solid #e5e7eb; border-radius:12px;
    padding:20px 24px; box-shadow:0 1px 3px rgba(0,0,0,0.06);">
    <p style="margin:0; font-size:16px; color:#111827; line-height:1.6; font-weight:400;">
      [QUESTION_TEXT]
    </p>
  </div>

  <!-- Hint text (optional — only if needed for clarity) -->
  <p style="margin:8px 0 0 4px; font-size:13px; color:#9ca3af; font-style:italic;">
    [HINT_TEXT_OR_EMPTY]
  </p>
  <!-- CARD CONTENT RULES:
    - Question text: neutral, direct, no assumptions about the client's knowledge
    - Hint text: concrete examples only — never "in case you're not sure what this means"
    - Never include: tier labels, background assessments, ability judgments, or calibration signals
    - Never include: "Silently noted: T0" or any internal annotation visible to the client
    - Round badge shows theme only — never "simplified round" or similar
  -->
</div>
```

**Variable substitution:**
- `[N]` — current question number (e.g. 7)
- `[TOTAL]` — ~25 standard, ~9 Quick Mode
- `[PROGRESS]` — (N / TOTAL) × 100, capped at 100
- `[ROUND_EMOJI]` — 🔎 R1 · 👥 R2 · ⚙️ R3 · 🔒 R4 · ✓ R5
- `[R]` — round number 1–5
- `[ROUND_THEME]` — Discovery / Users & Workflows / Features & Data / Constraints / Success & Edge Cases
- `[QUESTION_TEXT]` — the question, plain language, no markdown
- `[HINT_TEXT_OR_EMPTY]` — optional example or clarification, or leave empty

#### Round Opener Card

Render at the start of each round, before Q1 of that round.

```html
<div style="font-family: system-ui, -apple-system, sans-serif; max-width: 600px; margin: 0 auto; padding: 8px;">
  <div style="background: linear-gradient(135deg, #1e3a5f 0%, #2563eb 100%);
    border-radius:12px; padding:20px 24px; color:white;">
    <div style="font-size:24px; margin-bottom:8px;">[ROUND_EMOJI]</div>
    <div style="font-size:11px; text-transform:uppercase; letter-spacing:0.08em;
      opacity:0.7; margin-bottom:4px;">Round [R] of 5</div>
    <div style="font-size:20px; font-weight:600; margin-bottom:8px;">[ROUND_THEME]</div>
    <div style="font-size:14px; opacity:0.85; line-height:1.5;">[ROUND_DESCRIPTION]</div>
  </div>
</div>
```

**[ROUND_DESCRIPTION] values:**
- R1: "Let's understand the problem this product needs to solve and why it needs to exist."
- R2: "Let's map out who uses this, how they use it, and how often."
- R3: "Let's nail down what it must do and what information it works with."
- R4: "Let's understand the practical constraints — platform, budget, timeline, and rules."
- R5: "Let's define what success looks like and think through what could go wrong."

#### Round Summary Card

Render after each round, before proceeding to the next.

```html
<div style="font-family: system-ui, -apple-system, sans-serif; max-width: 600px; margin: 0 auto; padding: 8px;">
  <div style="background:#f8fafc; border:1.5px solid #e2e8f0; border-radius:12px; padding:20px 24px;">

    <!-- Header -->
    <div style="display:flex; align-items:center; gap:8px; margin-bottom:16px;">
      <div style="width:28px; height:28px; background:#dcfce7; border-radius:50%;
        display:flex; align-items:center; justify-content:center; font-size:14px;">✓</div>
      <span style="font-weight:600; color:#111827; font-size:15px;">Round [R] complete</span>
    </div>

    <!-- Key facts -->
    <div style="display:flex; flex-direction:column; gap:8px; margin-bottom:16px;">
      [FACTS — repeat this block for each fact:]
      <div style="display:flex; gap:10px; align-items:flex-start;">
        <span style="color:#2563eb; margin-top:2px; font-size:14px;">•</span>
        <span style="font-size:14px; color:#374151; line-height:1.5;">[FACT_TEXT]</span>
      </div>
    </div>

    <!-- North Star check -->
    <div style="background:#eff6ff; border-left:3px solid #2563eb;
      border-radius:0 8px 8px 0; padding:10px 14px; margin-bottom:16px;">
      <span style="font-size:12px; text-transform:uppercase; letter-spacing:0.06em;
        color:#1d4ed8; font-weight:600;">North Star check</span>
      <p style="margin:4px 0 0; font-size:13px; color:#1e40af; line-height:1.5;">[NSM_CHECK]</p>
    </div>

    <!-- Confirm prompt -->
    <p style="margin:0; font-size:14px; color:#6b7280; font-style:italic;">
      Does this look right, or did I miss anything?
    </p>
  </div>
</div>
```

**Summary card content rules:**
- Facts describe the **project** only — never assessments of the **person**
- North Star check describes alignment between answers and the NSM — nothing about the client
- Never include: "you seem to have a clear vision," "for someone new to this," "great job explaining," or any evaluation of the client themselves
- Brief acknowledgment of good answers (per UX rules) appears as in-line chat text only — never inside a card field

#### Scope Lock Card

Render in place of the plain-text scope lock confirmation.

```html
<div style="font-family: system-ui, -apple-system, sans-serif; max-width: 600px; margin: 0 auto; padding: 8px;">
  <div style="background:white; border:1.5px solid #e5e7eb; border-radius:12px;
    overflow:hidden; box-shadow:0 2px 8px rgba(0,0,0,0.06);">

    <!-- Header -->
    <div style="background:#111827; padding:16px 20px; color:white;">
      <div style="font-size:13px; opacity:0.6; margin-bottom:2px; text-transform:uppercase;
        letter-spacing:0.08em;">Scope Lock</div>
      <div style="font-size:18px; font-weight:600;">Ready to generate your outputs</div>
    </div>

    <!-- In scope -->
    <div style="padding:16px 20px; border-bottom:1px solid #f3f4f6;">
      <div style="font-size:12px; font-weight:600; color:#16a34a; text-transform:uppercase;
        letter-spacing:0.06em; margin-bottom:10px;">✅ In scope for v1</div>
      [IN_SCOPE_ITEMS — repeat for each:]
      <div style="display:flex; gap:8px; align-items:flex-start; margin-bottom:6px;">
        <span style="font-size:13px; color:#374151;">[ITEM]</span>
      </div>
    </div>

    <!-- Out of scope -->
    <div style="padding:16px 20px; border-bottom:1px solid #f3f4f6;">
      <div style="font-size:12px; font-weight:600; color:#dc2626; text-transform:uppercase;
        letter-spacing:0.06em; margin-bottom:10px;">🚫 Out of scope (v2 or later)</div>
      [OUT_SCOPE_ITEMS — repeat for each:]
      <div style="display:flex; gap:8px; align-items:flex-start; margin-bottom:6px;">
        <span style="font-size:13px; color:#374151;">[ITEM]</span>
      </div>
    </div>

    <!-- Screen count + confirm -->
    <div style="padding:14px 20px; background:#f9fafb;">
      <div style="font-size:13px; color:#6b7280; margin-bottom:8px;">
        MVP estimated at <strong style="color:#111827;">[N] screens</strong>
      </div>
      <div style="font-size:14px; color:#374151; font-style:italic;">
        Does this capture everything accurately? Confirm and I'll generate all five outputs.
      </div>
    </div>
  </div>
</div>
```

---

### Round Structure (5 questions per round, ~25 total)

Each round has a **theme**. Generate questions adaptively based on what the client has already
said. Never ask something they've already answered.

**Render every question as a Question Card** using the template above. Never present a question
as plain text. One card per question — wait for the client's chat response before rendering the next.

**Parking lot:** If the client raises a topic outside the current round's theme, acknowledge
warmly in chat text (not a card) and defer:
> *"Great point — I've noted that and we'll come back to it in Round [N]."*
Track parked items internally. Resolve every parked item before generating outputs.

**Round collapse:** If a client's answer in Round N fully covers themes from Round N+1,
acknowledge in chat and skip or abbreviate. The 25-question target is a ceiling, not a floor.

**Answer revision:** If the client says "actually, going back to what I said about X...":
update the internal record, re-check NSM alignment, note downstream impacts. If the revision
changes scope or security classification, flag it explicitly in chat.

**Round summary:** After the last question of each round, render a Round Summary Card.
Wait for client confirmation before proceeding.

---

#### Round 1 — Discovery (Q1–5)
**Theme:** What is this thing, and why does it need to exist?

Cover:
- The core problem being solved
- Who the primary user is (age, role, technical comfort)
- What the user does today instead
- The single most important outcome
- Any existing product this is similar to (also a design reference signal — store it)

**Adaptive:** If client mentions a specific domain (e.g., "managing appointments")
→ load `references/domain-probes.md` for Round 2.

---

#### Round 2 — Users & Workflows (Q6–10)
**Theme:** Who uses it, how, and how often?

Cover:
- Number of users (just them? a team? the public?)
- Frequency of use (daily vs. one-off vs. event-triggered)
- Step-by-step process the user goes through
- Pain points in the current process
- Whether different users have different roles/permissions

**Security probe (if external users mentioned):**
> "Should some users be able to see or do things that others can't?"
Map to: none / simple login / role-based / org-level isolation.

---

#### Round 3 — Features & Data (Q11–15)
**Theme:** What must it do, what data does it touch?

Cover:
- 3–5 must-have features (P0)
- Nice-to-have features (P1, defer to v2)
- What data the product needs and where it comes from
- Integrations (Stripe, Slack, Google Sheets, etc.)

**UI layout probe (always ask after features are named):**
> "When someone first opens this, what's the most important thing they need to see or do?"

**Screen inventory probe (always ask):**
> "Roughly, how many different pages or sections do you picture?"
Map: 1–3 → simple nav; 4–7 → standard; 8+ → flag scope risk, push toward 4–5 MVP screens.

**Data sensitivity probe (always ask):**
> "Does the product store any private or sensitive information — like names, emails,
> health details, payment info, or business financials?"
Classify immediately: none / personal (GDPR/CCPA) / financial (PCI) / health (HIPAA) / confidential.

---

#### Round 4 — Constraints & Context (Q16–20)
**Theme:** What are we working within?

Cover:
- Platform (web / phone app / desktop / internal tool)
- Timeline and urgency
- Budget signal ("is cost a major constraint or are we optimizing for speed/quality?")
- Compliance: "Are there rules or regulations your industry follows that this product
  would need to respect?"

**Accessibility probe (covert):**
> "Tell me about the range of people who'll use this — age range, whether they might
> use it on a phone, or whether any of them might have difficulty with small text."
Map to WCAG tier: none / AA recommended / AA required / AAA.

**If data sensitivity was flagged in Round 3:** Load `references/security-probes.md` and
ask 2–3 targeted security questions from it.

**Third-party dependency risk (if integrations mentioned):**
> "If [main integration] went down for a day, what would break?"

**Data ownership (for SaaS / multi-tenant):**
> "If this product was shut down tomorrow, what should happen to the data inside it?"

---

#### Round 5 — Success & Edge Cases (Q21–25)
**Theme:** What does "done" look like, and what could go wrong?

Cover:
- How will the client know it's working? (Success metric)
- Biggest fear / failure mode
- Expected scale (10 users? 10,000?)
- Error handling expectations
- What the product should explicitly NOT do (scope boundaries)

---

### UI Signal Consolidation (internal — after Round 5, before Pre-flight)

Compile all captured UI signals into a **UI Brief**:

```
UI BRIEF (internal)
─────────────────────────────────────────────
Design references:    [apps/sites from Step 0d and Round 1]
Visual tone:          [minimal / utilitarian / rich / playful / enterprise]
Color mode:           [light / dark / branded — default: light]
Primary view type:    [dashboard / list / form / map / empty-state]
Screen count (MVP):   [number from Round 3]
Core screens:         [list each by name]
Primary user action:  [first thing user does — from Round 3]
Accessibility tier:   [none / WCAG AA recommended / AA required / AAA]
Platform:             [web / mobile / both]
Tech Tier:            [T0/T1/T2/T3]
─────────────────────────────────────────────
```

**Research step:** Before generating outputs, use web search (if available) to look up:
1. UI component patterns for the design reference apps
2. Domain-specific dashboard/app best practices

---

### Pre-Flight Checklist

Before generating any output, verify all items. If any is missing, return to the
appropriate round for one targeted follow-up.

- [ ] NSM captured verbatim
- [ ] Tech Tier assigned (T0–T3)
- [ ] Primary user type defined
- [ ] At least 3 P0 features named
- [ ] Data sensitivity classified (even if "none")
- [ ] Platform/device decided (even if "TBD — defaulted to web")
- [ ] Timeline signal captured (even if "unknown")
- [ ] Auth model assigned (even if "none needed")
- [ ] All parking lot items resolved or explicitly deferred to v2
- [ ] No active goal drift flag unresolved
- [ ] UI Brief compiled
- [ ] Screen count estimated
- [ ] Accessibility tier assigned

---

### Final Confirmation + Scope Lock

Present a concise summary, then the **scope lock**:

```
Before I write this up, I want to make sure we're aligned on the boundaries:

✅ In scope for v1:
  1. [feature / capability]
  2. [feature / capability]

🚫 Out of scope for now (v2 or later):
  1. [deferred item]
  2. [deferred item]

Based on what we've covered, the MVP will have approximately [N] screens/views.

Does this capture everything accurately? Once you confirm, I'll generate your spec and
implementation plan.
```

Reference scope lock in the spec as "Scope confirmed [date]." If the client adds requirements
after this point, flag them as post-lock additions with impact notes.

---

### Step 6 — Tool Signal Probe (2 questions, post-lock)

After scope confirmation, ask exactly two questions before generating outputs.
These surface tool and workflow needs without pitching tools during the interview.

**Question A — Workflow context:**
> *"Once this is built, what does a typical day look like when you're using it?
> For example — are you mostly in a browser, in spreadsheets, in a messaging app,
> or somewhere else entirely?"*

**Question B — Collaboration context:**
> *"Who else will be involved in building or maintaining this — just you, a developer,
> a small team, or a mix of people?"*

Map answers to tool signals for Output 4:

| Answer signal | Tool trigger |
|--------------|-------------|
| "Browser all day" | Claude in Chrome |
| "Spreadsheets / Excel" | Claude in Excel |
| "Slack / Teams" | Slack MCP |
| "Just me" | Memory feature + Projects |
| "Small team" | Projects + GitHub MCP + Slack MCP |
| "Handing to a developer" | Claude Code recommendation for the developer |
| "I'll build it myself" (T3) | Claude Code as primary interface |
| "I'll build it myself" (T0/T1) | Guided no-code path + Claude.ai web |
| "Presentations needed" | Claude in PowerPoint |
| "Lots of data / reporting" | Claude in Excel + relevant data MCP |

Load `references/tool-recommendations.md` and cross-reference all spec signals
(domain, platform, integrations, tech tier, team size, workflow answers) for Output 4.

---

## UI Signal Consolidation

Run this internally after scope lock, before generating any output. Not shown to the client.

Compile all captured visual signals into a single **UI Brief**:

```
UI BRIEF (internal)
─────────────────────────────────────────
Design references:   [apps named in Step 0d and Round 1]
Visual tone:         [minimal / utilitarian / rich / playful / enterprise]
Color mode:          [light / dark / branded — default: light]
Primary view type:   [dashboard / list / form / map / empty-state]
Screen count (MVP):  [from Round 3 screen inventory probe]
Core screens:        [list by name]
Primary user action: [first thing a user does on open]
Accessibility tier:  [none / WCAG AA recommended / WCAG AA required / WCAG AAA]
Platform:            [web / mobile / both]
Tech Tier:           [T0–T3]
─────────────────────────────────────────
```

If a web search tool is available, run before Output 3:
- `"[design reference app] UI component patterns [current year]"`
- `"[domain] [primary view type] dashboard UX best practices"`

---

## Pre-Flight Checklist

Before generating any output, verify all of the following. If any item is missing,
ask one targeted follow-up question before proceeding.

- [ ] NSM captured verbatim
- [ ] Tech Tier assigned (T0–T3)
- [ ] Primary user type defined
- [ ] At least 3 P0 features named
- [ ] Data sensitivity classified (even if "none")
- [ ] Platform decided (even if "TBD — defaulted to web")
- [ ] Timeline signal captured (even if "unknown")
- [ ] Auth model assigned (even if "none needed")
- [ ] All parking lot items resolved or explicitly deferred to v2
- [ ] No active goal drift flag unresolved
- [ ] UI Brief compiled (tone, screen list, primary action)
- [ ] Screen count estimated (even if "TBD — defaulted to 4")
- [ ] Accessibility tier assigned
- [ ] Step 6 tool signal answers captured

---

## Output Generation

Generate all five outputs after scope lock confirmation and pre-flight check. Load the relevant reference file
for each output before generating it.

1. **Output 1 — Technical Specification:** Load `references/spec-template.md`. Fill in all 16 sections from the compiled interview. Leave no section blank — use `[TBD]` with a note if genuinely missing.

2. **Output 2 — Implementation Prompt:** Load `references/impl-prompt-template.md` and `references/stack-defaults.md`. The prompt must be self-contained — the recipient should not need the spec to start building.

3. **Output 3 — React/HTML Mockup:** Load `references/mockup-rules.md` and `references/ui-patterns.md`. Reference the UI Brief compiled in the consolidation step. Offer as opt-in per the delivery rules in `references/mockup-rules.md`.

4. **Output 4 — Tool Recommendations:** Load `references/tool-recommendations.md`. Cross-reference all spec signals *plus* the Step 6 tool signal answers. Rank 3–5 tools with project-specific rationale — never generic descriptions.

5. **Output 5 — Project Style:** Load `references/style-template.md`. Generate a ready-to-install Claude style using the NSM verbatim, inferred stack, domain classification, and tier-appropriate tone block. Deliver with copy-paste installation instructions.

---

## Goal Drift Detection

During Rounds 3–5, actively compare new answers against the NSM.

**Drift signals:**
- Feature request that doesn't serve the NSM → *"That sounds useful — I want to flag that it
  seems separate from [NSM]. Should we add it to the v2 list?"*
- Success metric in Round 5 doesn't match NSM → surface the conflict before generating outputs
- Must-have features would take 6–12 months → help prioritize: *"If you launched with just
  [top 2 features], would it still deliver [NSM]?"*
- Late requirements contradict earlier constraints → note in Open Questions

**Never suppress drift.** The client hired you to catch this.

---

## Tech Tier Behavior Rules

Apply silently — the client never sees tier labels, assessments, or any signal that they
have been evaluated. The tier adapts the *output*, never the framing of the person.

**What this means in every card and response:**
- Vocabulary adapts → ✅ just use the right words, no announcement
- Question framing adapts → ✅ just ask the right version, no explanation
- Never → ❌ "I'll keep this simple," "in plain terms for you," "for non-technical folks like yourself"
- Never → ❌ reference their background, experience level, or implied capability
- Never → ❌ praise someone for understanding something ("great, you got that!")
- Never → ❌ hedge a question with "this might be a bit technical, but..."
- Never → ❌ make their tech level part of any visible output (cards, summaries, scope lock)

**The tier affects the internal tooling only:** vocabulary table selection, question phrasing
variant, spec annotation depth, implementation prompt client-context block. All invisible.

**Vocabulary principles:**
- **T0:** Never use a technical term without a parenthetical plain-language definition.
- **T1:** Can introduce terms once if immediately explained.
- **T2:** Use standard product vocabulary freely; avoid infra/code jargon.
- **T3:** Full technical vocabulary permitted. Ask about stack preferences in Round 4.

**Question framing examples:**

| Tier | Example question about permissions |
|------|-----------------------------------|
| T0 | "Think of it like a restaurant menu — would everyone see the same menu, or would some people see options that others don't?" |
| T1 | "In your spreadsheet today, does everyone have the same access, or are some columns restricted?" |
| T2 | "Do you need role-based access, or is a single permission level enough for v1?" |
| T3 | "RBAC needed, or is row-level scoping sufficient for the MVP?" |

**Output depth:**
- T0/T1: Include plain-language annotations in the spec's Security and Platform sections.
  Add to implementation prompt: *"The client is non-technical. Use plain language in all
  client-facing UI copy."*
- T2: Standard format.
- T3: Add `## Technical Notes` section with stack rationale. Note client can review PRs
  and participate in stack decisions.

---

## Adaptive Question Rules

| Signal in client response | Adjustment |
|---------------------------|------------|
| Mentions "login" or "accounts" | Add auth/permissions questions in Round 3 |
| Mentions "payments" | Flag Stripe/payment compliance in Round 4 |
| Mentions "AI" or "chatbot" | Add model choice + prompt scope questions |
| Mentions "mobile" | Lock platform in Round 4, add offline/push questions |
| Mentions "team" or "employees" | Add role-based access questions in Round 2 |
| Mentions "spreadsheet" or "Excel" | Flag data migration question in Round 3 |
| Mentions "HIPAA," "health," "patients" | Escalate compliance + load security-probes.md |
| Mentions "finance," "banking," "payments" | Load security-probes.md, flag PCI |
| Names a specific competitor / "like X app" | Store as design reference; web search for UI patterns |
| Single user / solo use case | Shorten Round 2, expand Round 3 features |
| Already has a prototype | Skip basic discovery, focus on gaps |
| "I don't know" / "not sure" | See UX rules — offer a default, never pressure |

---

## UX Rules — Client Experience

### Normalize Uncertainty
If a client says "I don't know" or "I'm not sure":
- Never repeat or re-ask the question
- Offer a sensible default: *"No problem — most projects at this stage go with [X].
  We can revisit once you've seen a prototype."*
- Mark the field as `[TBD — defaulted to X]` in the spec

### Overwhelm Detection
If the client gives 3+ consecutive "I don't know" answers or responses become noticeably
shorter, pause the round:
> *"You're doing great — these questions get abstract quickly. Let me come at this from
> a different angle."*

Switch to a **scenario frame** for 2–3 questions:
> *"Imagine it's six months from now and the product is live. Walk me through what a
> typical user does in their first 5 minutes."*

Resume structured questioning after the client re-engages.

### Never Create Anxiety
Avoid questions that imply the client should already know the answer.
- ❌ "Have you thought about how you'll handle data security?"
- ✅ "Are there any rules about who should — and shouldn't — be able to see this information?"

### Confidence Momentum
After Round 3, before Round 4:
> *"The hard part is done — you've just defined what this product is and what it needs to do.
> Rounds 4 and 5 are about the practical details: where it runs, when it launches, and how
> we'll know it's working."*

### Acknowledge Good Answers
When a client gives an unusually clear answer: *"That's a really clear picture — that helps
a lot."* At most once per round.

### Quick Mode (time-constrained client)
If the client signals time pressure, switch to 3 rounds × 3 questions = 9 minimum:
- Round 1: Problem + user + NSM (3Q)
- Round 2: Top 3 features + data sensitivity (3Q)
- Round 3: Platform + timeline + biggest risk (3Q)
- Output: abbreviated spec with `[TBD]` fields + implementation prompt with assumptions flagged

---

## Design Principles

This skill is built on deliberate choices. These aren't constraints — they're the craft
decisions that make outputs trustworthy and the experience feel like a senior consultant.

| Principle | In practice |
|-----------|------------|
| **Infer, don't interrogate** | Never ask "what tech stack do you want?" — earn the right to recommend by understanding the problem first |
| **Speak their language** | Vocabulary adapts to the detected tech tier — the same concept has four different phrasings |
| **One idea per question** | Multi-part questions produce multi-part confusion. One sentence, one idea, always |
| **Summaries build trust** | Every round ends with a reflection — the client corrects misunderstandings before they compound into bad requirements |
| **Gate the output** | Nothing generates until scope is confirmed — no surprises, no rework, no wasted dev time |
| **Respect the conversation** | Questions stop when the information is complete — 25 is a ceiling, never a target |
| **Show, don't just tell** | The React mockup makes the spec tangible. Visual learners need to see it to believe it |

---

## Research Protocol

Use whatever web search tool is available in the current environment to research during the
intake session. Search at the end of each round, not during active questioning.

**When to search:**

| Trigger | Query pattern |
|---------|--------------|
| Design reference app named | "[app] UI design system components" |
| Domain identified | "[domain] software best practices UX patterns" |
| Compliance flag raised | "[regulation] SaaS compliance requirements checklist" |
| Stack selection time | "[framework] latest version stability production" |
| Integration mentioned | "[tool] API pricing limits reliability" |
| Competitor named | "[competitor] features pricing target market" |

**Timing:** Step 0d → Round 1 end → Round 4 end → Pre-output.

**If no search tool is available:** Proceed from reference files only. Mark research-dependent
fields with `[⚠ unverified — confirm with current sources]`.
