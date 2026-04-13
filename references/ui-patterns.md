# UI Patterns — Domain Conventions, Design References, Component Defaults

Load this file before generating Output 3 (React mockup) and when design references
are captured in Step 0d or Round 1.

---

## Design Reference → Component Convention Map

When a client names a reference app, use this table to infer UI conventions their
*users already know* — meaning the mockup should feel familiar to them.

| Reference App | Layout convention | Nav pattern | Data density | Tone | Color defaults |
|--------------|-------------------|-------------|-------------|------|----------------|
| **Notion** | Block-based, no chrome | Left sidebar, icon nav | Medium | Minimal | White bg, gray text, subtle accent |
| **Linear** | List/detail split view | Left sidebar with keyboard nav | High | Utilitarian | Dark or light, blue accent, tight padding |
| **Stripe** | Dashboard + detail drilldown | Top nav + left sidebar | High | Enterprise-clean | White, slate gray, #635BFF purple accent |
| **Figma** | Canvas + panels | Top toolbar + side panels | Very high | Utilitarian | Dark chrome, blue selection |
| **Airbnb** | Card-based browse + detail | Top search bar, minimal nav | Low-medium | Rich, warm | White, coral accent, generous whitespace |
| **Shopify** | Admin dashboard, polaris style | Left nav, breadcrumbs | High | Enterprise utilitarian | White, green accent, Polaris components |
| **Calendly** | Availability grid + booking flow | Minimal, wizard-style | Low | Clean, consumer | White, blue accent, progress steps |
| **Slack** | Channel list + message thread | Left sidebar channels | Very high | Utilitarian | Dark or light, purple/green online indicators |
| **Trello** | Kanban board | Top bar only | Medium-visual | Playful-utilitarian | White cards, color labels, drag handles |
| **Airtable** | Table/grid primary | Left sidebar, view switcher | Very high | Power-tool | White, yellow/orange accent, dense rows |
| **Duolingo** | Progress-driven, gamified | Bottom tab (mobile) | Low | Playful | Green, rounded, celebration states |
| **Robinhood** | Chart primary, minimal | Bottom tab | Medium | Rich, dark | Black bg, green/red data, thin type |

---

## Domain → Primary View Type

| Domain | Primary view | Secondary views | Key components |
|--------|-------------|-----------------|----------------|
| E-Commerce | Product grid / search results | Product detail, Cart, Checkout | ProductCard, FilterSidebar, CartDrawer |
| Scheduling | Calendar / availability grid | Booking confirmation, Event detail | CalendarGrid, TimeSlotPicker, ConfirmationCard |
| Internal tool / admin | Data table | Record detail, Create/Edit form | DataTable, StatCards, FilterBar, FormModal |
| SaaS dashboard | KPI metrics + chart | Data table, Settings, Onboarding | MetricCard, LineChart, SidebarNav |
| CRM / Sales | Lead pipeline (kanban or list) | Contact detail, Activity log | KanbanBoard or LeadTable, ContactCard, Timeline |
| Automation / workflow | Flow builder or run log | Config form, Error log | StepList, RunStatusBadge, LogViewer |
| Data pipeline / reporting | Chart + filter dashboard | Table drilldown, Export modal | AreaChart, FilterBar, DataTable, ExportButton |
| Mobile app | Feed or home screen | Detail view, Profile, Settings | FeedCard, BottomTabNav, FAB (floating action button) |
| Community / social | Post feed or member grid | Thread/detail view, Profile | PostCard, CommentThread, MemberAvatar |
| EdTech / learning | Course list or lesson player | Progress tracker, Quiz | LessonCard, ProgressBar, QuizBlock |
| Real estate | Property grid or map | Property detail, Inquiry form | PropertyCard, MapView, InquiryModal |
| FinTech | Account overview + transactions | Transfer flow, Statement | BalanceCard, TransactionList, TransferWizard |

---

## Visual Tone → Tailwind Design System Defaults

Apply these defaults in the React mockup unless design references override them.

### Minimal (Notion-like)
```
bg-white text-gray-900
font: Inter or system-ui
borders: border border-gray-200
radius: rounded-lg
shadow: shadow-sm
spacing: p-4 gap-4
accent: text-gray-900 / bg-gray-100 hover
empty state: text-gray-400 italic
```

### Utilitarian (Linear / Airtable-like)
```
bg-gray-50 text-gray-800
font: Inter, monospace for data
borders: border-b border-gray-300 (rows), no card borders
radius: rounded or rounded-md
shadow: none
spacing: p-3 gap-2 (tighter)
accent: bg-blue-600 text-white
data badges: bg-gray-100 text-gray-700 text-xs font-mono
```

