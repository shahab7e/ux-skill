# 11 — Live Site Audit Workflow

How to actually walk a live site with a browser and produce a report. Read this **together with `10-audit-checklist.md`** — that file says *what* to look for, this one says *how to gather it and how to write it up*.

## 0. Before touching the browser

Establish, or ask for, three things. Do not guess them from the site's appearance — an audit built on invented assumptions produces confident wrong findings.

- **Who the users are** and what the primary task is
- **What business outcome the site is for** (leads, sales, signups, support deflection)
- **Scope**: which pages, desktop and/or mobile, and whether flows involving login/payment are in scope

If the user hasn't said, ask once, briefly, then proceed with stated assumptions written at the top of the report.

**Interaction scope.** Forms and post-submit pages are a large part of what an audit needs to see, so plan to interact — but agree the boundaries first. Before the walk, confirm with the user:

- Is this **production** or a staging/test environment? Staging removes most of the risk; say so and behave accordingly.
- Which flows may be **submitted**, and which only inspected?
- Are there **test credentials**, a test account, or test card numbers?
- Is there a **safe recipient** for anything that sends a message or email (their own address, a catch-all)?

Then follow the rules in section 3½. Never submit a payment, create a real account, send a message to a real third party, or trigger anything irreversible without the user's explicit go-ahead in the conversation.

## 1. Browser setup

Use whatever browser control is available in the session (a Playwright/Puppeteer MCP, a devtools MCP, or a script you write and run). The workflow below is tool-agnostic — every step is either a navigation, a screenshot, an accessibility/DOM read, or a JavaScript evaluation.

Set up two viewports and use both:

- **Desktop**: 1440×900
- **Mobile**: 390×844, with a mobile user agent and touch emulation

If throttling is available, run at least the home page under a slow-3G / 4× CPU profile — performance findings are otherwise guesswork.

If no browser is available at all, fall back to fetching HTML and say clearly in the report that visual hierarchy, contrast, layout, and interaction could not be assessed. Don't present a source-only review as a full audit.

## 2. Discovery, coverage & element sweep

Don't hand-pick a few pages and call it an audit. **Discover the site first, then decide coverage.** The goal is to behave like a thorough test user who tries everything, bounded only by what's actually reachable and by time.

### Step 1 — Discover every page

Build the URL list mechanically, not from memory:

- Fetch `/sitemap.xml` (and any sitemap index it points to) and `/robots.txt`
- Crawl from the home page: collect every same-origin `href`, follow them, repeat until no new URLs appear
- Include pages only reachable behind login, if credentials are in scope
- Include pages reachable only *through* an action: confirmation pages, filtered states, expanded views, modals with their own URL

```js
// same-origin links on the current page
[...new Set([...document.querySelectorAll('a[href]')]
  .map(a => a.href)
  .filter(h => h.startsWith(location.origin))
  .map(h => h.split('#')[0]))]
```

Run this on each newly discovered page, keep a visited set, and stop when the set stops growing. Record the resulting URL count — it determines the coverage strategy below.

### Step 1½ — Categorize the whole site, then stop and confirm

**Never go straight from discovery into deep auditing.** Before spending any time on judgement work, turn the raw URL list into a categorized map and put it in front of the user.

Group discovered URLs into sections that make sense for the product — by path prefix, by role/area, or by the template clusters from §2½ if you've already run that. For a CRM this typically looks like: auth (login/reset), dashboard/home, each major module (leads, properties, deals, contacts, reports), settings/admin, and anything role-gated.

Present a short categorized summary — not the full audit, just the map:

```
## Site map — [n] pages discovered

| Section | Pages | Templates | Notes |
|---|---|---|---|
| Auth | 3 | 2 | login, forgot-password, reset |
| Dashboard | 1 | 1 | |
| Leads | 4 | 2 | list, detail, create, import |
| Properties | 6 | 3 | list, detail, create, edit, gallery, print view |
| Deals / Pipeline | 3 | 2 | |
| Settings | 8 | 4 | includes 5 admin-only pages — confirm role access |
| Reports | 2 | 2 | |

Proposed order: Auth → Dashboard → Leads → Properties → Deals → Settings → Reports
Proposed batching: one section per checkpoint (see below)
```

