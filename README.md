# technical-intake

> A Claude skill that turns a vague software idea into a developer-ready specification — in one conversation.

## What It Does

Runs a structured 25-question adaptive interview with a non-technical client, silently calibrates to their technical literacy, and produces five production-ready outputs:

1. **Technical Specification** — markdown doc with screen inventory + user flow map
2. **Implementation Prompt** — paste-ready LLM code-gen prompt
3. **React/HTML UI Mockup** — working visual skeleton of the primary user flow
4. **Tool Recommendations** — personalised Claude ecosystem setup
5. **Project Style** — custom Claude style tuned to the project's communication needs

**Domains:** Web apps / SaaS · Mobile · Automation & bots · Data pipelines & dashboards

---

## Repo Structure

```
SKILL.md                          # Main skill — execution protocol, card rendering, UX rules
references/
  domain-probes.md                # Domain-specific deep-dive questions (e-commerce, SaaS, etc.)
  stack-defaults.md               # Tech stack inference rules by domain + tier
  security-probes.md              # Compliance flags, risk pattern library (HIPAA, PCI, SOC 2)
  ui-patterns.md                  # Design reference mappings (Notion, Linear, Stripe, etc.)
  spec-template.md                # Output 1 — Technical Specification template
  impl-prompt-template.md         # Output 2 — Implementation Prompt template
  mockup-rules.md                 # Output 3 — React/HTML Mockup rendering rules
  tool-recommendations.md         # Output 4 — Claude tool recommendation logic
  style-template.md               # Output 5 — Project Style generation template
  cache-strategy.md               # Developer reference — API caching for programmatic use
```

---

## How to Use

### In Claude Desktop (MCP / Skills)

Install as a skill via your Claude Desktop config. The skill auto-triggers on phrases like:
- "I want to build something"
- "help me scope this"
- "my client needs a build plan"
- "I have an app idea"

### Manual

Paste the contents of `SKILL.md` as a system prompt. Load reference files on demand as the interview progresses (the skill's Reference Files table tells you when to load each one).

---

## Design Philosophy

| Principle | In practice |
|-----------|------------|
| Infer, don't interrogate | Never ask "what stack?" — earn the recommendation |
| Speak their language | 4 vocabulary tiers, silently applied |
| One idea per question | Multi-part questions = multi-part confusion |
| Gate the output | Nothing generates until scope is locked |
| Summaries build trust | Every round ends with a client-correctable reflection |

---

## Author

Oleg Strutsovski — [cyrillical00](https://github.com/cyrillical00)
