<div align="center">

# 🧭 UX Skill for Claude

**Give Claude the judgment of a senior UX designer.**

A condensed, battle-tested UX & usability knowledge base packaged as an [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) — for Claude Code, Claude.ai, and the Claude API.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
![Claude Skill](https://img.shields.io/badge/Claude-Agent%20Skill-d97757)
![Files](https://img.shields.io/badge/reference%20files-13-informational)
![RTL ready](https://img.shields.io/badge/RTL%20%2F%20Persian-ready-success)

</div>

---

Ask a general-purpose model to "review my checkout" and you get a list of generic best practices. Load this skill and Claude reviews like a practitioner: it **names the principle being violated, predicts what a real user will do, and gives a concrete, implementable fix** — while cross-checking accessibility and deceptive patterns every time, even when you didn't ask.

## ✨ What's inside

The skill distills the working models of six of the most respected books in the field — *Don't Make Me Think*, *Laws of UX*, *100 Things Every Designer Needs to Know About People*, *Form Design Patterns*, *Deceptive Patterns*, and *UX for AI* — into **original, condensed references** (no copied text) organized so Claude only loads what each request needs.

| # | Reference | What Claude gets |
|---|-----------|------------------|
| 01 | Core usability | Krug's laws, scanning & satisficing, billboard design, microcopy rules |
| 02 | Navigation & IA | Trunk test, persistent nav, home page jobs, search, IA for AI-first apps |
| 03 | Psychology | Fitts, Hick, Miller, Jakob, Tesler, Doherty, Peak–End… with *how to apply* each |
| 04 | Forms & inputs | Question protocol, labels, validation & error patterns, control choice, checkout/login |
| 05 | Accessibility | The 14 rules that catch most WCAG problems, cognitive a11y, cheap testing |
| 06 | Mobile | Touch targets, thumb reach, hidden affordances, RTL & bilingual layouts |
| 07 | Ethics | Full deceptive-pattern taxonomy, persuasion-vs-manipulation test, consent flows |
| 08 | AI interfaces | Value matrix, 7 LLM interaction patterns, copilot & agentic UX, designing for uncertainty |
| 09 | Testing & process | DIY usability testing, ending design debates with evidence |
| 10 | Audit checklist | End-to-end review checklist (A–K) with severity scale |
| 11 | Live site audit | Browser-driven audit workflow: crawl, categorize, checkpoint, test forms, report |
| 12 | Internal tools | Dashboards, CRMs, admin panels, data tables, pipelines, roles |
| 13 | Remediation report | Ticket-ready findings with acceptance criteria and a phased fix plan |

## 🚀 Install

### Claude Code — as a plugin (recommended)

```bash
/plugin marketplace add shahab7e/ux-skill
/plugin install ux@ux-skill
```

### Claude Code — manual

```bash
git clone https://github.com/shahab7e/ux-skill.git
cp -r ux-skill/skills/ux ~/.claude/skills/
```

(Use `.claude/skills/` inside a project instead to share it with your team via git.)

### Claude.ai / Claude Desktop

1. Download **`ux.zip`** from the [latest release](https://github.com/shahab7e/ux-skill/releases/latest).
2. Go to **Settings → Capabilities → Skills → Upload skill** and pick the zip.

### Claude API

Upload the `skills/ux` folder via the [Skills API](https://docs.claude.com/en/api/skills-guide) and attach it to your requests.

## 💬 Usage

Type `/ux` followed by your request — or just talk normally. Claude loads the skill automatically for anything UX-related.

```text
/ux review this signup form [screenshot]
```

```text
Why are people dropping off at step 3 of our checkout? Here's the flow…
```

```text
Audit https://example.com end to end — desktop and mobile. Output a report for the dev team.
```

```text
Is this cookie banner a dark pattern?
```

```text
We're adding an AI copilot to our CRM. How should it handle wrong answers?
```

```text
Write the error messages for this payment form.
```

### Example: what changes

**Without the skill**
> Consider improving the form's usability. Make sure labels are clear and validation is user-friendly.

**With the skill**
> **Serious — the email field uses its placeholder as a label.** Once a user starts typing, the label disappears, so on review they can't confirm what each field was; screen readers may not announce it at all. **Fix:** add a visible `<label for="email">Email address</label>` above the field, move the example `name@example.com` into hint text linked with `aria-describedby`, and set `autocomplete="email"`.

## 🧠 How it behaves

- **Diagnoses before prescribing** — every finding has *where, what the user experiences, why (the principle), severity, and the fix*.
- **Always cross-checks accessibility and ethics** on forms, consent, pricing, cancellation, and urgency.
- **Adapts to product type** — public sites get conversion thinking; dashboards and internal tools get efficiency thinking.
- **Separates findings from hypotheses** and tells you what to test instead of inventing user behavior.
- **Answers in your language**, keeping standard UX terms in English. First-class support for **Persian / RTL** interfaces (Jalali dates, digit consistency, ی/ک normalization, mirrored layouts).
- **Token-efficient** — a short `SKILL.md` routes to only the 1–3 reference files a request needs.

## 📁 Structure

```
ux-skill/
├── .claude-plugin/          # plugin + marketplace manifests
└── skills/ux/
    ├── SKILL.md             # entry point & routing table
    └── references/          # 13 focused knowledge files
```

## 🤝 Contributing

Issues and PRs are welcome — new patterns, better examples, corrections, or translations of examples into more languages. Please keep additions **original, concise, and actionable**: rules and decision criteria, not quoted book text.

If this skill improved your designs, a ⭐ helps others find it.

## 📜 License

[MIT](LICENSE). The referenced books are credited as sources of ideas; this repository contains no reproduced text from them — buy them, they're excellent.