Then **stop and ask**: does this match the site as the user understands it, is the proposed order right, is anything missing (pages behind a role you don't have, a module you didn't mention), and is the batching granularity right. Don't proceed to Step 3 until the user confirms or corrects the map.

This step exists for two reasons: it catches scope surprises early (discovering the CRM has 200 admin pages nobody mentioned, before you've spent time auditing them), and it's what makes the checkpoint workflow below possible — you can't check in "after the Leads section" if nobody agreed Leads was a section.

### Step 2 — Choose coverage honestly

| Site size | Approach |
|---|---|
| **Under ~40 unique pages** | Audit **every** page. No sampling. |
| **~40–150 pages** | Audit every *unique template*, plus every page of every flow, plus at least 3 instances of each repeated template (they differ more than you expect — long titles, missing images, out-of-stock states, empty categories). |
| **Over ~150 pages** | Switch to the two-tier strategy in section 2½ — automated scan across every page, judgement audit sampled by template. State the sampling in the report. |

Templates, not pages, are the real unit for large sites: 400 product pages usually have one problem, not 400. But **never** assume two pages share a template without checking — audit the extremes (longest content, shortest content, most images, none, oldest, newest).

Pages that must be in every audit regardless of size:

1. Home / landing
2. Every primary and footer navigation destination
3. Listing/category and search results (including a **zero-results** search)
4. Detail pages — several, including edge cases
5. Every step of every conversion flow (cart → checkout, signup, contact, application)
6. Login, registration, password reset, logout
7. Pricing / plans
8. Utility pages: contact, support, FAQ, about, terms, privacy
9. A **404** page — request a URL that doesn't exist
10. Every **post-submit / confirmation page** (see section 3½)
11. For apps: post-login landing, each main working screen, settings/account, every create/edit flow, and the empty state of each

### Step 3 — Sweep every interactive element on each page

On each page, don't only look — **enumerate and try things**. Pull the full inventory of interactive elements first, so nothing is missed by eye:

```js
[...document.querySelectorAll('a,button,input,select,textarea,summary,[role="button"],[role="tab"],[role="menuitem"],[onclick],[tabindex]:not([tabindex="-1"])')]
  .map((el,i) => ({
    i,
    tag: el.tagName,
    role: el.getAttribute('role') || '',
    text: (el.innerText || el.value || el.getAttribute('aria-label') || '').trim().slice(0,60),
    href: el.getAttribute('href') || '',
    visible: !!el.offsetParent
  }))
```

Then work through the list. For each element, decide which bucket it's in and act:

- **Navigation link** → follow it. Record: does it go where the label promised? Does the destination page name match the link text? Any dead link, redirect chain, or off-site jump that wasn't signposted?
- **In-page control** (tab, accordion, dropdown, modal, carousel, tooltip, filter, sort, toggle, "show more") → **activate it**. Does it do what the label implies? Can it be closed? Does it trap focus? Does it work by keyboard? Does it change the URL, and does Back then behave sanely?
- **Form control** → section 3½.
- **Consequential action** (submit, buy, delete, send, subscribe) → section 3½ rules; ask before firing.
- **Dead control** — anything that looks interactive and does nothing, or is disabled with no explanation. Every one of these is a finding.

Also deliberately probe the things a real user hits by accident:

- Browser **Back** after every state change, especially after submit and after opening a modal
- **Refresh** mid-flow — is progress lost?
- **Deep-link** directly into step 3 of a flow — does it handle it or break?
- Open something in a **new tab** — does the app assume single-tab state?
- **Double-click** the primary action
- Paste into fields that try to block pasting
- Resize the window mid-interaction

### Step 4 — Track coverage

Keep a running table as you go; it becomes the audit's coverage appendix and stops pages being silently skipped.

| URL | Template | Viewports done | Interactive elements tried / total | Forms tested | Notes |

Note the URL of every page you audit; the report references pages by URL, not by nickname.

### Step 5 — Work section by section, checkpoint between each

Don't run the whole audit as one uninterrupted pass. Once the site map is confirmed (Step 1½), work through it **one section at a time**, in the agreed order, and stop after each one.

After finishing a section:

1. Write its findings to disk (`findings/<section>.md`) — don't hold them in context waiting for the end.
2. Update the coverage table.
3. Post a short checkpoint to the user:

```
✅ Auth — done. 3 pages, 8 interactive elements, 1 form tested.
   4 findings (1 Serious, 3 Minor). Nothing blocking discovered.

Next: Dashboard (1 page). Continue?
```

4. Wait for a go-ahead before starting the next section. A plain "continue" / "ادامه بده" is enough — this isn't a full re-approval of scope, just a heartbeat.

Why this matters more than it looks: a long audit run has no natural place to resume if it's interrupted — a session limit, a network drop, a browser crash — unless you've already been checkpointing. Working in confirmed, disk-persisted sections means a cutoff loses at most one section's work, not the whole audit, and the user always knows exactly where things stand rather than waiting on a silent multi-hour run.

If a section turns out to be much bigger than the site map suggested (a "Settings" section that's actually 40 pages), say so at that section's checkpoint and ask whether to split it into sub-batches before continuing, rather than absorbing the surprise silently.

Only after every confirmed section is done — and the user has seen every checkpoint — assemble the final report per file 13.

## 2½. Running a large audit without falling over

A big site breaks the naive approach in two ways: the session runs out of context long before the pages run out, and quality drifts — page 80 gets audited to a different standard than page 3. Solve both by **never holding the whole audit in context** and by **splitting cheap machine checks from expensive judgement**.

### Persist everything to disk, immediately

Create a working directory at the start and write to it continuously. Context is scratch space; the directory is the audit.

```
audit/
  urls.json          # every discovered URL + status + template id + audited flag
  templates.json     # template clusters and their representative URLs
  scan/              # automated scan results, one JSON per URL
  findings/          # one markdown file per page or template
  screenshots/
  coverage.csv       # the coverage table, appended as you go
  REPORT.md          # assembled at the very end
```

Rules that make this work:

- **Write findings for a page before moving to the next one.** Never carry observations forward in your head.
- **Never load more than one page's raw evidence into context at a time.** Read it, write the finding, drop it.
- **Keep a checkpoint** in `urls.json` (audited: true/false) so the audit can stop and resume across sessions. Large audits *will* span sessions; plan for it rather than being surprised.
- Assemble `REPORT.md` at the end from the files, not from memory.

### Two-tier coverage

This is the core move. Don't try to do everything everywhere.

**Tier 1 — automated scan, 100% of pages.** Cheap, no screenshots, no judgement. For every discovered URL: load it, run the JavaScript checks from section 3(d)–(e), capture console/network errors and the response status, write one small JSON per page. Machine-checkable defects get complete coverage this way, at trivial cost:

- Missing form labels, missing alt attributes, empty links
- Heading structure violations, missing/duplicate `h1`, missing page title
- Touch targets under 44px
- Broken links, redirect chains, 404s, mixed content, console errors
- Missing `lang`/`dir`/viewport meta
- `div`s acting as buttons
- Page weight and request count

Then **aggregate before reporting**: a defect appearing on 300 pages is one site-wide finding with a count, not 300 findings. This is what keeps the report readable and it's the single biggest lever on a large site.

**Tier 2 — human-judgement audit, sampled by template.** Screenshots, squint tests, task walks, element sweeps, form testing. Expensive, so spend it where it pays:
- Every unique template (one representative + 2 edge cases each)
- Every step of every conversion flow — **never sample a flow**, always walk it end to end
- The pages Tier 1 flagged as worst
- The highest-traffic pages, if the user can supply analytics

### Cluster templates cheaply, before deciding anything

Don't eyeball which pages share a template. Fingerprint them structurally and cluster:

```js
// structural fingerprint of the current page — pages with the same
// fingerprint are almost certainly the same template
(() => {
  const sig = [...document.querySelectorAll('main *, body > *')]
    .slice(0, 400)
    .map(el => el.tagName + (el.className && typeof el.className === 'string'
      ? '.' + el.className.trim().split(/\s+/).slice(0,2).join('.')
      : ''))
    .join('>');
  // cheap hash so it's storable
  let h = 0; for (const c of sig) h = (h * 31 + c.charCodeAt(0)) | 0;
  return { hash: h, elements: sig.length, url: location.pathname };
})()
```

Group URLs by hash. Each group is a template. Pick a representative plus the two most extreme members (longest and shortest content, most and fewest images). On a 500-page e-commerce site this typically collapses to 8–15 templates — a tractable Tier 2 workload.

### Parallelize with subagents

In Claude Code, run Tier 2 as separate subagent tasks — one per template or per flow, **within the confirmed section currently being worked** (Step 1½/5 above). Each subagent gets:
- The template's representative URLs
- The relevant reference files (this one, plus 10, plus whichever of 01–08 apply)
- Instruction to write `findings/<template>.md` in the standard finding format and return only a short summary

This keeps each subagent's context small and stops the main session from accumulating 40 pages of raw evidence. Tier 1 should be a script you write and run, not a subagent per page. Fan subagents out inside one section, gather their results, post that section's checkpoint, then move to the next — don't fan out across the whole site at once, or the checkpoint workflow loses its purpose.

### Budget the element sweep

Sweeping every interactive element on every page is what actually explodes on a large site. Bound it:

- Sweep **every** element on template representatives and on every flow step
- On other pages, sweep only elements that are **unique to that page** — diff the interactive-element list against the template representative and try only what's new
- Global chrome (header, footer, cookie banner, nav) is swept **once**, not per page

### Bail-out conditions

Say these out loud rather than silently degrading:

- If discovery finds more URLs than the agreed scope, stop and report the count, the template breakdown, and a proposed coverage plan before continuing.
- If the site requires login for most content and credentials weren't provided, stop and ask.
- If crawling is being rate-limited or blocked, say so — a partial crawl presented as complete is worse than no crawl.
- Add a delay between requests and respect `robots.txt`; an audit that gets the auditor's IP banned helps nobody.

### What goes in the report

State the strategy explicitly near the top: how many URLs were discovered, how many templates, what got Tier 1 vs Tier 2, and what wasn't audited. A large-site audit is a sampling exercise and the report should say so plainly — a reader who thinks all 500 pages were hand-reviewed will draw wrong conclusions from the finding counts.

## 3. Per-page evidence collection

For every page in the inventory, gather this before forming any opinion. Doing it in a fixed order is what makes the audits repeatable.

**a. Full-page screenshot** at both viewports. Also capture the above-the-fold view separately — the fold view is what the five-second test judges.

**b. Accessibility / DOM snapshot** — the semantic structure: landmarks, headings, links, buttons, form controls and their accessible names. This is what reveals missing labels, heading-level jumps, div-buttons, and unnamed links.

**c. Text content** — for copy, terminology consistency, and content volume.

**d. Structural facts via JavaScript.** Run these in the page and record the results:

```js
// headings outline
[...document.querySelectorAll('h1,h2,h3,h4,h5,h6')]
  .map(h => `${h.tagName} ${h.textContent.trim().slice(0,80)}`)

// images missing alt decisions
[...document.images].filter(i => !i.hasAttribute('alt')).map(i => i.src)

// form controls without an accessible label
[...document.querySelectorAll('input,select,textarea')].filter(el => {
  if (el.type === 'hidden' || el.type === 'submit') return false;
  const id = el.id && document.querySelector(`label[for="${CSS.escape(el.id)}"]`);
  return !(id || el.closest('label') || el.getAttribute('aria-label') || el.getAttribute('aria-labelledby'));
}).map(el => el.name || el.id || el.outerHTML.slice(0,100))

// links whose text carries no meaning out of context
[...document.querySelectorAll('a')]
  .map(a => (a.innerText||a.getAttribute('aria-label')||'').trim())
  .filter(t => /^(here|click here|read more|more|learn more|link|>|→)$/i.test(t))

// interactive elements that aren't real buttons/links
[...document.querySelectorAll('[onclick]:not(button):not(a),div[role="button"],span[role="button"]')].length

// page facts
({
  title: document.title,
  lang: document.documentElement.lang,
  dir: document.documentElement.dir,
  viewport: document.querySelector('meta[name=viewport]')?.content,
  h1Count: document.querySelectorAll('h1').length,
  imgCount: document.images.length,
  wordCount: document.body.innerText.trim().split(/\s+/).length
})
```

**e. Touch target check** (mobile viewport only):

```js
[...document.querySelectorAll('a,button,input,select,[role="button"]')]
  .map(el => ({ el: (el.innerText||el.name||el.id||el.tagName).slice(0,40), r: el.getBoundingClientRect() }))
  .filter(x => x.r.width > 0 && (x.r.width < 44 || x.r.height < 44))
  .map(x => `${x.el} — ${Math.round(x.r.width)}×${Math.round(x.r.height)}`)
```

**f. Contrast spot-check.** Automated contrast math over a whole page is unreliable (gradients, images, overlays). Instead, sample the specific things that usually fail — body text, placeholder text, secondary/muted text, disabled states, link text on colored backgrounds, text over images — reading computed colors, and confirm against the screenshot:

```js
const el = document.querySelector('SELECTOR');
const s = getComputedStyle(el);
({ color: s.color, bg: s.backgroundColor, size: s.fontSize, weight: s.fontWeight })
```

**g. Keyboard pass** on the conversion flow at minimum: Tab through, record the focus order, and note anywhere the focus indicator is invisible or focus gets trapped. Take screenshots at a few focus positions — an invisible focus ring only shows in a screenshot.

**h. Performance signals** where the tool allows: LCP, CLS, INP if measurable; otherwise total transferred bytes, number of requests, and largest image sizes. Even the crude version supports a real finding.

**i. Console and network errors** — JS errors, failed requests, and mixed content are real defects and cheap to capture.

## 3½. Testing forms and post-submit pages

Most conversion loss lives in forms, and most form defects only appear when you actually type into them. Reading the HTML tells you a label is missing; it doesn't tell you that the error message is wrong, that the input gets wiped, or that the confirmation page says nothing useful.

### What may be done without asking

These are non-destructive and safe on production. Do them freely:

- Focus and Tab through every field
- Type into fields, then clear them
- Trigger client-side validation by blurring an empty or malformed field
- Open selects, date pickers, and autocompletes to see their options
- Read the accessible name, hint text, and error text attached to each control
- Inspect `type`, `inputmode`, `autocomplete`, `required`, `pattern`, `maxlength`
- Attempt submit on an **empty** form (this is the single most informative test — it reveals the whole error-handling design and almost never has side effects)

### What needs the user's go-ahead first

Ask in the conversation, name the specific action, and wait for a clear yes:

- Submitting any form that sends a message, creates a record, or emails someone
- Creating an account or logging in
- Anything that charges money or reserves inventory
- Newsletter signups, contact forms, demo requests, applications
- Any action that can't be undone

Once approved, use obviously-fake but valid-shaped data so the recipient can tell it's a test: name "Test Test", subject/message beginning with "UX audit test — please ignore", the user's own email or a supplied test address, and the standard test card numbers on payment sandboxes only. Record in the report exactly what was submitted, where, and when, so it can be cleaned up.

### The validation walk

For each form in scope, run this sequence and record what happens at each step:

1. **Submit empty.** Does anything happen at all? Is there an error summary at the top? Does focus move to it? Is each error linked to its field? Is the page title updated? Is the message in plain language, or a code?
2. **Submit with one invalid field** (bad email, short password, wrong format). Same checks, plus: is the *other* valid input preserved?
3. **Submit with valid data in an unexpected format** — spaces in a card or phone number, uppercase email, a date typed as `1404/06/18` vs `2026-09-09`, a name with a space or an apostrophe, a Persian/Arabic numeral. Rejecting these is a Postel's Law failure and a very common finding.
4. **Check inline validation timing.** Does an error appear while you're still typing, before you could have finished? That's a defect, not a feature.
5. **Check the field-level experience**: does the label stay visible while typing? Does the mobile keyboard match the field type? Does autofill work? Can you paste into the password field?
6. **Check the submit button**: does it double-submit? Does it disable and show progress? Is it disabled before the form is valid, with no explanation of what's missing?
7. **Keyboard-only**: complete the whole form without a mouse, including any custom control (date picker, autocomplete, file upload, multi-select). Custom controls are where keyboard support usually breaks.
8. **Mobile viewport**: with the on-screen keyboard open, is the active field and its label still visible, or is it under the keyboard?

Screenshot each error state. Error screens are where the biggest findings usually are, and they're invisible in a normal page capture.

### Analyzing the post-submit page

The page after submission is routinely unowned by anyone and routinely terrible. Audit it as its own page:

- Does it **confirm what happened**, specifically? ("Your message was sent" beats a bare "Thank you"; "Order #1042 confirmed, we'll email you at x@y.com" beats both.)
- Does it say **what happens next and when** — response time, delivery date, next step?
- Does it give a **reference** the user can quote (order number, ticket ID)?
- Is there **anything to do next**, or is it a dead end that dumps the user back on the home page?
- Can the user **get back** to what they were doing?
- Is the confirmation **also** delivered somewhere durable (email, account area), or does closing the tab lose it?
- For multi-step flows: is progress **saved** if the user leaves and returns?
- Does the browser Back button after submit cause a re-submission or a broken state?
- Is anything sensitive exposed in the **URL** (email, token, order details in query parameters)?

The same applies to zero-results search pages, empty states after filtering, and post-login landing screens: any screen that only exists after an action needs to be captured and reviewed, and these are exactly the screens a source-only audit never sees.

### Flows you can't complete

If a flow needs payment, a real account, or data you don't have, audit as far as you can and be explicit about where you stopped. Then inspect the remaining steps by other means where possible — the checkout markup, the client-side validation rules, the step indicators — and label those findings as inferred from code rather than observed. Don't silently omit a flow because it was hard to reach; say in the report that it wasn't audited and why.

## 4. Judgement passes

Evidence alone isn't an audit. After collecting, run these deliberately per key page:

- **Five-second judgement**: look only at the above-fold screenshot. What is this, and what would you do next? If you can't answer, that's a top-severity finding.
- **Squint pass**: blur/downscale the screenshot. Does the primary action still dominate? What actually stands out — is it the right thing?
- **Trunk test** (file 02) on 2–3 deep pages.
- **Task walk**: actually attempt the primary task end to end, counting steps, decisions, and moments of hesitation. Where you hesitate, a user fails.
- **Checklist sweep** using file 10, section by section.
- **Ethics sweep** using file 07 — pricing disclosure, consent, cancellation symmetry, fake urgency, pre-ticked boxes. Do this every time, even unasked.

## 5. Turning observations into findings

A finding is only useful if it has all five parts. Drop anything that can't fill them in:

1. **Where** — page URL and the specific element or region
2. **What the user experiences** — the failure, in behavioral terms
3. **Why it happens** — the principle being violated (name it)
4. **Severity** — blocker / serious / moderate / minor
5. **The fix** — concrete and implementable

Rules that keep the report honest:

- **Separate findings from hypotheses.** Anything that depends on how real users behave is a hypothesis. Label it and name the test that would settle it.
- **Don't infer motives.** Report what's there, not why you think they did it. "Shipping cost appears at step 4" — not "they're hiding shipping cost to boost conversion." Ask if the reason matters.
- **No generic best practices.** Every finding must point at something on the actual site.
- **Deduplicate.** One template-level problem repeated on 30 pages is one finding, noted as site-wide.
- **Say what's good, briefly.** Two or three lines. It calibrates the rest and stops the report reading as indiscriminate.

## 6. Report template

**If the report's audience is the implementing design/dev team rather than a business stakeholder, use `13-remediation-report.md` instead of the template below.** The structure there is a work order — every finding carries expected behavior, acceptance criteria, discipline, and a stable ID. It also sets the writing voice: the raw evidence gathered in sections 2–3½ above (computed styles, console traces, network timings) belongs in that report's appendix, not in the finding text — the finding itself reads like a human reviewer's description, not a diagnostic log.

Use this structure exactly.

```
# UX/UI Audit — [site name]
Date | Pages audited | Viewports | Tools used

## Assumptions
- Target users:
- Primary task:
- Business goal:
- (mark anything assumed rather than confirmed)

## Executive summary
3–5 sentences: overall state, and the single biggest problem.

## Priority actions
The 5–10 things to do first, ordered by impact ÷ effort.
| # | Action | Page(s) | Severity | Effort |

## Findings by page

### [Page name] — [URL]
[above-fold screenshot reference]
Purpose of this page | Primary action it should drive

**Issues**
| # | Finding | Severity | Why it matters | Recommended fix |

**What works**

### [next page…]

## Site-wide findings
Template-level and cross-cutting issues (navigation, accessibility, performance, ethics).

## Hypotheses to test
| Question | Suggested test | What it would settle |

## Coverage
URLs discovered | templates found | pages given Tier 1 automated scan | pages given Tier 2 judgement audit

| URL / template | Tier | Viewports | Elements tried / total | Forms tested | Notes |

State explicitly what was NOT audited and why.

## Appendix
Full checklist results, raw data (contrast values, touch target list, performance numbers), screenshots.
```

## 7. Output language and terminology

Write the report in the language the user asked in. When the user asks in Persian, write the report in **formal written Persian** and keep UX/CRO terminology in English — usability, affordance, CTA, conversion rate, above the fold, dark pattern, heuristic, information architecture, LCP/CLS/INP, WCAG. Don't translate these into unfamiliar coinages; a translated term that the client's developers won't recognize is worse than the English one.

Keep severity labels and the table structure consistent across audits so reports are comparable over time.

## 8. Common failure modes of the auditor

- Auditing the source without ever looking at the rendered page — misses everything visual
- Auditing desktop only — most traffic is mobile
- Producing 60 findings of equal weight; nobody acts on that. Prioritize ruthlessly.
- Asserting user behavior with no evidence
- Skipping the conversion flow because it needs interaction — that's where the money is
- Never triggering an error state, so the whole error-handling design goes unaudited
- Auditing a form but not the page that comes after it
- Looking at pages instead of *using* them — a link you didn't click and a tab you didn't open are untested
- Assuming pages share a template without verifying, and missing the edge cases (empty, overflowing, out-of-stock)
- Silently sampling a large site without saying so in the report
- Holding a large audit in context instead of writing to disk — quality drifts and the session dies mid-way
- Reporting one template-level defect 300 times instead of once with a count
- Skipping the ethics sweep because the client didn't ask
- Recommending a redesign instead of fixes. Redesigns don't get approved; fixes ship.
