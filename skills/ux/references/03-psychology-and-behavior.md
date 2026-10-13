# 03 — Psychology & Behavior

Use this to explain *why* a design works or fails, and to make predictions before testing.

## The ten laws

### Jakob's Law
People spend most of their time on *other* products, and build their expectations there. They prefer yours to work the way the ones they already know work.

**Apply:** lean on established conventions; users transfer knowledge instead of learning. When you must change something familiar, let people use the familiar version temporarily, and make the change gradual — abrupt redesigns generate backlash disproportionate to their objective merit, because you've invalidated a working mental model.

### Fitts's Law
Time to acquire a target is a function of the **distance to** and **size of** the target.

**Apply:** make interactive targets big enough to hit accurately (mobile touch targets: at least ~44×44 px / ~9mm, with adequate spacing). Put important controls where the hand or pointer already is — thumb zones on mobile, near the current focus on desktop. Screen edges and corners are effectively infinite-size targets on desktop. Never place a destructive action adjacent to a frequent one.

### Miller's Law
The average person holds about 7 (±2) items in working memory.

**Apply:** don't use "7" as a design rule — the real lesson is that working memory is small and fragile. **Chunking** is the tool: group content into meaningful units (phone numbers, card numbers, addresses) so it's processed and remembered in pieces. Applies to content structure as much as to number formatting.

### Hick's Law
Decision time increases with the number and complexity of choices.

**Apply:** minimize choices when responsiveness matters. Break complex tasks into steps. Highlight a recommended option. Use progressive disclosure — reveal advanced options only to those who go looking. Onboard gradually rather than dumping every capability at once. But don't over-simplify to the point of hiding what people need: the goal is fewer *simultaneous* decisions, not fewer capabilities.

### Postel's Law (robustness principle)
Be conservative in what you do, liberal in what you accept from others.

**Apply:** accept input in whatever format the person naturally provides (spaces in card numbers, various phone and date formats, upper/lowercase) and normalize on your side. Anticipate variability in devices, connection speeds, accessibility needs, and behavior. Give reliable, consistent output no matter how messy the input. Rejecting valid intent over formatting is one of the cheapest self-inflicted usability wounds.

### Peak–End Rule
People judge an experience mainly by its most intense point and its ending, not by the average of every moment.

**Apply:** identify the moments that matter most — the point of achieving the goal, the moment of highest emotion, and the final step — and invest disproportionately there. Also: negative peaks are remembered more strongly than positive ones, so removing one bad moment is usually worth more than adding a delightful one.

### Aesthetic–Usability Effect
Attractive designs are *perceived* as more usable, and people are more tolerant of minor problems in them.

**Apply:** visual quality buys goodwill and masks small flaws — useful, but also dangerous: it can hide real usability problems during testing, because users report satisfaction while still failing tasks. Watch behavior, not sentiment.

### Von Restorff Effect (isolation effect)
When multiple similar items are present, the one that differs is the one remembered.

**Apply:** make the primary action visually distinct from everything around it. But emphasis is a budget — emphasizing many things emphasizes nothing. Never rely on color alone to carry the distinction (color-blind users, low contrast), and be careful that a highly distinct element isn't mistaken for an advertisement. Avoid animating the distinction in ways that trigger motion sensitivity.

### Tesler's Law (conservation of complexity)
Every system has an irreducible amount of complexity. The question is only **who absorbs it** — the user, the designer, or the engineer.

**Apply:** move complexity out of the interface and into the system wherever possible (smart defaults, inference from context, doing the derivation instead of asking). Don't oversimplify to the point of removing meaning. When complexity truly can't be removed, it should be borne by the people building the product, not the person using it.

### Doherty Threshold
Productivity rises sharply when system and user interact at a pace where neither waits on the other — roughly **under 400ms**.

**Apply:** respond visibly within 400ms wherever you can. When you can't, use feedback to change the perception of duration: skeleton screens, progress indicators, optimistic UI, and showing partial results as they arrive. An operation that shows progress feels faster than a faster operation that shows nothing. Conversely, some operations are *too* fast to be believed (security scans, complex calculations) and benefit from deliberate perceived effort.

## Perception and attention

- **Gestalt grouping** drives what people perceive as related before they read anything: proximity, similarity, closure, continuity, common region, figure–ground. Whitespace and shared containers do more organizing work than borders and lines.
- **Attention is selective and filtered.** People notice what matches their goal and what's salient; everything else is genuinely invisible to them, including things they later swear were not there.
- **Peripheral vision detects the gist** of a scene before central vision resolves detail — which is why the squint test works and why layout matters before content does.
- **Sustained attention runs about 10 minutes** before it needs a refresh; long uninterrupted tasks need structure and checkpoints.
- **People cannot multitask** on tasks that both require attention; they task-switch, and pay a cost each time. Interrupting a user mid-task is expensive.
- **Faces, motion, and anything signalling danger or reward capture attention involuntarily** — powerful, and easy to abuse.

## Memory and comprehension

- **Recognition is far easier than recall.** Show options rather than requiring people to remember them. Never make someone remember information across screens.
- **Memory is reconstructive** — it changes each time it's recalled, and confident memories are frequently wrong. Don't trust users' recollection of what they did; observe.
- **Mental models vs. conceptual models.** People arrive with a mental model built from prior experience; your interface presents a conceptual model. Trouble comes from the mismatch. Either match the existing mental model, or make the new conceptual model so clearly visible that a correct new mental model forms fast.
- **People process information best in story form** and learn best from examples — worked examples beat abstract instructions.
- **Bite-sized chunks + progressive disclosure** beat completeness on one screen.

## Reading

- Font size and contrast are usability features, not style choices. Small, low-contrast text excludes people silently.
- All-caps text isn't intrinsically unreadable but is slower because it's less practiced — reserve it for very short labels.
- Long line lengths are read faster but preferred less; around 50–75 characters per line is a reasonable comfort target.
- Reading on screen is harder than on paper; people scan more and comprehend less, so structure (headings, chunks, lists) matters more.

## Motivation and behavior

- **Progress motivates.** People push harder as a goal gets closer; showing progress (and giving a head start) increases completion.
- **Variable rewards are powerful and habit-forming** — this is the mechanism behind feeds, notifications, and streaks. It works, and it's also the primary engine of compulsive use. Treat it as an ethical decision, not a growth tactic. (See file 07.)
- **Intrinsic motivation beats extrinsic** for sustained behavior; autonomy, mastery, and purpose outperform points and badges long-term.
- **People are cognitive misers** — they take the easy path and use shortcuts only if the shortcuts are themselves easy. Any friction you add will filter people out, whether or not you intended it to.
- **Social proof, reciprocity, and commitment** shape behavior online as in person; people apply social rules to systems that behave socially.

## Error

- **People will always make mistakes; there is no error-proof product.** Design for error as a normal state, not an exception.
- Errors rise under stress, time pressure, and cognitive load — exactly the conditions of checkout, support, and emergency flows.
- **Slips** (right intention, wrong action) are fixed by better affordances, spacing, and confirmation on destructive actions. **Mistakes** (wrong intention) are fixed by clearer conceptual models and information.
- Prevention beats messaging: constrain input, use appropriate controls, provide sensible defaults, make destructive actions undoable rather than confirmable where possible.
