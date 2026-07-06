# Design Workflow (UI/UX)

> Make it usable and consistent before you make it pretty.

**Use when:** you're building or reworking anything the user sees. Pairs with
[feature.md](./feature.md) step 2.

**Solo-dev reality:** you're not running usability studies. The goal is "clear,
consistent, and not annoying," not a design system. Steal good patterns instead
of inventing.

---

## 1. Start from the user's goal, not the screen

- What is the user trying to *do* on this screen? Design the shortest path to that.
- What's the one primary action? Make it obvious. Everything else is secondary.
- Copy first: write the actual button labels, headings, and error messages before
  styling. Good words remove the need for clever UI.

## 2. Design the states, not just the happy path

Every screen/component needs these handled. This is where UX quality actually lives:

- **Empty** — first-run, no data yet. Say what to do next, don't show a blank void.
- **Loading** — skeleton or spinner. Don't let layout jump.
- **Error** — plain-language message + a way to recover. Never a raw stack trace.
- **Success / populated** — the normal case.
- **Edge** — very long text, many items, tiny/huge screen.

## 3. Consistency > cleverness

- Reuse existing components, spacing, and colors. Pick a spacing scale (e.g. 4/8px)
  and stick to it.
- Limit choices: 1–2 fonts, a small color palette, one button style per priority.
- Match platform conventions — users already know how things "should" work.
- If building charts/data views, see the `dataviz` guidance before picking colors.

## 4. Accessibility basics (cheap, high-value)

- Sufficient color contrast (don't rely on color alone to convey meaning).
- Real semantic elements (`button`, `label`, `nav`) — not clickable `div`s.
- Keyboard: can you tab to and trigger every control?
- Alt text on meaningful images; visible focus states.
- Tap targets big enough for a thumb (~44px).

## 5. Responsive & feel

- Design mobile-first if it'll be used on phones, then scale up.
- Give feedback for every action (hover, active, disabled, loading).
- Keep motion subtle and fast (150–250ms). Respect `prefers-reduced-motion`.

## 6. Gut-check before building

- Show it to one real person (or rubber-duck it): "what would you tap first?"
- Squint test: is the primary action still obvious when blurry?

---

## Quick checklist

- [ ] Primary action per screen is obvious
- [ ] Real copy written before styling
- [ ] Empty / loading / error / success / edge states designed
- [ ] Reused existing components, spacing, colors
- [ ] Contrast + semantic elements + keyboard + focus states
- [ ] Feedback on every interaction
- [ ] Works on the smallest target screen
- [ ] Gut-checked with a person or rubber duck
