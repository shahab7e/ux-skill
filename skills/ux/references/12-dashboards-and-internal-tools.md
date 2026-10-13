# 12 — Dashboards, CRMs & Internal Tools

Internal tools are a different design problem from public sites, and applying marketing-site thinking to them produces bad advice. The differences that matter:

- **The user is not a visitor, they're an operator.** They use this all day, every day, and will become expert. Optimizing for the first five minutes at the cost of the next five hundred hours is the classic internal-tool mistake.
- **They can't leave.** Bad usability doesn't cost you a conversion, it costs the company hours, produces data-entry errors, and generates workarounds in spreadsheets outside the system.
- **Efficiency beats simplicity.** Density, keyboard shortcuts, bulk operations, and visible advanced controls are features here, not clutter. Hiding power behind progressive disclosure slows down the people who use it most.
- **Correctness matters more than delight.** A wrong record in a CRM propagates into contracts, commissions, and legal exposure. Error prevention and recoverability outrank aesthetics.
- **Learnability still matters for the first week and for new hires.** The goal is a short learning curve *and* a high ceiling, not one at the expense of the other.

So the questions change: not "is this obvious to a stranger?" but **"how many actions, decisions, and page loads does a competent user need to complete their most frequent task, and where does the system make them think or wait?"**

## Start from the jobs, not the screens

Before auditing any screen, establish the **task inventory**: what does each role actually do, how often, and how urgent is it? Without this, everything looks equally important and the audit produces an undifferentiated list.

For a real-estate CRM, the recurring jobs typically are:

- Capture a new lead (often while on the phone, in under a minute)
- Find a specific contact, property, or deal — fast, from partial memory
- Log an interaction (call, message, viewing) right after it happens
- Schedule and prepare for a viewing
- Move a deal through the pipeline and see what's stuck
- Match a buyer's requirements against available listings
- Upload and find documents attached to a deal
- See "what do I need to do today"
- Manager view: team performance, pipeline health, who's overloaded

Audit each of these as a **task walk** (count clicks, page loads, decisions, and moments of hesitation), then audit the screens. A screen that looks fine can still make the daily job three times slower than it needs to be.

**The highest-frequency task deserves the most optimization.** If agents log 40 calls a day and the call-log flow takes 6 clicks, that's the single most valuable finding in the audit — worth more than any number of styling issues.

## Data tables and lists

Usually the heart of a CRM, and usually where the most findings are.

- **Density is a feature.** Excessive padding means fewer rows visible, more scrolling, more lost context. Offer a density toggle (comfortable/compact) rather than picking one.
- **Column choice must be the user's**, not the developer's: let them add, remove, reorder, and resize columns, and persist that per user. Defaults should show what the most common task needs.
- **Sorting** on every meaningful column, with the current sort visibly indicated, and stable across pagination.
- **Freeze the identifying column** (name, property ref) when horizontal scrolling is possible — otherwise rows become anonymous once scrolled.
- **Row identity must survive**: after an action, the user should still be looking at the same row, in the same scroll position, with the same filters. Losing table state after an edit is one of the most common and most costly defects in internal tools.
- **Alignment**: numbers right-aligned, tabular figures for columns of numbers, dates in a consistent unambiguous format. In a Persian interface, decide explicitly whether numbers render as Persian or Latin digits and be consistent — mixed digits in one column are a real readability problem.
- **Pagination vs infinite scroll**: for work tables, pagination with a visible total count beats infinite scroll — operators need to know how many records there are, and need a stable position to return to.
- **Row actions**: the common one visible inline, the rest in a per-row menu. Don't hide the primary action behind hover only (invisible on touch, invisible to keyboard users).
- **Bulk selection and bulk actions** — select-all-on-page vs select-all-matching-filter must be distinguishable, the count must be shown, and destructive bulk actions must be confirmable and ideally undoable.
- **Empty state**: distinguish "no records exist yet" (teach and offer to create) from "no records match your filters" (say which filters, offer to clear them). Showing the same blank box for both is a defect.
- **Loading state**: skeleton rows, not a spinner that collapses the layout. Preserve the table's height so nothing jumps (CLS).

## Search and filtering

- **Global search must be reachable from anywhere**, ideally by keyboard shortcut, and must search across entity types (contacts, properties, deals) with results grouped by type.
- **Partial and fuzzy matching**: operators search from memory — a partial phone number, half a name, a misspelling, a property code fragment. Exact-match-only search is a top-severity finding in a CRM.
- **Persian search needs normalization**: ی/ي, ک/ك, ه/ة, zero-width non-joiner, Arabic vs Persian digits, and leading/trailing spaces. If searching "محمدی" fails to find "محمدي", that's a data-access bug wearing a UX costume.
- **Filters must show their current state** as removable chips, with a result count and a one-click clear-all.
- **Filter state should be in the URL** so a view can be bookmarked, shared with a colleague, and survive a refresh.
- **Saved views / saved filters** are high value in a CRM — "my active buyers", "viewings this week", "deals stalled over 14 days" — and their absence is a finding.
- Distinguish **auto-apply** (fast, disorienting) from **explicit apply** (predictable). For multi-criteria filters, explicit apply is usually right; announce the result count change either way.

## Forms and data entry in an operational context

Everything in file 04 applies, plus:

