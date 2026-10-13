# 07 — Ethics & Deceptive Patterns

Modern digital design is less about decoration and more about influencing behavior: tracking, testing, behavioral economics, and statistics aimed at business outcomes. The same research methods that make a product easier can make it more extractive — the difference is intent, not technique. Deceptive patterns spread because they're cheap to build and produce measurable short-term gains, so they appear by default unless someone actively prevents them.

They aren't always deliberate. A subscription page that doesn't clearly explain recurring charges can be pure carelessness — and then the revenue it generates becomes something the business depends on without understanding why.

## Terminology

**Deceptive pattern** is the preferred term ("deceptive or manipulative pattern" in legal contexts, since not all of them involve deception). "Dark pattern" is being phased out for its connotations, though it now has a definition in EU law and persists there. Related terms in use: **harmful online choice architecture** (UK CMA), **dark nudge** (behavioral economics), **sludge** (friction deliberately added to obstruct), and **asshole design** (colloquial).

Outside legal contexts, the most useful move is to describe the specific pattern plainly rather than argue about the label.

## Why they proliferate

- **Metrics-driven management.** Measuring is now nearly free, and teams are rewarded — via targets, bonuses, promotions — for moving a number. Rewarding a metric is nearly identical to punishing its absence, which pushes teams toward whatever raises it fastest.
- **A/B testing at scale** finds manipulative variants automatically. Nobody has to design a deceptive pattern deliberately; a test can simply discover that the confusing version converts better.
- **Asymmetry of attention.** The company optimizes full-time; the user glances for two seconds.
- **Diffuse responsibility.** No single person feels they made the decision, though a product manager is usually formally accountable for it.

## The taxonomy

Recognize these by name when reviewing an interface:

**Sneaking** — hiding or delaying information the user would object to.
- *Sneak into basket*: adding items the user didn't select
- *Hidden costs*: fees, shipping, or tax revealed only at the last step
- *Hidden subscription*: a "free trial" or one-off purchase that silently becomes recurring
- *Bait and switch*: an action produces a different outcome than advertised

**Urgency** — pressure to decide before thinking.
- *Fake urgency*: countdowns that reset, "sale ends today" every day
- *Fake scarcity*: invented low-stock or high-demand counts

**Misdirection** — steering attention or emotion toward the business's preferred choice.
- *Confirmshaming*: guilt-loaded decline text ("No thanks, I don't like saving money")
- *Visual interference*: the preferred option styled prominently, the alternative made faint, tiny, or ambiguous
- *Trick wording*: double negatives and deliberately confusing phrasing, especially in consent checkboxes
- *Pressured selling*: defaulting to the more expensive option

**Social proof** — manufactured consensus.
- *Fake reviews / fake testimonials*
- *Fake activity notifications* ("12 people are viewing this")

**Scarcity of exit / Obstruction (sludge)** — making a legitimate action artificially hard.
- *Hard to cancel* (the "roach motel"): one-click signup, phone-call-only cancellation
- *Price comparison prevention*: structuring information so options can't be compared
- *Intermediate currency*: points, coins, or credits that obscure real cost

**Forced action** — requiring something unrelated to proceed.
- *Forced enrollment*, *forced account creation*, *privacy Zuckering* (dark-pattern-driven oversharing)

**Nagging** — repeated interruptions to request something already declined.

**Preselection** — opt-in boxes ticked by default, especially for marketing and data sharing.

**Disguised ads** — advertising styled as content, navigation, or system UI.

## The line between persuasion and manipulation

A workable test, in three questions:

1. **Would the user object if the mechanism were explained to them plainly?** If yes, it's manipulation. Persuasion survives disclosure; manipulation depends on the user not noticing.
2. **Does it work by giving the user reasons, or by exploiting how attention and cognition fail?** Highlighting a genuine benefit is persuasion. Working because the decline link is grey and 10px is exploitation.
3. **Whose goal does it serve when they conflict?** A recommendation that's better for the user and for you is fine. A default that's better for you *and worse for them* is where the problem lives.

Additional practical checks:
- **Symmetry**: is unsubscribing/cancelling/declining as easy as the opposite action? Asymmetry is the single most reliable signal.
- **Defaults**: does the default serve the person who didn't read?
- **Reversibility**: can a wrong choice be undone easily?
- **Would you show this pattern to the user's family and describe it honestly?**

## Consent and privacy flows

- Consent must be freely given, specific, informed, and unambiguous to be meaningful (and legally valid under GDPR-style regimes). Pre-ticked boxes are not consent.
- Reject-all must be as prominent and as few clicks as accept-all. A cookie banner with a one-click "Accept" and a three-layer "Manage preferences" is a deceptive pattern, and increasingly an enforced one.
- Don't bundle unrelated consents. Don't condition core functionality on consent to unrelated data use.
- Say what data is collected and why, in words a person reads once and understands.

## Regulation (context, not legal advice)

The area is moving fast: the EU's Digital Services Act and Digital Markets Act, the Data Act and GDPR enforcement, the US FTC's actions on negative-option billing and "click to cancel," California's CPRA provisions on dark patterns, and the UK CMA's work on harmful online choice architecture. The practical implication for design work: patterns that were merely dubious a few years ago now carry real regulatory exposure, and "our competitors do it" is not a defense.

Where legal specifics matter, say that a lawyer needs to confirm rather than asserting the state of the law.

## Ethical use of psychology

Every principle in file 03 is dual-use. Variable rewards create habit *and* compulsion. Peak–end shapes memory *and* covers up a bad middle. Social proof informs *and* fabricates consensus. Scarcity communicates *and* coerces.

Two guardrails when applying them:

- **Design for the user's goals, not just engagement.** Time-on-app is a proxy that diverges from user benefit exactly where it matters most.
- **Consider the vulnerable user, not the median one.** Patterns tuned on aggregate metrics land hardest on people who are distracted, in crisis, cognitively impaired, elderly, or young. If a pattern is only defensible for an alert, informed adult with time, it isn't defensible.

## When reviewing a product

Flag any of these explicitly, name the pattern, and propose the honest alternative:
- Cancellation harder than signup
- Costs disclosed later than the point of commitment
- Any pre-ticked consent
- Countdowns, stock counts, or activity notifications that aren't real
- Decline options that are visually suppressed or emotionally loaded
- Anything that would embarrass the team if screenshotted publicly
