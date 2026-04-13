# Output 1 — Technical Specification Template

Load this file when generating Output 1 after scope lock confirmation.
Fill in every section from the compiled interview data. Leave no section blank —
use `[TBD]` with a note if information is genuinely missing.

---

## Template

```markdown
# [Project Name] — Technical Specification
Generated: [date]

## 1. Project Summary
[2–3 sentence plain-English summary of what this is and why it exists]

## 2. Problem Statement
[The core problem, from the client's perspective]

## 3. Target Users
| User Type | Description | Technical Level | Frequency of Use |
|-----------|-------------|-----------------|-------------------|
| ...       | ...         | ...             | ...               |

## 4. Core Features (MVP)
| Priority | Feature | Description | Notes |
|----------|---------|-------------|-------|
| P0       | ...     | ...         | ...   |
| P1       | ...     | ...         | ...   |

## 5. Out of Scope (v1)
- [list]

## 6. Data & Integrations
| Data Source | Type | Direction | Notes |
|-------------|------|-----------|-------|
| ...         | ...  | In/Out    | ...   |

## 7. Platform & Constraints
- Platform: [web / mobile / desktop / internal]
- Devices: [...]
- Compliance requirements: [...]
- Accessibility tier: [none / WCAG 2.1 AA recommended / WCAG 2.1 AA required / WCAG 2.1 AAA]
- Budget posture: [lean / standard / enterprise]
- Timeline: [...]

## 8. UI Design Brief
| Dimension | Value | Source |
|-----------|-------|--------|
| Visual tone | [minimal / utilitarian / rich / playful / enterprise] | Step 0d |
| Color mode | [light / dark / branded] | Step 0d |
| Design references | [app names from Step 0d + Round 1] | Interview |
| Primary view type | [dashboard / list / form / map / empty-state] | Round 3 |
| Screen count (MVP) | [number] | Round 3 |
| Primary user action | [first action on open] | Round 3 |

## 9. Screen Inventory
| # | Screen Name | Purpose | Primary Components | Auth Required |
|---|------------|---------|-------------------|--------------|
| 1 | [e.g., Dashboard] | [what it shows/does] | [cards, charts, table] | [yes/no] |
| 2 | [e.g., Create Item] | ... | [form, dropzone] | ... |
| 3 | ... | ... | ... | ... |

**User Flow Map** (primary path):
```
[Entry point] → [Screen 1: action] → [Screen 2: outcome] → [Screen 3: completion]
```
Include branch paths for: error states / empty states / logged-out users (if auth present)

## 10. Security Profile
| Dimension | Classification | Notes |
|-----------|---------------|-------|
| Data sensitivity | [public / personal / financial / health / confidential] | ... |
| Auth requirement | [none / simple login / role-based / org-isolated / SSO] | ... |
| Compliance flags | [none / GDPR / HIPAA / PCI-DSS / SOC 2 / other] | ... |
| Access scope | [single user / team / customers / public] | ... |
| Breach consequence | [low / medium / high / critical] | ... |

**Recommended security posture:** [minimal / standard / hardened]

## 11. North Star Metric & v1 vs Vision
- **North Star:** [the single outcome that defines success, verbatim from client]
- **v1 (MVP):** [the smallest version that delivers the North Star]
- **Vision (v2+):** [what it grows into — not in scope now]

## 12. Business Outcome & ROI Signal
*Why this investment makes sense — even if no formal ROI was calculated.*

| Dimension | Current State | Target State | Proxy Metric |
|-----------|-------------|-------------|-------------|
| Time cost | [hours/week on manual process] | [hours/week with product] | time saved × hourly rate |
| Error rate | [current failure frequency] | [acceptable error rate] | cost per error × frequency |
| Revenue / growth | [what's blocked today] | [what becomes possible] | [estimated upside] |
| Customer experience | [current pain point] | [target experience] | [NPS / churn / conversion proxy] |

**Note:** Fill in only the rows the client can speak to. Leave others blank.

## 13. Success Criteria
- [measurable outcome 1]
- [measurable outcome 2]

## 14. Risk Flags
- [potential issue 1 — tag as: scope / technical / security / compliance / resource / ui]
- [potential issue 2]

Cross-reference against the Risk Pattern Library in `references/security-probes.md` before
finalising this section.

## 15. Open Questions
- [anything still ambiguous after the interview]

## 16. Scope Lock Record
- Confirmed: [date / "pending client confirmation"]
- Screen count agreed: [N screens for v1]
- In scope for v1: [list]
- Explicitly deferred: [list]
- Post-lock additions (if any): [list with impact notes]
```

---

## Tech Tier Adjustments

- **T0/T1:** Include a plain-language "What this means" annotation under §7 (Platform),
  §8 (UI Design Brief), and §10 (Security Profile). Replace abbreviations with full names.
- **T2:** Standard spec format.
- **T3:** Add a `## Technical Notes` appendix with stack rationale and architecture considerations.
