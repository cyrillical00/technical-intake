# Domain Probes — Adaptive Follow-Up Questions

Load this file when a client's Round 1 answer signals a specific domain.
Use the relevant section to replace generic Round 2–3 questions with sharper,
domain-specific ones.

---

## E-Commerce / Marketplace

- Do you sell physical products, digital products, or services?
- Do you need to manage inventory, or is stock unlimited?
- How do you handle returns or refunds today?
- Will there be multiple sellers/vendors, or is it one store?
- Do you need tiered pricing, discount codes, or subscriptions?

---

## Scheduling / Booking

- What is being booked — appointments, resources, seats, or time slots?
- Can one user book multiple things at once?
- Do you need automated reminders (email/SMS) before the appointment?
- How far in advance can bookings be made? Is there a cancellation window?
- Do staff members have their own calendars, or is it one shared calendar?

---

## Internal Tools / Admin Dashboards

- Who inside the company uses this — one team, or multiple departments?
- Does it replace a spreadsheet, a legacy system, or a manual process?
- What actions can users take — just view data, or also edit/approve/reject?
- Does it need to connect to your existing software (CRM, ERP, HR system)?
- Is this for daily operations or executive reporting?

---

## Automation / Bots / Scripts

- What triggers the automation — a schedule, a user action, or an external event?
- What data goes in, and what should come out or happen as a result?
- What happens if the automation fails — does someone need to be alerted?
- How many times per day/week does this need to run?
- Is this replacing a manual task someone does today? If so, who and how often?

---

## Data Pipelines / Dashboards / Reporting

- Where does the data live today (spreadsheets, databases, third-party tools)?
- Who is the audience — internal team, leadership, or external clients?
- Does the data need to update in real time, daily, or on demand?
- What decisions will people make based on this dashboard?
- Do you need to export reports, or is on-screen viewing enough?

---

## SaaS / Multi-Tenant Products

- Will each customer have their own isolated data, or is it shared?
- How do customers sign up — self-serve, or do you onboard them manually?
- Do you need usage tiers or billing plans (free/pro/enterprise)?
- Who manages the product — just you, or do you need admin controls for staff?
- What happens to a customer's data if they cancel?

---

## Mobile Apps

- Is this iOS only, Android only, or both?
- Does the app need to work offline (without internet)?
- Does it need push notifications?
- Will the app need access to device features — camera, GPS, contacts, biometrics?
- Is this a consumer app (public App Store) or an internal employee app?

---

## AI / LLM-Powered Features

- What is the AI supposed to do — answer questions, generate content, classify data, or something else?
- What information does the AI need to work with — documents, user history, live data?
- How important is accuracy vs. speed? Is it OK if the AI is sometimes wrong?
- Will users see the AI's "reasoning" or just the final answer?
- Are there topics or outputs the AI should never produce?

---

## Healthcare / Compliance-Sensitive

- Does this handle patient data, financial records, or other sensitive personal information?
- Are there regulatory requirements you're aware of (HIPAA, GDPR, SOC 2, PCI)?
- Does your organization have an IT or legal team who will need to review this?
- Is this used by clinicians, administrators, or patients?
- What are the consequences if data is lost or accessed by the wrong person?

---

## CRM / Sales Tools

- Who are you tracking — leads, existing customers, partners, or all of the above?
- What's the most important action a salesperson needs to take in a day using this tool?
- Do you need to track the history of every interaction with a contact (calls, emails, notes)?
- Are there stages or steps a deal moves through before it closes?
- Does this need to connect to your email, calendar, or existing sales tools?

---

## EdTech / Learning Platforms

- Who is learning — students, employees, general public, or a specific professional group?
- Is the content created by you, by instructors, or by the learners themselves?
- Do learners need to complete things in a specific order, or can they go at their own pace?
- Do you need to track progress, issue certificates, or report completion to someone?
- Is there a live/synchronous component (video calls, live sessions) or is it all self-paced?

---

## FinTech / Financial Tools

- Are you moving money, tracking money, or helping people make decisions about money?
- Who are the users — consumers, businesses, or financial professionals?
- Does this need to connect to bank accounts, credit cards, or financial institutions?
- Are there transaction limits, approval workflows, or audit requirements?
- What's the regulatory environment — are you aware of any licenses or registrations required?

---

## Real Estate / Property Management

- Are you managing properties you own, properties for clients, or helping people find properties?
- Who are the key people involved — landlords, tenants, agents, buyers, sellers?
- What's the most painful part of the current process — paperwork, communication, payments, scheduling?
- Do you need to handle lease agreements, maintenance requests, or rent collection?
- Is this for residential, commercial, or both?

---

## Community / Social Platforms

- Is this a private community (invite-only) or open to the public?
- What do members do here — discuss topics, share content, find other members, attend events?
- How do you keep the community safe from spam, abuse, or inappropriate content?
- Do members have profiles, or is this more anonymous?
- Is monetization part of the plan — memberships, paid content, sponsorships?

---

## IoT / Hardware-Adjacent Products

- What is the physical device or hardware this product connects to or controls?
- Does the software need to work when there's no internet connection (offline / local mode)?
- How often does the device send data — constantly, every few seconds, or on specific events?
- Who sets up the device — a technical installer, or an end user with no technical background?
- If the device loses connection or malfunctions, what should the software do?

*Stack signal: IoT projects typically require MQTT or WebSocket for device communication,
edge-capable storage (SQLite / InfluxDB), and a separate device management layer. Flag in spec.*

---

## Legacy System Modernization

- What does the current system look like — how old is it, and what does it run on?
- What's the main reason you want to replace or modernize it — speed, features, cost, or reliability?
- Is there data in the old system that needs to be migrated into the new one?
- Who currently maintains the old system — an internal team, an outside vendor, or no one?
- Does the new system need to run alongside the old one for a transition period, or is it a
  clean cutover?

*Risk flags to check for this domain: data migration complexity, undocumented business logic
baked into the old system, stakeholder resistance to change, compliance requirements that
the old system was already certified for. Load `references/security-probes.md` risk patterns.*
