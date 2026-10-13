# 04 — Forms & Inputs

Forms are where most conversion is lost and where accessibility failures are most common. Every recommendation here should be read as accessibility-first: an inaccessible form is a broken form, not a form with a missing feature.

## Before designing the form: the question protocol

For every field, ask **why does this question exist?**

- What is the answer actually used for?
- Who inside the organization uses it, and what would break without it?
- Can it be derived (from the postcode, from the account, from context) instead of asked?
- Can it be asked later, when it's relevant, instead of now?
- Is it required, or "required" because someone wanted it?

Most form improvements come from deleting fields, not restyling them. A field that costs the user thought and typing, and delivers marginal internal value, is a net loss. Deferring optional questions past the point of commitment usually raises completion without losing the data.

## Labels

- **Every input that accepts data needs a real `<label>`**, programmatically associated (`for` matching `id`). This is not optional and not decorative: sighted users read it, screen readers announce it, and clicking the label focuses the field — a bigger hit area for motor-impaired users.
- Submit buttons don't need an auxiliary label; the button text is the accessible name.
- **Never use a placeholder as a label.** Placeholders disappear on focus, so people lose the label exactly when they're using the field; they're low-contrast by default, look like pre-filled content, aren't reliably announced by assistive tech, and can't be checked while reviewing the completed form.
- **Float/floating labels** trade clarity for space: the label ends up tiny, the empty-state still looks like a placeholder, and long labels break. Prefer a normal label above the field.
- Label **above** the field, left-aligned (in RTL languages, right-aligned) — a single vertical scan line, no ambiguity about which label belongs to which field, no problems with long labels or narrow screens.
- Label text should be short and in the user's words. Push explanation into hint text, not into the label.

## Hint text

- Use for genuinely necessary guidance (password rules, why you need a phone number, format constraints you can't remove).
- Put it **above the input**, below the label, so it's read before use and isn't hidden by autocomplete overlays or the on-screen keyboard.
- Associate it with the field (`aria-describedby`) so screen readers announce it.
- Don't repeat what the label already says.

## Choosing the right control

- **Text input** for short free text. Set `type` correctly (`email`, `tel`, `url`, `number` with care) — it changes mobile keyboards and enables native validation, but `type="number"` is unsuitable for card numbers, phone numbers, and postcodes (spinners, silent character rejection); use `type="text"` with `inputmode="numeric"` for those.
- **Radio buttons** for one-from-few, all options visible. Prefer radios to a select when there are only a handful of options.
- **Checkboxes** for independent yes/no or many-from-many.
- **Select menus** are a last resort: they hide options, require a click plus a scan, behave inconsistently across platforms, and are painful on mobile. Use them only when there are many options and the user knows what they're looking for. Consider a text input with autocomplete instead.
- **Date inputs:** for a known date (birth date), three separate text fields (day/month/year) beat a date picker. For an unknown or near-term date (booking, appointment) where relationships between dates matter, a calendar picker earns its complexity.
- **Toggles/switches** imply an immediate state change; checkboxes imply a value submitted later. Don't mix the semantics.
- **Autocomplete** helps only when the underlying dataset is known and the matching is forgiving; a strict autocomplete that rejects valid input is worse than a plain text field.
- Set `autocomplete` attributes (`name`, `email`, `street-address`, `cc-number`) so browsers and password managers can fill fields — a large, invisible usability win.

## Required vs. optional

Mark whichever is rarer, and mark it in **text**, not by an asterisk alone (an asterisk requires a legend the user must find and remember). If most fields are required, mark the optional ones "(optional)".

## Validation and error messaging

The core rules:

- **Validate on submit**, not aggressively as the user types. Inline validation that fires mid-typing tells people they're wrong before they've finished being right. If you validate inline, do it on blur, and never show an error for a field the user hasn't finished.
- On error, show an **error summary at the top of the form**: a heading naming the problem, and a list of links, one per error, each jumping to the field. Move focus to the summary so screen reader users hear it.
- Also show the **error inline, next to its field**, with the same wording as the summary.
- Mark the field with `aria-invalid` and associate the error text with the input.
- Update the page `<title>` to indicate an error, so it's announced and visible in tabs.
- **Never** clear the user's input on error. Never make them retype anything.
- Error text says **what's wrong and what to do**: "Enter your email address in the format name@example.com" beats "Invalid email". Avoid "please", avoid blame, avoid jargon and codes.
- Don't rely on color to signal error state; add an icon, text, and a border-width change.
- Success/confirmation deserves the same care: confirm what happened and what happens next.

## Buttons

- Buttons look like buttons; links look like links. Don't style a link as a button unless it navigates and don't style a button as a link.
- One primary action per screen, visually dominant. Secondary actions visibly subordinate; destructive actions visually distinct and separated.
- Place the primary button **left-aligned with the fields** (in LTR; mirrored in RTL) so it's found on the same scan line the form was filled along.
- Label with the outcome: "Create account", "Pay £34.50", "Send message".
- Disable-on-submit to prevent double submission, and change the label to a progress state.
- **Avoid disabled submit buttons** as a validation strategy: they give no explanation of what's missing, and are often invisible to assistive tech. Let people submit and tell them what's wrong.

## One thing per page

For long or complex forms, put a single question (or a single tightly-coupled group) on each page.

Why it works: less to process at once, faster to load and re-render, easier to track progress, far easier error handling (one error, unmissable), simpler to test and to analyze drop-off per step, and it works better on small screens. The cost is more page transitions — which is cheap compared to the cost of a wall of fields.

Pair it with: a clear back path, saved progress, a progress indicator when there's a defined number of steps, and a review step before final submission.

## Specific flows

- **Registration:** ask for the minimum. Consider whether you need registration at all before the user has committed. A single password field with a "show password" toggle beats "confirm password."
- **Login:** don't obscure the reset path, allow email *or* username where possible, never limit password length or paste (breaks password managers), and don't reveal which of the two fields was wrong.
- **Checkout:** offer guest checkout. Show total cost including shipping and tax as early as possible — surprise cost at the last step is the single largest cause of cart abandonment. Don't remove navigation without giving a way back and a way to get help.
- **Search:** always allow submit by keyboard, keep the query visible in the results page, handle zero results as a designed state.
- **Filters:** show what's currently applied and make each one removable individually; show result counts; decide deliberately between auto-apply (fast, disorienting) and explicit apply (slower, predictable), and if auto-applying, announce the result-count change to assistive tech.
- **Upload:** the native file input is more accessible than most custom drop zones; if you build a drop zone, keep a real file input inside it. Show progress, allow removal, state accepted formats and size limits *before* the attempt fails.
- **Very long forms:** save progress automatically, allow resuming, and let people see everything they've entered before submitting.

## Quick form audit

1. Which fields can be deleted, derived, or deferred?
2. Does every input have a visible, associated label — no placeholder-as-label?
3. Is the right control used for each question, especially selects and dates?
4. Can the form be completed by keyboard alone, in a sensible order?
5. What happens on error — summary, inline, focus moved, input preserved, plain language?
6. Is the primary action obvious, correctly labeled, and protected from double submission?
7. Are costs, consequences, and required commitments disclosed before the user invests effort?
8. On a phone, with one thumb, on a slow connection — does this still work?
