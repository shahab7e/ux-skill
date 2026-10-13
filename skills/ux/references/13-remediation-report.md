# 13 — Remediation Report for a Technical Team

Use this instead of the general report shape in file 11 §6 when the audience is the **design and development team** who will implement the fixes — even when the client commissioning the audit is the business owner.

The difference is fundamental. A report for a business owner argues *that* there is a problem. A report for an implementing team is a **work order**: every finding must be specific enough to become a ticket without anyone coming back to ask what was meant.

## The test for every finding

Before writing a finding, check that a developer who has never spoken to you could implement it correctly from the text alone. If they'd have to guess at the intended behavior, the finding isn't finished.

Concretely, each finding must survive these questions:

- Where exactly is this? (screen, URL, component, element)
- What does it do now?
- What should it do instead?
- How will we know it's fixed?
- Who does it — designer, frontend, backend?
- How bad is it, and what does it depend on?

## Writing voice: this is a human review, not a QA log

Everything in section 3 of file 11 — computed styles, console traces, resource timings, `getBoundingClientRect()` output — is **evidence you gather to become certain**, not text that belongs in the report. A reviewer who actually used the product describes what they saw and why it matters, in sentences a business owner and a developer can both read without translating. Raw devtools output is for the appendix or the `scan/` files, referenced by ID — not for the body of a finding.

**The rule:** give a precise number only when it *is* the finding — it fails a named threshold (44px, 4.5:1, 16px), or it sizes the impact (11 duplicate calls, 6 clicks instead of 2). One number, stated once, in a sentence. Never a table of properties to prove what a screenshot already shows.

**Before (evidence dump):**

> **Current behavior**
> Computed styles are identical for both tile types:
> ```
> background-color : rgba(255, 255, 255, 0.68)   ← both
> border           : 0.8px solid rgba(255,255,255,0.78)   ← both
> box-shadow       : rgba(15,23,42,0.043) 0px 10px 28px   ← both
> cursor           : pointer                      ← both
> ```

**After (human review):**

> **Current behavior**
> Tiles that go straight to a page and tiles that open a menu look identical — same background, same border, same shadow. The only difference is a small count badge on some tiles, and even that isn't reliable: the "Partners" tile, which is a direct link, also shows a badge.

**Before (diagnostic log):**

> ```
> Uncaught TypeError: Cannot read properties of null (reading 'classList')
>   at sidebar.js:106:35
>   at sidebar.js:149:25     ← repeated 5×
> ```

**After:**

> The page throws two recurring JavaScript errors on load — both try to read a property of an element that doesn't exist, in `sidebar.js` (lines 106 and 149; the second fires 5 times). Full trace in the appendix, item DASH-08.

**Before (network waterfall):**

> ```
> 11x  /api/reminders/due
> 4x   /api/tickets/unread-count
> total: 65 requests, 48 unique URLs
> ```

**After:**

> A single dashboard load calls the reminders endpoint eleven times — a pattern that doesn't fit normal polling and looks more like a bug in how events are wired. Overall, more than 60 requests go out for one page load.

More rules that keep the report readable:

- **"What works" is praise in plain language**, not a scoreboard. "Contrast held up everywhere I checked" beats listing every ratio measured. Mention a specific number here only if it's remarkable in either direction.
- **Quote code only for what should change** — a class name to target, the exact copy to ship, an attribute to add. Don't quote code to *prove* what's currently there; describe it, and point to the appendix if proof is needed.
- **Location stays precise** (screen, URL, selector) — that's navigation, not evidence, and developers need it to find the spot. Keep it.
- **Acceptance criteria stay checkable but read in plain terms first.** "The close button can be tapped accurately on a phone" is the criterion; "hit area ≥ 44×44px" can follow in parentheses if the plain version alone would be ambiguous to a developer. Don't reach for a code-level check (`getBoundingClientRect()`, a specific DOM query) unless plain language genuinely can't pin down what "done" means.
- **If a finding has more monospace lines than sentences, rewrite it.** That ratio is the tell that evidence leaked into the write-up instead of being distilled from it.
- **Numbers still belong** — this isn't about vagueness. "6 clicks instead of 2", "40 calls a day", "11 duplicate requests" are exactly the numbers that make a finding land. The cut is specifically against raw tool output standing in for a sentence.

Use this structure for every finding, without exception. Consistency is what makes a long report usable.

```
### [ID] — [Short imperative title]

**Location:** [screen name] · [URL or route] · [component/element]
**Severity:** Blocker | Serious | Moderate | Minor
**Discipline:** Design | Frontend | Backend | Content | Product decision
**Effort:** S | M | L
**Affects:** [which roles / how often the affected task occurs]

**Current behavior**
What happens now, in plain sentences, as someone who actually used it would
describe it. Screenshot reference. A precise number only if it's the crux
of the problem (see "Writing voice" above) — never a raw computed-style,
console, or network dump.

**Why this is a problem**
The user consequence, and the principle behind it. One or two sentences —
not a lecture.

**Expected behavior**
The specific target state. Be concrete: exact copy where copy is the fix,
exact interaction where interaction is the fix.

**Acceptance criteria**
- [ ] Verifiable statement 1, in plain terms — add the technical check in
      parentheses only if plain language would leave the developer guessing
- [ ] Verifiable statement 2

**Notes / dependencies**
Related findings, a decision the client must make, a technical
prerequisite, or an open question.
```

### Rules that keep this honest

