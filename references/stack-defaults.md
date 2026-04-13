# Stack Defaults — Suggested Technology by Domain

Use this file to populate the "Suggested Stack" section of the Implementation Prompt.
Infer from the spec — do not ask the client to choose a stack.

Priority logic:
1. If client has existing tech → match it
2. If timeline is urgent → lean toward no-code/low-code options
3. If scale is unknown/small → prefer managed/serverless
4. If compliance-sensitive → prefer established platforms with audit trails

---

## Web App / SaaS

| Need | Lean/Fast | Standard | Enterprise |
|------|-----------|----------|------------|
| Frontend | Next.js + Tailwind | Next.js + Tailwind | Next.js + design system |
| Backend | Supabase (BaaS) | Node.js/Express + PostgreSQL | NestJS + PostgreSQL |
| Auth | Supabase Auth / Clerk | Clerk / Auth0 | Auth0 / custom |
| Payments | Stripe (built-in) | Stripe | Stripe + custom billing |
| Hosting | Vercel + Supabase | Vercel + Railway | AWS / GCP |
| CMS / Admin | Supabase dashboard | Retool / Directus | Custom admin panel |

---

## Mobile App

| Need | Lean/Fast | Standard | Native Only |
|------|-----------|----------|-------------|
| Framework | React Native (Expo) | React Native | Swift (iOS) / Kotlin (Android) |
| Backend | Supabase | Firebase | Custom API |
| Auth | Supabase Auth | Firebase Auth | Custom |
| Offline | MMKV + sync queue | WatermelonDB | Core Data / Room |
| Push notifications | Expo Notifications | Firebase FCM | APNs / FCM direct |

---

## Automation / Bots / Scripts

| Trigger type | Lean/Fast | Programmable |
|--------------|-----------|--------------|
| No-code workflow | Make.com / n8n | n8n (self-hosted) |
| Scheduled script | Python + cron / GitHub Actions | Python + Celery |
| Event-driven | Zapier / Make webhook | Python + FastAPI webhook |
| Browser automation | Playwright (Python) | Playwright + Browserless |
| Internal Slack bot | Slack Bolt (Python) | Slack Bolt + custom backend |
| AI-powered pipeline | LangChain + Anthropic API | Custom agent loop |

---

## Data Pipeline / Dashboard

| Need | Lean/Fast | Standard | Scale |
|------|-----------|----------|-------|
| Visualization | Streamlit | Metabase / Superset | Grafana / Redash |
| ETL | Python + pandas | dbt + Airbyte | Airflow + dbt |
| Database | SQLite / Supabase | PostgreSQL | BigQuery / Snowflake |
| Scheduling | GitHub Actions / cron | Prefect / Dagster | Airflow |
| Embedding in existing app | Observable / Recharts | Chart.js / Recharts | D3.js |

---

## AI / LLM Features

| Use case | Recommended approach |
|----------|---------------------|
| Q&A over documents | RAG pipeline: Anthropic API + pgvector or Pinecone |
| Content generation | Anthropic API (Claude Sonnet) with structured prompts |
| Classification / tagging | Claude API with JSON output mode |
| Chatbot / assistant | Claude API + conversation history management |
| Autonomous agent | Claude API + tool use |
| Fine-tuned behavior | System prompt engineering first; fine-tune only if needed |

---

## Auth Patterns by User Type

| Scenario | Recommendation |
|----------|---------------|
| Single user / personal tool | No auth needed |
| Small team (< 20 people) | Clerk or Supabase Auth (magic link) |
| Consumer app | Google OAuth + email/password via Clerk |
| Enterprise / SSO required | Auth0 or WorkOS |
| Healthcare / compliance | Auth0 + MFA enforcement |

---

## Security-Aware Stack Variants

Use this section when the spec has a security posture of **standard** or **hardened**.
These replace or augment the default stack choices above.

### Auth — Security-Tiered Recommendations

| Security posture | Auth recommendation | Why |
|-----------------|-------------------|-----|
| Minimal (T0 client, single user) | No auth or magic link only | Simplest path, no password management |
| Standard | Clerk (hosted) | MFA built-in, breach monitoring, easy setup |
| Hardened (sensitive data, enterprise) | Auth0 + MFA enforcement | Audit logs, anomaly detection, SAML/SSO |
| Healthcare (HIPAA) | Auth0 on HIPAA-eligible plan + BAA | Contractual compliance coverage |
| Financial (PCI) | Auth0 or in-house — NO card data storage | Stripe/Braintree handles card scope |

---

### Hosting — Compliance-Aware Choices

| Flag | Hosting requirement | Avoid |
|------|-------------------|-------|
| GDPR (EU data) | EU-region hosting (AWS eu-west, GCP europe-west) | Default US-only regions |
| HIPAA | AWS/GCP with signed BAA | Shared hosting, Vercel for PHI storage |
| SOC 2 target | AWS / GCP / Azure (all have SOC 2 certs) | Serverless platforms without audit logs |
| High breach consequence | Private VPC, no public DB exposure | Supabase public anon key for sensitive tables |
| Standard | Vercel + Supabase | Fine for most projects |

---

### Secrets Management by Project Type

| Project type | Secret storage recommendation |
|-------------|------------------------------|
| Web app | Environment variables in Vercel / Railway / Render |
| Automation / scripts | `.env` file locally; GitHub Actions secrets for CI |
| Self-hosted | HashiCorp Vault or AWS Secrets Manager |
| Mobile app | Never in app bundle — fetch from secure backend endpoint at runtime |
| Any project with T3 client | Note: rotate credentials on a schedule; revoke on team member departure |

---

### Database — Security Posture Variants

| Need | Recommendation |
|------|---------------|
| Row-level security (multi-tenant) | Supabase (Postgres RLS built-in) |
| Audit log required | Postgres with pgaudit extension, or Supabase + custom trigger |
| Encryption at rest required | AWS RDS with encryption enabled, or Supabase (encrypted by default) |
| HIPAA PHI storage | AWS RDS on HIPAA-eligible account with BAA |
| No auth needed (internal read-only) | SQLite or Postgres without RLS fine |

| Budget posture | Backend | Database | Frontend |
|----------------|---------|----------|----------|
| Lean (free tier) | Render free / Railway starter | Supabase free tier | Vercel free |
| Standard ($50–200/mo) | Railway / Render paid | Supabase pro / Neon | Vercel pro |
| Enterprise ($500+/mo) | AWS ECS / GCP Cloud Run | RDS / Cloud SQL | Cloudflare / AWS |

---

## UI Component Stack by Project Type

| Project type | Component library | Styling | Charts | Forms |
|-------------|------------------|---------|--------|-------|
| Web SaaS / dashboard | shadcn/ui | Tailwind CSS | recharts or tremor | react-hook-form + zod |
| Internal tool / admin | shadcn/ui + Tanstack Table | Tailwind CSS | recharts | react-hook-form |
| Consumer web app | shadcn/ui or plain Tailwind | Tailwind CSS | recharts | react-hook-form |
| Mobile (React Native) | NativeWind + React Native Paper | NativeWind | Victory Native | react-hook-form |
| Data pipeline / reporting | Streamlit components | Streamlit built-in | Plotly / Altair | Streamlit widgets |

**Mobile-first defaults for web:**
- Viewport target: 375px (iPhone SE) as baseline
- Tap targets: minimum 44×44px (`min-h-[44px] min-w-[44px]`)
- Font size floor: 16px body — below 16px causes iOS auto-zoom on input focus
- Breakpoints: `sm:` 640px, `md:` 768px, `lg:` 1024px
