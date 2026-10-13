# 10 — Audit Checklist

Use this for full site/app reviews, or when a request spans several areas at once. Work top to bottom; the early sections find the biggest problems.

## How to structure an audit response

1. **State what you assumed** — who the users are, what the primary task is, what business goal the product serves. If unknown, say so and ask; don't invent motives for observed data.
2. **Findings, ordered by severity**, not by page order. For each: what the user experiences, why it happens (name the principle), and the concrete fix.
3. **Severity scale**: blocker (task cannot be completed) > serious (many users will fail or abandon) > moderate (friction, confusion, wasted time) > minor (polish).
4. **Separate what's known from what needs testing.** Anything depending on how real users behave is a hypothesis, labeled as one.
5. **End with a short prioritized list** — the 5–10 things to do first, ordered by impact ÷ effort.

Don't produce a flat list of generic best practices. A finding that isn't tied to something specific on the actual screen isn't a finding.

---

## A. Purpose and first impression

- [ ] Is it clear within seconds what this is and what it's for?
- [ ] Is the value proposition specific and differentiating, or generic filler?
- [ ] Is the most likely primary task the most prominent thing on the entry screen?
- [ ] Five-second test: after five seconds, could a stranger say what this does and what to do next?
- [ ] Does the design look credible and cared-for, or does execution quality undermine trust?

## B. Navigation and structure

- [ ] Trunk test on 3 random deep pages: site ID, page name, sections, local nav, "you are here", search — all findable at a glance?
- [ ] Is persistent navigation present and consistent (with deliberate exceptions in focused flows)?
- [ ] Do page names match the links that lead to them?
- [ ] Are category labels in the user's vocabulary, not internal jargon?
- [ ] Is anything important reachable only through a menu that hides it?
- [ ] Does search exist, work, handle typos, and handle zero results as a designed state?

## C. Visual hierarchy and clarity

- [ ] Does prominence match importance?
- [ ] Are related things visually grouped, and is nesting visually expressed?
- [ ] Squint test: does the primary action still stand out?
- [ ] Is everything clickable obviously clickable — and is anything non-clickable pretending to be?
- [ ] Noise check: what's shouting, what's disorganized, what's clutter?
- [ ] Is emphasis rationed, or is everything emphasized?

## D. Content and copy

- [ ] Cut the word count in half — what breaks?
- [ ] Any happy talk, welcome text, or mission statements occupying prime space?
- [ ] Any instructions that exist because something isn't self-evident?
- [ ] Are buttons labeled with outcomes rather than mechanisms?
- [ ] Are error messages plain, specific, actionable, and blame-free?
- [ ] Are empty states doing teaching work, or are they blank?
- [ ] Is terminology consistent for the same concept across every screen?

## E. Forms and conversion flows

- [ ] Which fields can be deleted, derived, or deferred? (question protocol)
- [ ] Visible associated labels on every input — no placeholder-as-label?
- [ ] Right control for each question (selects and date pickers scrutinized)?
- [ ] Correct `type`, `inputmode`, and `autocomplete` set?
- [ ] Is input format forgiving (spaces, cases, separators normalized server-side)?
- [ ] Validation on submit, error summary at top with links, inline errors, focus moved, input preserved?
- [ ] One primary action, protected from double submission, labeled with the outcome?
- [ ] Long flows split one-thing-per-page, with progress saved and a review step?
- [ ] Guest checkout available; total cost disclosed before the last step?

## F. Accessibility

- [ ] Complete the primary task with the keyboard only — does it work, in a sensible order, with a visible focus indicator?
- [ ] Contrast: body ≥ 4.5:1, large text and component boundaries ≥ 3:1?
- [ ] Is color ever the only signal for status, error, or selection?
- [ ] Semantic HTML — real buttons, real headings forming an outline, one h1, no skipped levels?
- [ ] Alt text decisions made on every image; decorative images marked as such?
- [ ] Usable at 200% zoom, reflowing without horizontal scroll?
- [ ] Dynamic changes announced (validation, filter counts, loading completion)?
- [ ] Motion preferences respected; nothing autoplaying or flashing?
- [ ] `lang` and `dir` correct, including for mixed RTL/LTR content?

## G. Mobile

- [ ] Real device, real connection — does the primary task complete?
- [ ] Touch targets ≥ ~44px with spacing; no destructive action adjacent to a frequent one?
- [ ] Anything discoverable only by hover or by an unhinted gesture?
- [ ] When the keyboard opens, is the active field and its label still visible?
- [ ] Anything important buried under a banner, promo, or cookie wall?
- [ ] Is capability missing on mobile that exists on desktop?
- [ ] Zoom not disabled; body text ≥ ~16px?

## H. Performance and feedback

- [ ] Does interaction feedback appear within ~400ms?
- [ ] Where it can't, is there meaningful progress feedback rather than a spinner?
- [ ] Are slow operations optimistic or streaming partial results where possible?
- [ ] Is perceived weight reasonable on a throttled connection?

## I. Ethics and trust

- [ ] Is cancelling/unsubscribing/declining as easy as the opposite action?
- [ ] Are all costs disclosed before the user invests effort?
- [ ] Any pre-ticked consent boxes, or bundled consents?
- [ ] Is reject-all as prominent and as few clicks as accept-all?
- [ ] Any countdowns, stock counts, or activity notifications that aren't real?
- [ ] Any confirmshaming, visual suppression of the decline option, or trick wording?
- [ ] Is anything hidden that users obviously want (prices, support contact, cancellation)?
- [ ] Would any pattern here embarrass the team if screenshotted publicly?

## J. AI features (if present)

- [ ] Is the use case one where a mostly-right answer is genuinely valuable?
- [ ] Does the interface show what the system understood (restating) before consequential actions?
- [ ] Do empty states offer useful initial suggestions rather than a blank prompt box?
- [ ] Is uncertain output visually distinguishable from retrieved fact?
- [ ] Are sources and reasoning available where the user must verify?
- [ ] Is correcting a wrong output cheap and one-step?
- [ ] Are irreversible actions gated by explicit human confirmation?
- [ ] Is the manual path preserved for when the AI is wrong or unavailable?
- [ ] Are failure states — wrong, empty, partial, timed-out, refused — actually designed?

## K. Evidence and next steps

- [ ] Which findings are certain, and which are hypotheses that need testing?
- [ ] What does analytics already show about where people drop off?
- [ ] What three problems would you fix before the next round of testing?
- [ ] What's the cheapest test that would settle the biggest open question?