- **Acceptance criteria must be checkable by someone else**, not by you. "Improve the hierarchy" is not a criterion. "The primary action is the only filled button on the screen and appears above the fold at 1440×900" is.
- **Where the fix is copy, write the copy.** Don't say "improve the error message"; give the exact string, in the interface's language.
- **Where the fix is a rule, state the rule.** "Table state (filters, sort, scroll position, page) is preserved when returning from a detail view" is implementable; "fix the table" is not.
- **Where the fix requires a client decision, say so and stop.** Mark it `Product decision` and present the options with tradeoffs rather than picking silently.
- **Never bundle.** One finding, one problem, one ticket. If a screen has six problems, that's six findings.
- **Reference screenshots by filename**, and include annotated ones where the issue is positional.
- **Cross-reference, don't repeat.** A template-level defect appearing on 30 screens is one finding, with the affected screens listed, and a note that fixing the component fixes all of them.

## Severity, honestly defined

Define these at the top of the report so the team and the client read them the same way:

| Severity | Definition |
|---|---|
| **Blocker** | A task cannot be completed, data is lost or corrupted, or the system is unusable for a role. Fix before anything else. |
| **Serious** | The task can be completed but many users will fail, abandon, or produce bad data. Significant daily time cost. |
| **Moderate** | Friction, confusion, or wasted time on a task that still completes correctly. |
| **Minor** | Polish, consistency, and small refinements. Real, but nobody is blocked. |

Severity is about **user and business consequence**, not about how offensive it looks to a designer. A beautiful screen that silently loses filter state is Serious; an ugly button that works is Minor. Being disciplined about this is what makes the team trust the priority order.

Effort (S/M/L) is your estimate from the outside and should be labeled as such — the team will correct it, and that's fine. Its purpose is to let the client see the quick wins.

## Report structure

```
# UX/UI Audit — [product name]
Version | Date | Auditor | Environment audited (URL, staging/production)
Scope: roles audited · screens covered · viewports · what was excluded and why

## 1. How to read this report
- The finding format
- Severity definitions
- Discipline labels
- How to turn a finding into a ticket

## 2. Executive summary
For the business owner. Half a page maximum, plain language:
- Overall state of the product
- The 3 biggest problems, in business terms (time cost, error risk, adoption)
- What changes if these are fixed

## 3. Remediation plan
The whole audit, ordered for execution. This is what the team works from.

### Phase 1 — Blockers ([n] findings)
### Phase 2 — Serious, high-frequency tasks ([n] findings)
### Phase 3 — Serious, lower-frequency ([n] findings)
### Phase 4 — Moderate ([n] findings)
### Phase 5 — Minor / polish ([n] findings)

Each phase opens with a one-line summary table:
| ID | Title | Screen | Discipline | Effort |

## 4. Findings by screen
The full findings, in the format above, grouped by screen.
Each screen section opens with:
- What this screen is for, and which roles use it
- The primary task it should support
- Screenshot
- Finding IDs on this screen, with severity

## 5. Cross-cutting findings
Component-level and system-wide issues that appear on many screens:
design system inconsistency, table behavior, form patterns, empty and
error states, accessibility, performance, RTL/Persian handling.
These usually deliver the most value per fix — call that out.

## 6. Task walkthroughs
For each primary task per role: the current step-by-step path with the
step count, where it breaks, and the proposed path with its step count.
This is the section that shows the business owner what the fixes buy.

## 7. Product decisions required
Things that are not the team's call: what to do, the options, the
tradeoffs, and a recommendation. The client answers these before the
relevant findings can be implemented.

## 8. Open questions and hypotheses
What couldn't be determined from the outside and needs data, user
research, or a conversation with the team. Explicitly separated from
findings so nobody implements a guess.

## 9. Coverage
What was audited, per role and per screen. What was NOT audited and why.
| Screen / route | Role(s) | Viewports | Forms tested | Notes |

## 10. Appendix
Screenshots index, raw scan data, contrast measurements, performance
numbers, the full checklist results.
```

## Section 6 is the one that sells the audit

Task walkthroughs — "logging a call currently takes 6 clicks and 2 page loads; here it is in 2 clicks and 0 page loads" — are what make a business owner understand the value and what give the team a target. Include a before/after step count for every primary task. When possible, multiply by frequency: "×40 calls per agent per day × 8 agents" turns a UX finding into a business case without exaggerating anything.

## Language and terminology

Write the report in the language the client asked for. For a Persian client with a Persian-speaking technical team: **formal written Persian**, with UX/CRO/technical terminology kept in English — usability, affordance, CTA, empty state, skeleton, validation, accessibility, WCAG, focus, hover, breadcrumb, information architecture, LCP/CLS/INP.

Two things stay in English regardless: **exact code, selectors, and property names**, and **the finding IDs**. Two things must be in the interface's own language: **any copy you are proposing** (write the actual Persian string the team should ship), and **any quote of what the interface currently says**.

## Numbering and traceability

- Give every finding a stable ID: `[AREA]-[NN]` — e.g. `TBL-03`, `FRM-11`, `NAV-02`, `A11Y-07`, `PERF-01`.
- Keep IDs stable across report versions. If a finding is withdrawn, mark it withdrawn rather than reusing the number.
- Provide the full finding list as a flat table (or a CSV in the appendix) so the team can paste it into their tracker.

## What not to do in this report

- Don't propose a redesign. Redesigns don't get approved; specific fixes ship. If the structure is genuinely wrong, say so as one clearly-marked structural recommendation and still give the incremental fixes.
- Don't soften severity to be polite, and don't inflate it to seem thorough.
- Don't include a finding you can't specify a fix for. Move it to §8 as an open question.
- Don't blame. Findings describe the product, never the team that built it.
- Don't leave the biggest problem buried at position 47 because it happens to be on the last screen you audited. §3 exists so execution order is not audit order.
