# 08 — AI Interfaces

Designing for AI is different from designing conventional software in one specific way: **the output is probabilistic**. The system will be wrong some of the time, and you can't know in advance which times. Everything below follows from that.

## Framing the problem before designing anything

### Pick the right use case
The most common cause of failed AI features is a bad use case, not bad design or a bad model.

Red flags:
- **AI telling experts how to do their job.** Domain experts don't want instructions from a system that can't justify itself; they want their work accelerated. Reframe from "the AI decides" to "the AI does the tedious part and shows its work."
- **"Add AI" as a goal** rather than a specific job the user is struggling with.
- **A chatbot replacing an interface that already worked.** Conversation is a poor control surface for tasks that were already three clicks.

Ask a better question: what part of this workflow is slow, tedious, or requires scanning huge volumes of data — and would still be valuable if the answer were only mostly right?

### Storyboard the flow
Before any UI, sketch the sequence of what the user is doing, what the system does, what it shows, and — critically — what happens when it's wrong. Cheap sketches surface bad assumptions before they get built.

### Build a digital twin
Model the real-world entities, states, and events the system reasons about (a machine, a customer, a shipment, a case). Naming the objects and their states is what makes it possible to design meaningful states, errors, and explanations rather than generic ones.

### The value matrix (the most important tool here)
Accuracy percentages are close to meaningless on their own. A confusion matrix tells you the *rate* of false positives and false negatives; it doesn't tell you what they *cost*.

The value matrix adds the human cost/benefit of each outcome:

| | Model says yes | Model says no |
|---|---|---|
| **Actually yes** | value of true positive | cost of false negative |
| **Actually no** | cost of false positive | value of true negative |

Multiply each cell's cost/benefit by its frequency to get the real expected value of a design decision.

**This determines the interaction design directly.** If the cost of a false positive is trivial (running a query, showing a suggestion), act immediately. If it's severe (sending a message, moving money, deleting data, a medical or safety decision), require confirmation, show the reasoning, and keep a human in the loop. The same model accuracy demands completely different UX depending on the cell values.

## LLM interaction patterns

These seven patterns recur across nearly every language-model interface. Use them by name.

**Restating** — the system shows what it understood the request to be, before or alongside the answer. Removes ambiguity about what's being answered, and makes sloppy or incomplete input safe. Whether to restate *before* acting is a value-matrix question: cheap, reversible actions can execute immediately; consequential ones must confirm first ("You asked me to send X to your manager — correct?").

**Auto-complete** — suggesting the correct concepts and vocabulary *while* the user types, preventing the misunderstanding rather than correcting it. Signals what the system can actually answer, which is otherwise invisible. Distinguish suggestions that match known data from input that doesn't match anything, and say so at the point of typing rather than after submission.

**Talk-back** — richer than restating: the system explains what went wrong and why, asks clarifying questions, and proposes alternative approaches. Turns a dead end into a guided exploration. Needs room, so it usually implies a chat or panel interface.

**Initial suggestions** — what to show before the user has done anything, when they don't yet know what's possible. Generic examples work; suggestions tuned to the user's actual data, history, or last session work far better. This is the empty state of an AI product, and it's where most of them fail.

**Next steps** — suggestions after a result, inferring the likely follow-up. Best versions analyze the returned results (anomalies, gaps, trends) rather than just matching similar past queries. Retrain continuously on what users actually click.

**Regen tweaks** — controls for regenerating a variant. Belongs to a different flow than next steps:
- *Chat/exploration flow*: output is assumed mostly right, model runs "cold" for consistency, and the useful affordance is **next steps** to continue.
- *Creative generation flow*: output is assumed to be a draft, model runs "hot" for variety, and the useful affordance is **regen tweaks** — vary subtly, vary strongly, vary a region, adjust parameters.
Identify which flow you're designing before choosing controls.

**Guardrails** — constraints on what the system will produce. Design them as part of the interface, not just the model: when a request is refused or constrained, say so clearly and explain what *can* be done. Note honestly that no guardrail is absolute — sufficiently determined reframing gets around most of them — and that role-based access control over information inside a model is not currently achievable. If data must be genuinely restricted, restrict access to the model instance, not to prompts.

## Copilot design

- **Real estate scales with task importance.** A trivial assist can live in a corner; a task the user is genuinely working through needs a full panel or full screen.
- **Copilots are stateful.** They carry conversation history and context, and the interface must show what context is in play. Users lose trust fast when they can't tell what the system remembers.
- **Keep the information architecture simple** and centered on the conversation, with clear paths back to the underlying data.
- **Prompt scaffolding** (prompt libraries, templates, "promptbooks") saves users from having to become prompt engineers. If a feature only works with a well-crafted prompt, supply the prompts.
- **Grounding in the user's actual system** — plugins, connectors, indexed data — is what separates a useful copilot from a generic chatbot.

## Designing for uncertainty

- **Show confidence honestly**, but don't display raw probabilities to users who can't act on them. Translate into what to do: "verify this before sending," "3 of 40 results look unusual."
- **Show sources and reasoning** where the user must be able to check the work. Traceability is often more valuable than accuracy.
- **Make correction cheap.** The user should be able to fix a wrong output in one step, and the fix should teach the system.
- **Never present a guess with the visual language of a fact.** Style uncertain output differently from retrieved fact.
- **Preserve the manual path.** Users must be able to do it themselves when the AI is wrong or unavailable.
- **Design the failure states first.** Wrong answer, no answer, partial answer, timed-out answer, refused answer — these are the majority of the design work, not edge cases.

## AI in search and dashboards

- LLM-based search can answer questions that keyword search cannot, but it must not hide the underlying results — people need to verify, and to explore beyond the answer.
- **Degree-of-interest (DOI) sorting**: rank what's shown by inferred relevance to the current user and moment, rather than a fixed order. Powerful for dense dashboards; requires showing *why* something was surfaced, otherwise it reads as arbitrary.
- **Watch for bias in recommendations.** A ranking model trained on past behavior reproduces past bias and hides alternatives. Give users a way to see the unfiltered set.

## Agentic AI

Systems that plan and take multi-step actions raise the stakes on everything above:

- **Show the plan before execution** for anything consequential, and make it editable.
- **Show progress step by step**, not a spinner — the user needs to see where it went wrong when it does.
- **Make interruption and rollback first-class.** Stop, undo, and "don't do that part" must always be available.
- **Draw a clear line between reversible and irreversible actions.** Irreversible actions (sending, purchasing, deleting, publishing) need explicit human confirmation regardless of confidence.
- **Attribute clearly** what the agent did versus what the user did, in logs and in the interface.

## Process implications

AI features can't be specified fully in advance, because feasibility depends on data that hasn't been examined yet. Practical consequences:

- **Design and data science must work together from the start.** Designers need to ask what the data can actually support; data scientists need to know what decision the output feeds.
- **Use spikes** — short, time-boxed technical investigations — to answer feasibility questions before committing to a design.
- **Prototype with real data**, or the design will be built around outputs the system can't produce.
- **Test with wrong outputs on purpose.** A prototype that only ever shows the correct answer tests nothing that matters.
