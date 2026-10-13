# 06 — Mobile & Responsive

## The premise that has to be corrected first

Mobile usability is not a different discipline with different rules. Everything in file 01 applies: don't make me think, design for scanning, minimize noise, omit words. The difference is that mobile removes slack. On a small screen, in poor light, one-handed, on a bad connection, while distracted, every usability flaw that desktop tolerated becomes a task failure.

Also correct the old assumption that mobile users want *less*. Mobile is where most usage happens; people do everything on a phone. Removing capability from the mobile version because "they're on the go" is a mistake — anything you cut, someone will need, in the place you cut it from.

## The core constraints

1. **Screen space.** The hardest constraint. Everything competes; hierarchy must be brutal.
2. **Touch instead of pointer.** Fingers are imprecise and occlude what they touch.
3. **No hover.** Any information or affordance revealed on hover is invisible on touch. This silently breaks tooltips, hover menus, and "hidden until hover" controls.
4. **Attention is fragmented.** Interruptions, notifications, and context switches are constant.
5. **Connection and battery** are variable. Weight is a usability feature.

## Practical rules

- **Touch targets** at least ~44×44 px with real space between them. Adjacent destructive and frequent actions are a recipe for accidental taps.
- **Thumb reach:** the bottom and center of the screen are cheap to reach; top corners are expensive. Put primary actions where the thumb already is. Bottom navigation and bottom sheets exist for this reason.
- **Don't hide affordance.** Mobile designs routinely trade affordance for space — icons without labels, gestures without hints, controls behind menus. Every hidden thing is a thing some users will never find. Icons alone are rarely self-evident; label them.
- **The hamburger menu is a tradeoff, not a solution.** It saves space at the cost of discoverability, and putting everything behind it means users only find what they already knew to look for. Surface the two or three most important destinations outside it.
- **Forms are the worst mobile experience.** Everything in file 04 applies double: correct `inputmode` and `autocomplete` so the right keyboard appears and autofill works, one-thing-per-page for anything long, labels above fields (never placeholder-as-label — the on-screen keyboard covers half the screen and the placeholder is gone).
- **Never disable zoom.** `user-scalable=no` is an accessibility failure, and unnecessary once text is legible.
- **Legibility:** ~16px minimum for body text; smaller sizes trigger the browser's zoom and break your layout anyway.
- **Interstitials, popups, and cookie walls** cost far more on mobile, where they can occupy the entire viewport and their close buttons are often too small to hit.
- **Test on a real device**, at real brightness, outdoors, on a throttled connection. Browser devtools emulation hides the problems that matter most.

## Responsive vs. separate mobile site

A single responsive implementation is almost always preferable: one URL, one content set, one codebase, no "view desktop site" escape hatch, and no divergence over time. Separate mobile sites reliably fall behind in content and features, and users notice.

Where a genuinely different mobile design is justified, it's usually because the *task* differs contextually (quick lookup vs. deep configuration), not because the *user* differs.

## Apps vs. web

Apps get: device capabilities, offline, notifications, better performance, home screen presence. They pay: install friction, update cycles, two codebases, store policies.

Two usability traps specific to apps:

- **Learnability is worse than assumed.** Native apps use fewer shared conventions than the web; users can't fall back on "it works like every other site." The first-run experience carries more weight, and idiosyncratic gestures need teaching.
- **There's often no Back.** The web's universal safety net doesn't exist consistently in apps, so wrong turns are more expensive. Make the way back explicit, and make destructive actions undoable.

## RTL and bilingual interfaces

For Persian/Arabic/Hebrew interfaces:

- Mirror the layout, not just the text: navigation, progress direction, back/forward icons, sliders, and alignment all flip.
- Numbers, Latin brand names, and code stay LTR inside RTL text — check that mixed strings render correctly rather than assuming.
- Icons with directional meaning (arrows, undo, next) flip; icons representing real-world objects generally don't.
- Set `dir` and `lang` correctly; don't fake RTL with text-align alone.
- Persian text needs adequate line height and a font with proper Persian glyphs (`ی`/`ک` vs Arabic variants) — a rendering problem reads to users as a quality problem.

## Mobile critique checklist

1. Squint at it: is the primary action still obvious?
2. Is anything only discoverable by hover or by an unhinted gesture?
3. Can each target be hit accurately with a thumb, without hitting its neighbor?
4. When the keyboard opens, is the field being filled still visible, along with its label?
5. Is anything important below a full-viewport promo, banner, or cookie wall?
6. What breaks at 200% text size?
7. What does this look like at 3G speed, with images not yet loaded?
