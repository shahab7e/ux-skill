# 09 — Testing & Process

## Why most usability arguments are unresolvable

Teams argue endlessly about whether users like drop-downs, whether people scroll, whether the copy is too long. These arguments don't resolve because everyone is generalizing from themselves.

Two root causes:

1. **"Most users" doesn't exist.** There is no average user. For nearly any design question, some users prefer one thing and some the other. The right question is never "do users like X?" — it's "does *this* X, in *this* context, on *this* page, work well for the people trying to do *this* thing?"
2. **Everyone on the team is a bad proxy for users.** Designers judge by design sensibility, developers by capability, marketers by message, executives by preference. Each is a legitimate professional lens and none of them is what a distracted stranger experiences. Worse, everyone who works on a product knows too much about it to see it fresh again.

The way out is not a better argument. It's cheap evidence.

## Do-it-yourself usability testing

The core claims, which are counterintuitive but hold up:

- **Testing one user is 100% better than testing none.**
- **Testing a small number early and often beats a big study late.** Three users a morning, once a month, finds more fixable problems than one 20-person study a year.
- **Three users find most of the serious problems.** The most severe issues show up in the first two or three sessions; further users mostly re-find them.
- **Recruiting loosely is fine.** Grab people who are roughly plausible. Perfect representativeness matters much less than doing it at all.
- **You don't need a lab.** A room, a laptop, screen sharing for observers, and a recording.

### How to run one

**Setup:** pick one morning a month. Recruit 3 people. Invite the whole team to watch (this is the actual mechanism by which anything changes — watching a user fail is more persuasive than any report).

**Session structure (~1 hour each):**
1. Explain that you're testing the site, not them; there are no wrong answers; think out loud continuously.
2. **Get-it tour**: show the home page or entry screen, don't let them click, and ask — whose site is this, what can you do here, what would you click first? This tests comprehension of the value proposition and the visual hierarchy.
3. **Tasks**: 3–4 realistic tasks phrased as goals, not instructions. "Find a plan that would work for a team of five" — not "click Pricing." Watch, don't help.
4. **Probing at the end**, not during. Don't answer questions mid-task; ask "what would you do if I weren't here?"

**Facilitation rules:**
- Say almost nothing. Silence produces data.
- Never lead ("do you see the blue button?").
- Never explain how it works until the session is over.
- Note *where they hesitate*, not just where they fail — hesitation is the cheapest signal of a thinking-required moment.

**Debrief the same day**, while it's fresh. List problems observed, pick the **three most serious** to fix before the next round, and ignore the rest for now. Fixing three real problems monthly beats a backlog of forty.

### Choosing what to fix

- Prefer **taking things away** over adding things. The instinct after a test is to add explanation, a tooltip, an onboarding step. Usually the right fix is to remove whatever caused the confusion.
- Distinguish **"they didn't see it"** (a visual hierarchy problem) from **"they saw it and misunderstood"** (a wording or model problem). The fixes are completely different.
- Don't fix what only one user hit unless it's severe or you can see why it would generalize.
- Beware "we'll fix it with better copy." Copy rarely rescues a structurally confusing screen.

## Other cheap methods

- **Squint test / trunk test** (file 02) — seconds, no participants.
- **Keyboard-only pass** (file 05) — minutes, finds real defects.
- **Five-second test** — show a screen for five seconds, ask what it was for. Tests the value proposition and hierarchy.
- **First-click test** — where would you click to do X? Where users' first click goes predicts task success strongly.
- **Analytics for "where", testing for "why."** Funnels tell you which step loses people; only observation tells you why.
- **Support tickets and search logs** are free usability research that most teams never read.

## Making it happen in an organization

- **Get people to watch.** Nothing shifts opinion like seeing a real person fail at something the team believed was obvious. A recorded highlight clip is the single most effective artifact.
- **Frame recommendations around risk and cost**, not craft. "This step loses X% of signups" travels further than "this is bad design."
- **Fix small things.** Momentum comes from many small visible improvements, not from a proposed redesign that never gets approved.
- **Don't fight for perfection.** The goal is fewer serious problems, not an ideal product.

## Process for AI-driven products

The classic sequence — research, design, build — breaks down when feasibility depends on data nobody has examined yet.

- **Involve data science from the start.** Design questions ("can we detect this?", "how often will it be wrong?") are answerable only with the data.
- **Use spikes**: short, time-boxed technical investigations that answer a specific feasibility question before design commits to a direction.
- **Prototype with real data and real (wrong) outputs.** A demo built on curated perfect outputs validates nothing.
- **Test the failure paths explicitly**: what the user does when the answer is wrong, incomplete, or refused.
- **The handoff to development includes the value matrix**, not just the screens — the cost of each error type is a design decision, and engineering will make it by default if design doesn't.

### AI in research work

Some research tasks are being automated (transcription, first-pass thematic coding, survey analysis, note synthesis) and some are being augmented (recruiting, screening, generating interview guides, drafting reports). What does not transfer is the part where a human watches a real person struggle. Synthetic users and model-generated personas produce plausible outputs that do not correspond to real behavior — they're an idea generator, not evidence. Use them to prepare, never to conclude.