### Enterprise (Stripe / Shopify-like)
```
bg-white text-gray-700
font: Inter
borders: border border-gray-200, divide-y divide-gray-100
radius: rounded-md
shadow: shadow (subtle)
spacing: p-5 gap-5
accent: bg-indigo-600 / bg-violet-600 text-white
stat cards: border-l-4 border-indigo-500
```

### Rich / Premium (Robinhood / Airbnb-like)
```
bg-slate-900 text-slate-100 (dark) or bg-white text-gray-900 (light)
font: Inter or DM Sans
radius: rounded-2xl
shadow: shadow-xl shadow-black/20
spacing: p-6 gap-6
accent: gradient (from-violet-500 to-indigo-600) or brand color
glass: bg-white/10 backdrop-blur-sm (dark mode cards)
```

### Playful (Duolingo / consumer-game-like)
```
bg-gradient-to-br from-green-400 to-emerald-600 (or domain color)
text-white / text-gray-900 on cards
font: Nunito or Poppins (round)
radius: rounded-2xl rounded-3xl
shadow: shadow-lg shadow-green-500/30
spacing: p-5 gap-4
accent: bg-yellow-400 text-gray-900 (CTA)
celebration: animate-bounce, confetti hint
```

---

## Empty State Patterns by Domain

Every screen that lists data must have an empty state. Use these by domain:

| Domain | Empty state message | CTA |
|--------|-------------------|-----|
| Internal tool / CRM | "No records yet. Add your first one to get started." | "Add [item]" button |
| E-commerce / inventory | "Your catalog is empty. Add products to start selling." | "Add product" |
| Scheduling | "No upcoming appointments. Share your booking link to get started." | "Copy link" |
| Dashboard / analytics | "No data yet. Data will appear here once users start using the product." | — |
| Community / feed | "Nothing here yet. Be the first to post." | "Write a post" |
| Automation / run log | "No runs yet. Trigger your first automation to see results here." | "Run now" |

---

## Loading State Patterns

Every async operation needs a loading state. Defaults by component type:

| Component | Loading pattern |
|-----------|----------------|
| Full page | Full-width skeleton cards (3–4 gray pulse blocks) |
| Data table | Skeleton rows with gray bars for each column |
| Stat/metric card | Pulse gray rectangle replacing number |
| Chart | Light gray area placeholder at chart height |
| Button (submitting) | Spinner icon + disabled state, keep button width |
| Form submit | Replace text with "Saving..." + spinner |

Tailwind skeleton class: `animate-pulse bg-gray-200 rounded` on placeholder divs.

---

## Component Library Selection by Context

| Situation | Recommended library | Notes |
|-----------|-------------------|-------|
| Standard React web app | shadcn/ui + Tailwind | Best quality, copy-paste, accessible by default |
| Rapid prototype | Tailwind utility classes only | No dependency, faster to write |
| Mobile (React Native) | NativeWind + React Native Paper | Tailwind classes in RN |
| Dashboard-heavy | shadcn/ui DataTable + recharts | Tables + charts covered |
| Form-heavy | shadcn/ui Form + react-hook-form + zod | Validation + accessibility |
| Admin tool | shadcn/ui + Tanstack Table | Sort/filter/pagination built in |

**shadcn/ui components most used in intake outputs:**
`Card`, `Button`, `Badge`, `Table`, `Tabs`, `Input`, `Select`, `Dialog`, `Sheet`,
`Avatar`, `Skeleton`, `Separator`, `DropdownMenu`, `Command` (search)

---

## Navigation Patterns by Platform × Screen Count

| Platform | 1–3 screens | 4–6 screens | 7+ screens |
|----------|------------|------------|-----------|
| Web | No nav / minimal links | Top nav bar (4 items max) | Left sidebar + top bar |
| Mobile web | Bottom sheet or single page | Bottom tab bar (4 items) | Hamburger → drawer |
| Mobile app (RN) | Stack navigator only | Bottom Tab Navigator | Drawer + Bottom Tab |
| Internal tool | Single full-width layout | Left sidebar + header | Left sidebar sections |

---

## Accessibility Quick-Reference for Mockup Generation

Apply by tier:

**WCAG 2.1 AA (recommended — default for consumer apps)**
- All images: `alt` text
- All interactive elements: keyboard focusable + `aria-label`
- Color contrast: 4.5:1 for normal text, 3:1 for large text
- No color as sole indicator (use icons or text labels too)
- Focus indicator visible (don't remove `:focus` outlines)

**WCAG 2.1 AA (required — government, healthcare, broad consumer)**
- All of above, plus:
- Form inputs: explicit `<label>` elements (not just placeholder)
- Error messages: programmatically associated with the field
- Headings: logical hierarchy (h1 → h2 → h3, not skipped)

**WCAG 2.1 AAA (when explicitly flagged)**
- All of above, plus:
- Contrast: 7:1 normal text
- No content flashes > 3 times/sec
- Session timeout warnings at 20 seconds minimum
