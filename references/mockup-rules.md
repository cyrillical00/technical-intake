# Output 3 — React/HTML UI Mockup Generation Rules

Load this file when generating Output 3. Also load `references/ui-patterns.md` for
domain-specific component patterns, design system defaults, and visual tone mappings.

---

## What to Generate

A complete, self-contained React component (JSX) that visually represents the **primary
user flow** identified during the interview. This is a working mockup — not a wireframe.
It should render immediately with mock data, using only Tailwind CSS for styling.

---

## Component Generation Rules

1. **Scope:** Cover the single most important screen identified in Round 3 (primary view).
   Include navigation to 1–2 adjacent screens as non-functional tab/button stubs.

2. **Mock data:** Include realistic hardcoded data (not "Lorem ipsum"). Use names, dates,
   amounts, and statuses that match the product domain.

3. **Accessibility:** Apply `aria-label`, `role`, and semantic HTML per the accessibility
   tier from the UI Brief. Minimum: all interactive elements labeled, color not sole indicator.

4. **Interactivity:** Include at least one interactive state using React `useState` — e.g.,
   a toggle, a selected item, a form input. The mockup should feel alive, not static.

5. **Tech Tier adaptation:**
   - T0/T1: Include annotation comments explaining what each section does
   - T2: Standard commented code
   - T3: Minimal comments, include prop types or TypeScript interface stubs

6. **Mobile-first:** Default to mobile viewport width (max-w-sm or max-w-md) unless
   the spec states desktop-primary.

---

## Component Structure Template

```jsx
// [Product Name] — Primary Screen Mockup
// Generated from intake interview: [date]
// Screen: [screen name from §9 Screen Inventory]
// Design reference: [app from UI Brief]
// Tone: [minimal / utilitarian / rich / etc.]

import { useState } from "react";

// Mock data — replace with real API calls in implementation
const MOCK_DATA = {
  // [realistic domain-appropriate data]
};

export default function [ProductName]App() {
  const [activeTab, setActiveTab] = useState("[primary view]");
  // [additional state as needed]

  return (
    <div className="min-h-screen bg-[color] font-sans">
      {/* Navigation / Header */}
      <header className="...">...</header>

      {/* Primary View */}
      <main className="...">
        {/* [Primary action / content area] */}
        {/* [Secondary content] */}
        {/* Empty state — shown when no data */}
      </main>

      {/* Footer / Tab Bar (mobile) or Sidebar (desktop) */}
    </div>
  );
}
```

---

## Design System Defaults by Tone

Apply these from `references/ui-patterns.md` unless design references override them:

- **Minimal** (Notion-like): `bg-white text-gray-900`, `rounded-lg`, `shadow-sm`
- **Utilitarian** (Linear-like): `bg-gray-50 text-gray-800`, tight spacing, no shadows
- **Enterprise** (Stripe-like): `bg-white text-gray-700`, subtle shadow, indigo accent
- **Rich** (Robinhood-like): `bg-slate-900 text-white`, gradients, `shadow-xl`
- **Playful** (Duolingo-like): bold colors, `rounded-2xl`, celebration states

See `references/ui-patterns.md` for full Tailwind class specifications per tone.

---

## Offering the Mockup

Rather than auto-generating the mockup after every intake, present it as an option:

> *"I can also generate a working visual mockup of the [primary screen name] — a React
> component with realistic mock data that shows what the main screen could look and feel
> like. Want me to include that?"*

Generate only if the client confirms. This respects their time and avoids overwhelming
non-technical clients with code output they may not understand.

If the client declines, note in the spec: "UI mockup available on request."
