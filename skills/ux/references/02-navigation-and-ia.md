# 02 — Navigation & Information Architecture

## Why navigation carries more weight than it looks

Physical spaces give you a sense of scale, direction, and location for free. A website or app gives you none of that: no sense of how big it is, no sense of direction, no sense of where things are relative to each other. Navigation *is* the substitute for all three. It also tells people what's here at all, and — through its quality — signals whether the people who built this are competent enough to be trusted.

## Persistent navigation

The set of elements that appear on every page (or nearly every page). It does four jobs at once: says what site/app this is, says what's here, tells people how to search, and shows where they are.

Standard components:

- **Site/product ID** — top-left in left-to-right languages (top-right in RTL languages like Persian/Arabic), always links home. It's the frame around everything; it should read as "this is the whole thing," not as one more item in a row.
- **Sections (primary navigation)** — the top level of the hierarchy.
- **Utilities** — a small set (ideally ≤ 4–5 visible) of account, cart, help, contact, language. Distinct in appearance from sections, so they don't compete.
- **Search** — a box, a button, and nothing else. Don't decorate it, don't rename the button to something clever, and make its scope obvious if it isn't searching everything.
- **Home link** — usually via the logo, always present.

**Exceptions:** the home page and dedicated single-task flows (checkout, a long form, a wizard) may legitimately strip navigation to reduce distraction — that's a deliberate choice, not an oversight. In checkout, removing navigation reduces leaks; make sure a way back and a way to get help remain.

## The trunk test

The acid test for navigation. Imagine being dropped, disoriented, onto a random deep page having never seen this product before. Without hesitation you should be able to answer:

1. **What site/app is this?** (identity)
2. **What page am I on?** (page name)
3. **What are the major sections?** (primary navigation)
4. **What are my options at this level?** (local navigation)
5. **Where am I in the scheme of things?** ("you are here" indicators)
6. **How do I search?**

How to run it: pick a random deep page, view it at arm's length or squinted so details blur, and try to find and circle each of the six. If you have to look closely, it fails — the point is that these must pop out on a glance, not survive close study.

This test matters more than ever because most entry is *not* through the home page — people arrive from search, social, ads, email, and shared links, landing deep with no context.

## Page names

Every page needs a name, and it must be:
- **Present** (surprisingly often missing)
- **In the right place** — framing the content it names, visually above/before it
- **Prominent** — the most visually dominant text in the content area
- **Matching what was clicked to get there.** This is the one that breaks silently: if the link said "Returns" and the page says "Product Exchange Policy," the person doubts they arrived where they meant to. Exact match is best; close paraphrase is acceptable; different wording is a defect.

## "You are here" indicators

Highlight the current section in the navigation, and make the highlight strong enough to actually see — subtle changes get missed. Use more than one cue (color + weight + a connected shape). Multi-level structures should highlight at each level.

**Breadcrumbs** work best for deep hierarchies. Put them at the top, use ">" between levels, keep them small, boldface only the last item (the current page), and don't try to make them substitute for real navigation.

**Tabs**, when used, must create the visual illusion that the active tab sits in front of the others — different color/shade and physically connected to the space below. Tabs done wrong stop reading as tabs.

## The home page

The home page has more competing jobs than any other page:

- Say what this is and why the user should be here rather than elsewhere
- Show the hierarchy — what's here and what can be done here
- Provide search
- Tease the good stuff (content and feature promos)
- Show signs of life for anything users are meant to return to
- Show the user what to do first
- Establish credibility

Because everyone in the organization wants a piece of it, the home page is where clarity gets negotiated away. Two things must survive that negotiation:

1. **The value proposition** — a plain, short statement of what this is and what it does for the person. It should be visible immediately, in words a stranger understands. Taglines should be specific and differentiating, not generic ("Enterprise-grade solutions" says nothing).
2. **An obvious starting point.** Whatever most visitors came to do should be the most prominent action on the page.

## Search

- Users split into "search-dominant" and "link-dominant" people; both must be served.
- The search box needs to be where people expect it (top of page, persistent).
- Scope matters: make clear what's being searched if it isn't obvious, and don't offer scope options users can't reason about.
- Zero-results pages are a design surface, not an error: offer corrected spellings, related results, popular items, and a way to browse instead.
- Results need enough information per item to choose without clicking through blindly.

## Information architecture principles

- **Organize around what users want to do, not the org chart.** Departmental structure leaks into navigation constantly and is invisible to the people who work there.
- **Use the user's words.** Category labels should come from user language and search terms, not internal taxonomy.
- **Mutually exclusive, collectively exhaustive** categories where possible; when items genuinely belong in two places, put them in both rather than forcing a wrong single choice.
- **Depth vs. breadth:** deep hierarchies mean more decisions and more chances to guess wrong; extremely broad ones overload the choice. Broad-and-shallow generally beats narrow-and-deep for browsing, but the real constraint is whether each label is unambiguous.
- **Test the labels, not the diagram.** A card-sort or a quick "which of these would you click to do X?" test finds more problems than any amount of debate.

## IA for AI-first applications

The arrival of chat and copilot interfaces does not eliminate information architecture; it changes where it lives.

- The **canvas** pattern (a large open working area with an assistant alongside) is becoming a default shell for AI-driven apps. It's a container, not a structure — it still needs organized destinations behind it.
- A conversational entry point does not remove the need for browsable structure. People who don't know what to ask need something to look at; people who do know still need to navigate results, history, saved items, and settings.
- Instead of navigation *disappearing*, expect it to become **dual-mode**: an intent-driven path (ask for what you want) alongside a browse-driven path (see what's here). Both need to reach the same places.
- Conversation history, saved artifacts, and prior sessions are new IA objects that need real homes — this is a common gap in AI products.
