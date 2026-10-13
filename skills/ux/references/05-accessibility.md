# 05 — Accessibility

Accessibility is not a separate track from usability. Almost everything that helps disabled users helps everyone: clear labels, keyboard support, adequate contrast, and forgiving input reduce failure for the distracted, the rushed, the elderly, people on bad connections, and people in bright sunlight.

Treat this file as a **mandatory cross-check** on any answer involving forms, components, color, motion, or media — not as a topic that only comes up when asked.

## Framing

Four dimensions to check any design against:

- **Visual** — make it easy to see
- **Auditory** — make it easy to hear
- **Motor** — make it easy to interact with
- **Cognitive** — make it easy to understand

The cognitive dimension is the most neglected and the widest in impact.

Two arguments worth having ready: accessibility work is usually far smaller than feared once it's part of the process rather than a retrofit, and it is increasingly a legal requirement (US ADA/Section 508, EU Accessibility Act, EN 301 549, and equivalents) rather than a courtesy.

## The rules that catch most problems

1. **Use semantic HTML.** A `<button>` is focusable, keyboard-operable, and announced as a button for free; a `<div>` with a click handler is none of those. Native elements first, ARIA only to fill genuine gaps. Bad ARIA is worse than no ARIA.
2. **Every image needs an alt decision.** Meaningful images get descriptive alt text; decorative images get `alt=""` so they're skipped. Alt text describes function in context, not appearance ("Search", not "magnifying glass icon"). Complex images (charts, diagrams) need a longer description nearby.
3. **Everything works by keyboard.** Every interactive element reachable by Tab, in a logical order, operable by Enter/Space, with a **visible focus indicator**. Never remove focus outlines without replacing them with something at least as visible. No keyboard traps. Provide a "skip to main content" link.
4. **Contrast.** Body text at least 4.5:1 against its background; large text (≥18pt / 14pt bold) at least 3:1; interactive component boundaries and meaningful graphics at least 3:1. Placeholder text, disabled states, and light-gray-on-white are the usual offenders.
5. **Never use color as the only signal.** Errors, status, required fields, chart series, and selected states all need a second cue: text, icon, shape, weight, or position.
6. **Form fields need programmatically associated labels**, and errors need to be announced — see file 04 for the full pattern.
7. **Headings form an outline.** One `<h1>`, no skipped levels, used for structure not for size. Screen reader users navigate by heading more than any other method.
8. **Text resizes and reflows.** Content must remain usable at 200% zoom and reflow into a single column at narrow widths without horizontal scrolling.
9. **Respect motion preferences.** Honor `prefers-reduced-motion`. Avoid parallax, auto-playing carousels, and large-area motion. Nothing may flash more than three times per second.
10. **Media needs alternatives.** Captions for video, transcripts for audio, audio description where visual content carries meaning. Never autoplay audio.
11. **Announce dynamic changes.** Content that appears without a page load (validation results, filter counts, toasts, loading completion) needs a live region or a focus move, or screen reader users simply won't know it happened.
12. **Give users control of time.** Warn before session timeouts and allow extension. Don't impose time limits on reading or input without a way to turn them off.
13. **Touch targets** at least ~44×44 CSS px with spacing between adjacent targets.
14. **Language is declared** (`lang` attribute), including on inline passages in another language — this determines screen reader pronunciation. For bilingual Persian/English interfaces, mark the direction (`dir="rtl"`) and language correctly, and check that mixed-direction strings (numbers, Latin brand names inside Persian text) render in the right order.

## Cognitive accessibility

Often the largest group affected, and the least addressed:

- Plain language; short sentences; no unexplained jargon or acronyms
- Consistent navigation and consistent naming for the same thing across screens
- No unnecessary memory load across steps
- Clear, forgiving error recovery — and undo where possible
- Predictability: don't change context on focus or on input without warning
- Don't require precision (exact formats, exact timing) that isn't genuinely needed

## Testing accessibility cheaply

In rough order of value-per-minute:

1. **Unplug the mouse.** Do the main task with the keyboard only. This alone finds a large share of defects.
2. **Zoom to 200%** and to a narrow viewport. See what breaks or gets cut off.
3. **Run an automated checker** (axe, Lighthouse, WAVE). Automated tools catch roughly a third of issues — necessary, never sufficient.
4. **Turn on a screen reader** (VoiceOver, NVDA) and try to complete one task. Uncomfortable at first, extremely informative.
5. **Check contrast** on real screens, including the low-contrast items designers habitually approve.
6. **Test with disabled users.** Nothing else substitutes for this.

## Where accessibility and usability arguments converge

If someone resists an accessibility fix on cost grounds, the strongest points are usually: it also fixes an ordinary usability problem for everyone; it reduces support load; it's legally required in most markets; and it's dramatically cheaper now than as a retrofit after launch.
