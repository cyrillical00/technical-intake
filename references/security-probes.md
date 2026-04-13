# Security Probes — Deep-Dive Questions for Flagged Projects

Load this file when any of the following flags were raised during the interview:
- Data sensitivity: personal / financial / health / confidential
- Auth requirement: role-based or above
- Compliance: any flag raised
- Breach consequence: high or critical

---

## Auth & Access Control Probes

- Does every user log in with their own account, or do some people share a login?
- Are there users who should only see their own data, not everyone else's?
- Do some users need to approve or reject actions taken by others?
- Should there be an "admin" who can manage all users and see everything?
- If someone's account is compromised, how quickly does that need to be detected and stopped?

**Map answers to:**
- No auth → public tool
- Single login → basic session auth
- Per-user data isolation → row-level security (Supabase RLS / Postgres RLS)
- Approval workflows → role-based access control (RBAC)
- Admin panel → separate admin role with elevated permissions
- Compromise detection → session invalidation + audit log requirement

---

## Data Storage & Retention Probes

- How long does the product need to keep data — days, years, or forever?
- Is there a situation where a user should be able to delete all their data permanently?
- Does data ever need to be archived or exported for legal/accounting purposes?
- If the product stores files (documents, images, etc.), how large could the total storage get?
- Is there any data that should never leave your country or region?

**Flag in spec if:**
- "Delete all data" required → right-to-erasure (GDPR Article 17) flag
- Legal/accounting archive → immutable audit log requirement
- Data residency mentioned → region-locked hosting flag (EU: GDPR, health: HIPAA BAA)

---

## Threat Model Probes (ask if breach consequence is high/critical)

- Who would be most motivated to access this data without permission?
  (Competitors? Disgruntled employees? External attackers? Curious users?)
- Is there any action in this product that, if done maliciously, would cause serious harm?
  (e.g., deleting all records, sending messages as someone else, approving fraudulent transactions)
- Has your organization ever experienced a data breach or security incident?
- Do you have any existing security policies (password requirements, VPN access, etc.)?
- Will this product be accessed from personal devices, company devices, or both?

**Translate to spec notes:**
- External attackers → pen-test recommendation, rate limiting, WAF
- Insider threat → audit logging, principle of least privilege
- Prior breach → heightened security posture, security review gate before launch
- Personal devices → MDM consideration, session timeout requirements

---

## Compliance Quick-Reference

| Regulation | Triggers | Key Requirements | Stack Impact |
|------------|----------|-----------------|-------------|
| GDPR | Any EU user data | Consent, right to erasure, DPA | EU hosting, cookie consent |
| HIPAA | US health/patient data | PHI encryption, BAA agreements | HIPAA-eligible hosting (AWS/GCP) |
| PCI-DSS | Storing card data | Don't store raw card numbers | Use Stripe/Braintree (offloads scope) |
| SOC 2 | B2B SaaS enterprise sales | Audit logs, access controls, incident response | Logging + monitoring infrastructure |
| CCPA | CA residents' personal data | Right to know + delete | Similar to GDPR, US-focused |
| FERPA | US student educational data | Parental/student consent | Consent workflows |

**Default recommendation:** "Don't store what you don't need." Offload payment processing to
Stripe. Offload identity to Clerk/Auth0. These choices eliminate most compliance scope.

---

# Risk Pattern Library — Common Project Failure Modes

Load this section when generating Risk Flags in the spec. Cross-reference client answers
against these patterns and flag any matches.

---

## Scope Creep Signals

| Client Said | Risk | Recommended Response |
|-------------|------|---------------------|
| "And it should also..." (5+ times) | Feature bloat — v1 will never ship | Hard prioritization: "Which 3 are non-negotiable?" |
| "It needs to do everything [Competitor] does" | Undefined scope ceiling | Ask: "What's the one thing [Competitor] does badly that you'd do better?" |
| "We'll figure out the details later" (for core features) | Ambiguity will cause rework | Mark as Open Question, don't spec until answered |
| "It should be flexible enough to handle anything" | Over-engineering trap | Pin down the top 3 actual use cases |

---

## Timeline Mismatch Signals

| Situation | Realistic Timeline | Flag If |
|-----------|------------------|---------| 
| Full SaaS with auth + payments + admin | 3–6 months (team) | Client expects < 6 weeks |
| Mobile app (iOS + Android) | 3–5 months | Client expects < 2 months |
| Simple internal tool / CRUD app | 2–6 weeks | Client expects < 1 week |
| Data pipeline + dashboard | 2–8 weeks | Client expects "by end of week" |
| AI-powered product with custom training | 2–4 months | Client expects "just plug in ChatGPT" |

When flagged: note in Risk Flags as `[timeline: mismatch — recommend scoping conversation]`

---

## Ownership & Maintenance Blindspots

| Missing Answer | Risk |
|---------------|------|
| No one named as product owner | Decisions will stall; spec will drift |
| "The developers will figure it out" for business logic | Core requirements will be misbuilt |
| No plan for who updates content / data after launch | Product goes stale immediately |
| No budget for hosting/maintenance post-launch | Product gets abandoned at first invoice |
| "We'll add security later" | Security is architecturally expensive to retrofit |

---

## Technical Assumption Traps

| Client Assumption | Reality Check |
|-------------------|--------------| 
| "It should sync in real time" | Real-time adds significant complexity + cost. Is 5-min delay OK? |
| "It needs to handle millions of users" | Most v1 products never hit 10k users. Over-engineering delays launch. |
| "Just use AI for that" | AI adds latency, cost, and unpredictability. Is rules-based logic sufficient? |
| "It should work on every device" | "Every device" = 3–4x more testing surface. Prioritize by actual user devices. |
| "We already have the data" | Data is rarely clean. Budget for data migration/normalization. |

---

## Accessibility Tier Classification

Assign during Round 4 accessibility probe. Record in spec §7.

| Audience signal | WCAG tier | Spec entry |
|----------------|-----------|-----------| 
| General consumer, broad public | 2.1 AA recommended | "WCAG 2.1 AA — recommended" |
| Government, healthcare, education | 2.1 AA required | "WCAG 2.1 AA — required (regulatory)" |
| Elderly users explicitly mentioned | 2.1 AA required | "WCAG 2.1 AA — required (user need)" |
| Internal / tech-savvy team only | None / best-effort | "Accessibility: internal tool, best-effort" |
| Disabled users explicitly mentioned | 2.1 AAA | "WCAG 2.1 AAA — required" |

AA compliance adds ~10–20% to dev time. AAA adds ~30–40%. Flag in Risk Flags if required on lean budget.

---

## UI-Specific Risk Patterns

| Risk | Trigger signal | Spec flag |
|------|---------------|-----------| 
| Missing empty states | Any list/table feature, no "no data" design discussed | `[ui: empty states undefined — required for all list views]` |
| Missing loading states | Any async fetch or form submit | `[ui: loading states undefined — required for all async operations]` |
| Mobile blindspot | Platform "web" but users likely mobile | `[ui: mobile responsiveness unconfirmed — recommend mobile-first]` |
| Accessibility gap | WCAG required but no accessibility budget | `[ui: accessibility requirement vs budget conflict — flag for scoping]` |
| Visual scope creep | Client described 8+ screens on lean budget | `[scope/ui: screen count exceeds lean budget — prioritize 4 core screens]` |
| No design system | Multiple contributors, no component consistency plan | `[ui: design consistency risk — recommend shadcn/ui or Tailwind component set]` |
| Dark mode assumed | Client mentioned "dark" without confirmation | `[ui: dark mode requires 2× color system work — confirm if required for v1]` |
