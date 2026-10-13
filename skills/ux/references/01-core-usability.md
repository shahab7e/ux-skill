# 01 — Core Usability

The default starting point for any "is this clear / why is this confusing" question.

## The first law: don't make me think

A page or screen should be **self-evident** — obvious, self-explanatory. A person should understand what it is and how to use it without expending effort on interpretation.

Where "thinking" gets injected without anyone noticing:

- Names that are clever, marketing-driven, or company-specific instead of plain ("Jobs" beats "Career Opportunities" beats "Talent Acquisition Hub")
- Links and buttons that don't visibly look clickable
- Two things that do the same job styled differently, or two different things styled the same
- Search boxes with an unexplained "Go" / "Search" ambiguity, or unclear scope (searching what?)
- Anything that makes a person pause and ask "wait, is this what I want?"

When self-evident isn't achievable (genuinely novel or complex interactions), the fallback standard is **self-explanatory**: the person may need a moment, but the explanation is right there and costs nothing to get.

**Every question mark added to a screen is friction.** Some friction is unavoidable; unintentional friction never is.

## How people actually use interfaces

Three realities that invalidate most design assumptions:

1. **People scan, they don't read.** Users are usually on a mission, know most of a page is irrelevant, and are good at ignoring anything that doesn't look like what they want. They look for words matching their goal, not the "best" answer on the page.
2. **People satisfice.** They pick the *first reasonable option*, not the best one. Optimizing is hard, the penalty for a wrong guess is usually just a Back click, and guessing is faster than evaluating. This means the first plausible-looking link wins — so the first plausible-looking link had better be a good one.
3. **People muddle through.** Most users don't have an accurate mental model of how the system works; they form a rough one that works well enough and stick with it. They rarely read instructions or explore to understand — they get by. Designs must work for people who *don't* understand them.

**Implication:** don't design for the ideal user who reads carefully, considers options, and understands the system. Design so the muddling scanner still succeeds.

## Billboard design: designing for scanning

Rules that make a page scannable:

- **Use conventions.** Conventions exist because they work; users arrive already knowing them. Deviating requires a clearly better idea *and* the new thing must be self-evident immediately. Innovation should be spent where it creates value, not on relocating the search box.
- **Create a clear visual hierarchy.** Importance = prominence. Things that are logically related are visually related (grouped, aligned, nested). Things that are part of something else are visually nested inside it. A page whose visual hierarchy matches its logical hierarchy can be understood at a glance; one that doesn't forces conscious parsing.
- **Break pages into clearly defined areas.** Users decide within seconds which regions to ignore. Clear regions let them skip most of the page cheaply.
- **Make clickable things obviously clickable.** Users spend most of their time deciding what to click. Ambiguous affordance is expensive.
- **Minimize noise.** Three kinds: shouting (everything competing for emphasis at once), disorganization, and clutter. Emphasis only works if most things aren't emphasized. Default to deleting.

## Mindless choices

The number of clicks matters less than how hard each click is. **Three mindless, unambiguous clicks beat one click that requires thought.** Don't collapse a flow into fewer steps if the compression makes each step ambiguous — and don't pad a flow with steps that could have been merged without adding ambiguity.

Every choice should be:
- Unambiguous (I know what each option means)
- Reversible or clearly consequential (I know what happens if I'm wrong)
- At the right moment (I have what I need to decide now)

## Omit needless words

Cut the word count of every page roughly in half, then cut it again. Two categories to remove first:

- **Happy talk.** Introductory prose that says nothing — welcome text, "we're pleased to offer", mission statements at the top of a task page. It is scenery, and scanners skip it, so it just pushes real content down.
- **Instructions.** Most instructions exist because the thing they describe isn't self-evident. Fix the thing; delete the instruction. What can't be eliminated should be compressed to the minimum, placed exactly where it's needed, not in an intro paragraph.

Shorter text means: less noise, more visible useful content, shorter pages, less scrolling past filler.

## Microcopy rules of thumb

- Label buttons with the **outcome**, not the mechanism: "Create account", "Pay $24", not "Submit" or "Continue" where the destination matters.
- Use the user's vocabulary, not the org chart's. If people call it "shipping cost," don't call it "logistics fee."
- Error messages should say what went wrong, in plain language, *and* what to do about it. Never blame the user, never expose codes without translation.
- Empty states are teaching moments, not blank space: say what goes here and give the action to create the first one.
- Avoid ambiguity in verbs of consequence: "Remove" vs "Delete" vs "Cancel" mean different things and should not be used interchangeably.

## Usability as courtesy: the reservoir of goodwill

Each person arrives with a finite, idiosyncratic, situational reserve of patience. Every problem draws it down. Once it's empty, they leave — and may think worse of the organization afterward. A single severe insult (a huge unexpected registration form, a hidden price) can drain it instantly.

**Things that drain goodwill:**
- Hiding what people obviously want — support phone numbers, prices, shipping costs, cancellation
- Punishing users for not doing things your way (rigid input formats for phone numbers, dates, card numbers)
- Asking for information you don't need, or asking for it before it's relevant
- Fake sincerity — friendly copy contradicted by hostile behavior
- Amateurish or broken execution, which reads as "they don't care"
- Not acknowledging a situation the user is obviously worried about (outage, delay, strike, incident) anywhere visible

**Things that refill it:**
- Answering the most likely questions prominently, before being asked
- Saving people steps: pre-filling what you know, remembering choices, being forgiving about input format
- Being upfront about cost and consequence
- Making it easy to recover from mistakes
- Behaving as if the user's time has value

## Applying this to a critique

Ask, in order:
1. What is a person most likely here to do?
2. On first glance, is that path the most visually prominent thing?
3. What could be misread, and what happens if it is?
4. What's on this screen that no one on that path needs?
5. What question is a person likely to have here that isn't answered?
