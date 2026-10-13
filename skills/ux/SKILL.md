---
name: ux
description: Usability and UX knowledge base for websites, apps, and AI-driven products, built from "Don't Make Me Think", "Laws of UX", "100 Things Every Designer Needs to Know About People", "Form Design Patterns", "Deceptive Patterns", and "UX for AI". Trigger whenever the user types /ux. Also consult it — without the slash command — whenever the user asks about making a site or app easier to use, reviews or critiques an interface, designs navigation or information architecture, writes UI copy, designs forms, checkout or signup flows, asks about accessibility, mobile usability, conversion drop-off, onboarding, error handling, deceptive patterns and design ethics, usability testing, or designs chat/copilot/agent interfaces for AI products. Also use it for full site UI/UX/CRO audits. Apply this skill's frameworks (Krug's laws, the trunk test, psychological principles, form patterns, accessibility rules, AI interaction patterns, the audit checklist) rather than answering from general knowledge alone.
---

# UX & Usability (ux)

This skill packages the working knowledge of six books on usability, design psychology, forms, ethics, and AI interfaces. It is not a copy of any of them — it's an original, condensed reference to their models, rules, and terminology, organized so only the relevant part gets pulled into context.

Use it whenever helping the user:

- Review, critique, or audit a website, app, screen, or flow
- Decide how to structure navigation or information architecture
- Write or fix interface copy, labels, error messages, and empty states
- Design or debug forms, signup, checkout, search, and filtering
- Diagnose why users drop off, get confused, or fail a task
- Make something accessible, or check it against accessibility expectations
- Design mobile/responsive experiences
- Judge whether a pattern is persuasive or manipulative
- Design chat, copilot, or agent interfaces for AI-driven products
- Plan or run cheap usability testing

## Reference map

Read only the file(s) the request actually needs. Don't load everything.

| File | Covers | Use it when… |
|------|--------|--------------|
| `references/01-core-usability.md` | Krug's laws, scanning/satisficing, billboard design, conventions, omitting words, mindless choices | Any "is this clear?", "why is this confusing?", or general usability question — this is the default starting point |
| `references/02-navigation-and-ia.md` | Persistent navigation, the trunk test, home page jobs, search, breadcrumbs, IA for classic and AI-first apps | Navigation, menus, site structure, "where do users get lost", home page design |
| `references/03-psychology-and-behavior.md` | The 10 Laws of UX + key findings on perception, memory, attention, motivation, and error | Explaining *why* a design works or fails; predictive models (Fitts, Hick, Miller); load, attention, motivation questions |
| `references/04-forms-and-inputs.md` | Labels, validation, error messaging, one-thing-per-page, question protocol, specific field patterns | Forms, signup, checkout, login, search, filters, uploads, long multi-step flows |
| `references/05-accessibility.md` | Accessibility-first rules, WCAG-level expectations, screen readers, keyboard, contrast, motion | Any accessibility question, and as a mandatory cross-check on any form or component design |
| `references/06-mobile.md` | Mobile constraints, touch targets, responsive tradeoffs, mobile-first discipline, affordance loss | Mobile/responsive design, small-screen critique, "why is this bad on phone" |
| `references/07-ethics-and-deceptive-patterns.md` | Deceptive-pattern taxonomy, sludge/obstruction, consent, regulation, ethical persuasion line | Reviewing consent flows, cancellation, pricing, urgency/scarcity, subscriptions; any "is this manipulative?" question |
| `references/08-ai-interfaces.md` | LLM interaction patterns, copilot UX, value matrix, designing for uncertainty and error, agentic UX | Chat, copilot, assistant, agent, AI search, AI features in an existing product |
| `references/09-testing-and-process.md` | Do-it-yourself usability testing, resolving team arguments, AI-inclusive design process, research | Planning testing, "how do I validate this", team/process/stakeholder questions |
| `references/10-audit-checklist.md` | End-to-end review checklist across all the above | Full site/app audits, "review my whole product", or when a request spans several areas at once |
| `references/11-site-audit-workflow.md` | How to walk a live site with a browser: crawling and site-map categorization with a confirmation checkpoint, section-by-section execution with checkpoints, evidence collection, form/validation testing, post-submit pages, output language | Auditing a real URL or running app — read alongside file 10 whenever a browser is involved |
| `references/12-dashboards-and-internal-tools.md` | Dashboards, CRMs, admin panels and other internal B2B tools: task inventory, data tables, filtering, operational forms, record pages, pipelines, roles, notifications, Persian/RTL operational specifics | Any audit or design work on a tool people use all day behind a login — replaces marketing-site and CRO thinking |
| `references/13-remediation-report.md` | Report format when the audience is the implementing design/dev team: finding structure, acceptance criteria, severity definitions, phased remediation plan, task walkthroughs | Any audit whose output is a work order for a technical team rather than an analysis for a stakeholder |

## How to use this skill in a response

1. **Identify what's actually being asked.** A "which label should this button have?" question needs file 01. A "review my checkout" request needs 04 + 07 + 10. Don't dump every framework into every answer.
2. **Read the relevant reference file(s) before answering.** They hold the specific rules, thresholds, and decision criteria — not just the general idea.
3. **Diagnose before prescribing.** For critique requests, name what's likely to break for a real user and why (which principle it violates), then give the concrete fix. A list of generic best practices is much less useful than a specific "users won't see this because X."
4. **Prefer concrete, implementable recommendations.** "Move the primary action above the fold and label it with the outcome, not 'Submit'" beats "improve the call to action."
5. **Always cross-check accessibility and ethics** when the answer touches forms, consent, pricing, cancellation, urgency, or any persuasive pattern — even if the user didn't ask. These are the two areas where a fluent-looking answer causes real harm.
6. **Match the product type.** For a dashboard, CRM, admin panel, or any tool used daily behind a login, read file 12 — public-site and conversion advice actively misleads there. For a report aimed at an implementing team, use file 13's format instead of file 11's.
7. **Don't invent findings.** If a recommendation depends on how the user's actual users behave, say what to test rather than asserting what users do.
8. **Answer in the language the user asked in.** This knowledge base is written in English to preserve the field's terminology, but match the user's language in the response. Keeping well-known terms in English (usability, affordance, dark pattern, CTA) alongside a translation on first use is fine and often clearer.
9. **Don't cite the books or this skill to the user.** Use the knowledge the way an expert uses their own, unless the user explicitly asks where a principle comes from.

## Core mental model

Every screen should let a person accomplish what they came for **without stopping to think about the interface itself**. Attention, working memory, and patience are limited and non-renewable; every ambiguous label, unnecessary question, unexplained error, and extra decision spends some of them.

Two tests apply to almost any design question:

- **Clarity:** would a distracted stranger, glancing at this, know what it is, what they can do, and what happens next?
- **Good faith:** is the design working toward what the user came to do, or against it? A pattern that only works because the user misunderstands it is a defect, not a feature — regardless of what it does to the metrics.
