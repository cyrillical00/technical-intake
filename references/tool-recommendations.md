# Tool Recommendations — Claude Ecosystem by Project Signal

Load this file when generating Output 4 (Tool Recommendations) after scope lock.
Cross-reference spec signals against the trigger columns to produce tailored recommendations.

---

## Claude Installations

| Tool | What it is | Best for | Install |
|------|-----------|----------|---------| 
| **Claude.ai** (web/mobile) | Primary interface, Projects, memory | Any project — daily driver | claude.ai |
| **Claude Code** | Terminal agent, full codebase context | Any project where code will be written | `npm install -g @anthropic-ai/claude-code` |
| **Claude in Chrome** | Browsing agent — reads pages, fills forms | Competitive research, scraping, web testing | Chrome Web Store |
| **Claude in Excel** | Spreadsheet agent | Data pipelines, financial tools, reporting | Microsoft AppSource |
| **Claude in PowerPoint** | Slide generation | Pitch decks, client presentations | Microsoft AppSource |

---

## Claude.ai Features to Enable

| Feature | Trigger signal | Why it helps |
|---------|---------------|-------------|
| **Projects** | Any multi-session build (most projects) | Persistent context — Claude remembers the spec across sessions |
| **Memory** | Solo founder / single developer | Remembers your preferences, stack choices, and coding patterns |
| **Web search** | Anything requiring current data (prices, APIs, compliance) | Real-time research without leaving the conversation |
| **Deep Research** | Market research, competitive analysis, compliance deep-dives | Multi-source synthesis in one shot |
| **Extended thinking** | Complex architecture decisions, security design | Slower but higher-quality reasoning for hard problems |

---

## MCP Connectors — by Project Domain

### Always Useful
| Connector | Trigger | What it unlocks |
|-----------|---------|----------------|
| **GitHub MCP** | Any project with code | Create repos, push files, open PRs, manage issues — all from Claude |
| **Google Drive MCP** | Any project with documents | Read/write specs, share with stakeholders, version history |
| **Gmail MCP** | Client communication | Draft client updates, send spec summaries, track approvals |
| **Google Calendar MCP** | Scheduling, meetings, deadlines | Book review sessions, set milestone reminders |

### By Domain
| Domain | Connector | Why |
|--------|-----------|-----|
| **SaaS / payments** | Stripe MCP | Query customers, subscriptions, invoices from Claude |
| **Team / collaboration** | Slack MCP | Post spec updates, create channels, search history |
| **Content / knowledge** | Notion MCP | Write specs directly to Notion, maintain living docs |
| **E-commerce** | Shopify MCP (if available) | Query products, orders, customers |
| **CRM / sales** | HubSpot MCP (if available) | Access contacts, deals, pipeline from Claude |
| **Data / analytics** | Airtable MCP (if available) | Query and update records, build data models |
| **Internal tools** | Postgres/Supabase MCP | Query production DB directly from Claude for debugging |
| **DevOps / cloud** | AWS MCP / GCP MCP | Infrastructure queries, deployment status |

---

## Recommended Claude Project Setup

When a project scope is confirmed, suggest this Project configuration in Claude.ai:

```
Project Name:    [Product Name] — Dev
Project Instructions (paste into Project Settings):

You are the technical lead for [Product Name].

NORTH STAR: [NSM verbatim]
TECH TIER: [T0/T1/T2/T3]
STACK: [inferred stack from spec]
PLATFORM: [web / mobile / desktop]
CURRENT PHASE: [discovery / planning / build / testing / launch]

When answering questions about this project:
- Always align suggestions to the North Star
- Flag scope creep explicitly
- Use [vocabulary tier] language when communicating with the client
- Reference the spec in responses when relevant

Attached files: [spec document, implementation prompt]
```

---

## Recommendation Scoring Matrix

Cross-reference spec fields to generate the final ranked list:

| Spec signal | → Recommend |
|------------|------------|
| Code will be written | Claude Code (always P0) |
| Multi-session project | Claude.ai Projects + Memory |
| GitHub repo needed | GitHub MCP |
| Documents shared with client | Google Drive MCP + Gmail MCP |
| Payments in scope | Stripe MCP |
| Team of 2+ people | Slack MCP |
| Notion user (T1–T2) | Notion MCP |
| Data-heavy / analytics | Claude in Excel + Airtable MCP |
| Competitive research needed | Claude in Chrome + Web search |
| Complex architecture decisions | Extended thinking |
| Client presentations needed | Claude in PowerPoint |
| Database queries during dev | Supabase/Postgres MCP |
| T3 client (developer) | Claude Code as primary interface |
| T0/T1 client | Claude.ai web (simplest entry point) |
| Compliance research needed | Deep Research mode |

---

## Tier-Adapted Recommendation Language

Adjust how tools are described based on Tech Tier:

**T0 — Plain language**
> "I'd suggest using Claude's Projects feature — it lets Claude remember everything about your
> project between conversations, so you don't have to repeat yourself each time."

**T1 — Product-familiar**
> "Claude Projects keeps your spec and context persistent across sessions. Pair it with
> Google Drive so your documents stay in sync."

**T2 — Tech-adjacent**
> "Set up a Claude Project with the spec attached. If you're using GitHub, the GitHub MCP
> connector lets Claude push code and manage issues directly."

**T3 — Technical**
> "Claude Code for implementation. GitHub MCP for repo management. Supabase MCP if you
> want live DB access during development. Projects for context persistence across sessions."