- **Speed of entry dominates.** Tab order must follow visual order exactly. Every field reachable and completable by keyboard. No mouse-only controls in a high-frequency entry flow.
- **Inline creation**: let the user create a related record (a new contact while creating a deal) without losing the form they're in. Forcing them to abandon and start over is a major efficiency loss.
- **Autosave or draft preservation** on long forms. Losing 10 minutes of entry to a session timeout or an accidental navigation is a serious finding.
- **Duplicate detection at entry time**, not after — CRMs accumulate duplicate contacts, and the fix is a warning at the moment of typing a matching name or phone.
- **Sensible defaults from context**: the agent who is logged in, today's date, the currently open property. Every field pre-filled correctly is a field not typed.
- **Validate against reality**: local phone number formats, national ID numbers, postal codes, area units (e.g. Iranian mobile numbers written with or without `+98`/`0`, Persian or Latin digits). Accept variations and normalize (Postel's Law) rather than rejecting.
- **Required-field discipline**: a field required at capture time that the agent genuinely doesn't have yet (on a phone call) forces fake data. Fake data is worse than missing data. Audit for this specifically.

## Detail views and record pages

- **The most important information above the fold**, not below a wall of metadata. For a contact: who they are, what they want, last interaction, next action. For a property: key specs, status, price, assigned agent.
- **Activity timeline** as a first-class element — a chronological record of calls, notes, emails, viewings, status changes. Should be filterable by type and should show who did what and when.
- **Related records linked, not just listed**: from a contact, reach their deals, viewings, documents, and matched properties in one click.
- **Inline editing** for fields that change often (status, price, assigned agent) beats opening a full edit form.
- **The next action must be obvious.** A record page that shows history but doesn't answer "what do I do now?" fails the primary job.

## Dashboards and reporting

- **A dashboard should answer questions, not display numbers.** For each widget ask: what decision does this support, and what would the user do differently based on it? Widgets that fail this test are noise.
- **Comparison is what makes a number meaningful**: versus last period, versus target, versus team average. A bare "47" tells nobody anything.
- **Personalize by role.** An agent's dashboard (my leads, my viewings today, my pipeline) is not a manager's dashboard (team performance, conversion by source, stalled deals).
- **Every number should be drillable** to the underlying records. A metric you can't investigate is a metric you can't act on.
- **State the data freshness** — "as of 5 minutes ago" — and be honest about it.
- Chart choice, decluttering, and emphasis: apply file 03's perception principles; if a data-visualization or data-storytelling skill is available in the session, use it for the chart-specific critique.

## Pipeline / kanban views

Real-estate CRMs usually have a deal or lead pipeline. Audit it specifically:

- Are stage names meaningful to the business, and mutually exclusive?
- Is the card showing what's needed to triage without opening it (value, contact, next action, days in stage)?
- Is drag-and-drop the *only* way to move a stage? That's a keyboard and touch accessibility failure — there must be a menu or select as well.
- Does the view surface **stalled** items (days in stage, overdue next action)? A pipeline that doesn't show what's stuck isn't doing its job.
- Are the column counts and totals visible?

## Roles, permissions, and multi-user reality

- **Audit each role separately.** A screen that works for an admin may be full of disabled or empty regions for an agent. Empty and disabled states per role are a real and commonly missed finding area.
- **Disabled controls need explanation.** "You don't have permission" beats a greyed-out button with no tooltip; better still, hide what's irrelevant and explain what's restricted.
- **Concurrent editing**: what happens when two agents edit the same record? Silent last-write-wins is a data-integrity defect, not a UX nitpick.
- **Attribution**: who created, who last modified, when. Essential in a shared operational system.
- **Handoff**: reassigning a lead or deal between agents should carry context, not just change an owner field.

## Notifications and workload

- **Distinguish urgent from informational.** If everything notifies, nothing does, and the user learns to ignore all of it.
- **The "today" view** — what needs my attention now — is the single highest-value screen in an operational tool and is frequently missing or buried.
- **Notification actions**: from the notification, reach the record in one click.
- **Don't rely only on email** for things the operator must act on inside the system, and don't rely only on in-app for things that happen while they're out.

## Performance in operational context

- **Per-interaction latency compounds.** A 2-second table filter is trivial once and intolerable 200 times a day. Measure and report the *frequent* interactions, not just page load.
- **Perceived responsiveness**: optimistic updates for actions that almost always succeed; skeletons that preserve layout; keep the previous data visible while refreshing rather than blanking the screen.
- **Large lists** need virtualization; a table that renders 5,000 DOM rows will feel broken regardless of design.

## Mobile and field use

Real-estate agents work outside the office. Check whether the tool is genuinely usable on a phone for the field subset of tasks:

- Look up a property or contact while standing in front of a building
- Log a viewing outcome immediately after it happens
- Check today's schedule
- Take and attach photos
- Call or message a contact from within the app

The full CRM does not need to work on a phone; these do. If they don't, that's a finding regardless of what the desktop experience is like.

## Persian / RTL operational specifics

- Layout mirrored properly — including tables, pipelines, progress indicators, and directional icons (see file 06)
- Number and digit consistency (Persian vs Latin digits) across tables, inputs, and charts
- **Calendar**: whether the system uses the Jalali or Gregorian calendar, whether it's consistent everywhere, and whether date entry accepts what agents actually type. Mixed calendars in one system is a serious correctness risk, not a preference issue.
- Sorting and searching Persian text correctly (collation, normalization)
- Font choice with correct Persian glyphs and adequate line height for dense tables

## Audit sweep for an internal tool

1. Build the task inventory per role; walk the top 5 tasks end to end, counting steps.
2. For every table: density, columns, sort, filter, state persistence, bulk actions, empty states.
3. For every form: entry speed, keyboard, defaults, duplicates, autosave, validation realism.
4. For every record page: is the next action obvious?
5. For every dashboard widget: what decision does it support?
6. Per role: what's empty, disabled, or broken that isn't for an admin?
7. Frequent-interaction latency, not just page load.
8. The field subset on mobile.
9. Accessibility (file 05) — internal tools are where it's most often skipped and most often legally required.
10. Where are users working around the system? Ask. Spreadsheets kept alongside the CRM are a map of its failures.
