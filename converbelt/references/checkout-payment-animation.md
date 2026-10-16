# Checkout payment animation spec (Station 2 to Station 3)

Use this whenever the project has a checkout, payment, donation, or "pay" step, or when the user runs
`/converbelt checkout-demo` to build it as a standalone demo. Inside a real project, keep the layout,
animation, accessibility and code-quality rules, but use the project's own palette and fonts from Station 2.

Build a single-file checkout demo whose centerpiece is a payment animation:
tap "Pay" → processing → confirmed. Vanilla HTML/CSS/JS, no build step, no external
dependencies except one Google Font.

## Anti-slop rules

Follow the anti-slop rules in `SKILL.md` (Station 2). Breaking any is a failure. One exception:
this spec's pay button is a pill that morphs into a circle, because that morph is the point of the animation.
Every other button in the demo uses a modest corner radius.

## Design system

Feel: calm, precise, trustworthy. Fintech, not startup landing page. Restraint wins.

Color tokens (CSS variables on :root, plus a dark-mode set via prefers-color-scheme):

| Token | Value | Use |
|---|---|---|
| `--navy` | `#0F2A4A` | headings, brand accent |
| `--action` | `#1559B8` | pay button (white text 6.7:1) |
| `--action-press` | `#0F4796` | pay button hover/pressed |
| `--bg` | `#FAF8F5` | warm off-white page |
| `--surface` | `#FFFFFF` | panels |
| `--ink` | `#1A1A1A` | body text |
| `--muted` | `#6B6B6B` | secondary text (5.0:1 on `--bg`) |
| `--line` | `#E6E2DC` | 1px dividers (use lines, not shadows, to separate) |
| `--success` | `#1A7F37` | confirmed state (white check 5.1:1) |
| `--error` | `#C5221F` | error state (white mark 5.8:1) |

These are starting values. Re-check contrast for the dark-mode set.

Type: "Manrope" (Google Fonts) 400/600/700. Prices use font-variant-numeric: tabular-nums.
Scale: 13 / 15 / 17 / 22 / 28px. Line-height 1.45 body, 1.2 headings.

Radius: 8px panels, 999px for the pay button only. At most one soft shadow on the main panel, nowhere else.

Layout:
- Desktop: two columns. Left = order summary (3 real-sounding line items with qty and price,
  then subtotal, shipping, tax, total). Right = payment panel (email field prefilled,
  "Pay with" row, the pay button, one muted line of fine print).
- Mobile (<768px): single column, summary collapses into a "Show order summary" row,
  16px side gutters, no horizontal scroll, pay button full width and sticky at the bottom.

## The animation (the whole point)

State machine: idle → pressed → processing → success | error. One class per state on the
button root. Only animate transform, opacity, clip-path, stroke-dashoffset.

1. IDLE
   Pill button, 52px tall, label "Pay $84.20" in 600 weight.
   Hover: background → --action-press, 150ms. No lift, no glow.

2. PRESSED (on pointerdown)
   scale(0.97), 100ms ease-out. On release, return to 1.0 in 160ms, no overshoot.

3. PROCESSING (starts on release, lasts ~1600ms, simulated)
   a. Label fades out + translates up 4px, 150ms.
   b. Button collapses from full width to a 52px circle, 320ms,
      cubic-bezier(0.65, 0, 0.35, 1). Use clip-path or a width-locked wrapper, not reflow.
   c. Inside the circle, an SVG ring spinner: 2px white stroke, 270° arc, rotating 800ms
      linear infinite, dasharray gently breathing.
   d. Order summary dims to opacity 0.55, pointer-events: none.

4. SUCCESS (~700ms total)
   a. Spinner arc sweeps to a full 360° ring (200ms).
   b. Circle background crossfades to --success (180ms).
   c. White checkmark draws via stroke-dashoffset, 260ms ease-out, then a single
      1.0 → 1.06 → 1.0 pop (180ms).
   d. Below it, staggered 60ms apart, each fading in + rising 8px:
      "Payment complete" (17px, 700)
      "$84.20 paid to Northline Supply Co." (muted)
      "Ref 7KX-21904 · Receipt sent to you@example.com" (13px, muted)
   e. Summary returns to full opacity.

5. ERROR (when the "Simulate failure" toggle is on)
   Circle turns --error, white "!" appears, horizontal shake 3 cycles × 4px over 300ms,
   then it expands back into the pill with label "Try again" and a muted error line:
   "Your card was declined. No charge was made."

Reduced motion (prefers-reduced-motion: reduce):
   No morph, shake or spin. Button label swaps to "Processing...", then instant check icon,
   with plain 150ms opacity fades only.

Accessibility:
   Real <button>, visible :focus-visible ring (2px --navy, 2px offset).
   aria-live="polite" region announces "Processing payment", then "Payment complete" or the error.
   Button gets aria-busy="true" while processing. Contrast meets WCAG AA in both themes.

## Demo controls

Small, unobtrusive control strip in the bottom-left corner (muted, 13px):
  [Replay]  [ ] Simulate failure  [ ] Slow-mo 3×
Slow-mo multiplies every duration via a single --t CSS variable.

## Code quality

- All timings and easings defined once as CSS variables.
- JS is a tiny state machine (setState(name)), no animation logic scattered in timeouts
  beyond the simulated network delay.
- No console errors. Works in latest Chrome, Safari and Firefox.

## Before you finish

Check every anti-slop rule against your output, then end your reply with:
1. Which rules you were tempted to break and what you did instead.
2. One thing you'd refine with more time.
