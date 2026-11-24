---
name: project-foundation
description: Set up or audit the foundation of a software project in any language — codebase structure (folders, architecture, boundaries, types) and static analysis (formatter, linter, type checker, tests, CI, agent hooks) — while teaching the user the judgment behind each choice. Use this whenever the user is starting a new project or repo, picking a stack or libraries, asking how to organize folders or files, configuring lint/types/formatting, adopting an unfamiliar language, or says their codebase feels messy and AI keeps copying bad patterns — even if they never say "foundation" or "setup".
license: MIT
---

# Project Foundation

AI agents copy whatever the codebase already does. A weak foundation gets multiplied; a strong one makes the right thing the default. This skill builds the two cheapest and most durable layers of trust in agent-written code:

1. **Codebase** — architecture, API design, types and boundaries that make mistakes hard or impossible to write.
2. **Static analysis** — formatter, linter, type checker, tests, CI and agent hooks that catch by machine what the code still allows.

Prose rules (CLAUDE.md, AGENTS.md, comments) and human review sit *below* these layers: they are skipped, forgotten, or expensive. So whenever a decision here can be enforced by code or tooling, enforce it there instead of writing it down.

The user is often still building judgment. The goal is not only a working setup but a user who can explain *why* every piece exists. A setup the user can't defend is a setup they can't maintain.

## Modes

- **New project** — run steps 0 → 5 in order.
- **Existing project audit** — run step 0 (with early findings), then audit against steps 1–4 (what's missing, what's inconsistent, what's slop), propose fixes ranked by leverage, then step 5.
- **Single question** ("which linter for Go?", "how should I split these folders?") — answer from the relevant step only, still applying the research and learning rules below. Don't force the whole process on a small question.

## Working rules

- **Research, don't recall.** Tooling changes fast (config formats, deprecations, new defaults). Before recommending a tool, version, or config shape, check current official docs or release notes with whatever search/fetch tools are available. Say when you couldn't verify something.
- **Stop at decisions.** Propose, explain, and let the user choose at each checkpoint marked ⛳. Don't silently install packages, scaffold structure, or pick libraries on their behalf.
- **Context beats "best practice".** There is no best practice without a context. An enterprise layout in a solo weekend project is slop. Tie every recommendation back to the brief from step 0.
- **Less is more.** Prefer fewer dependencies, fewer layers, one way to do each thing. Every addition must earn its place.
- **Match the user's language.** Reply in the language the user writes in. If the skill is invoked with no message text, use the language of the conversation so far, or English if there is none. Tool names and config keys stay as they are.

## Step 0 — Brief, then restate

Get (or ask for) a short brief: what the project is, scale and team size, expected lifespan, where it runs/deploys, hard constraints (language mandated, performance, compliance, budget), and the user's experience with this stack.

Then restate it **in your own words** before doing anything else:

- **Goal** — what you understand they want
- **Constraints**
- **Done means (proposed)** — what the finished foundation would include. This is a draft scope, not a decision: tag any item that depends on an open question with that question (e.g. "CI pipeline — depends on Q2"), and leave out items the user hasn't had a chance to choose yet. Step 0 frames the decisions; it doesn't make them.
- **Out of scope**
- **Assumptions I'm filling in** — the most important part; surface what was not said
- **Early findings** (audit mode only) — the few highest-leverage gaps, plus anything already done well that's worth building on. Mark them as not acted on yet.
- **Open questions** — numbered, so "Done means" items can point at them

⛳ Wait for the user to confirm or correct. Parroting the request back is useless; the value is in exposing hidden assumptions.

## Step 1 — Fill the slots

Every language ecosystem has the same slots, even though the tools differ. Read `references/slots.md` for the slot list, per-language starting points, and the library evaluation checklist.

For each slot, give:

- the current mainstream choice (verified, see Working rules)
- one credible alternative
- the trade-off between them
- **when this choice would be wrong** — this question reveals real understanding faster than "is this good?"

For libraries beyond the toolchain, compare 2–3 candidates on the checklist in `references/slots.md`, including the often-forgotten **cost of removing it later**.

⛳ User picks per slot. Before showing your recommendation on a slot, offer the user the chance to guess first (see Learning loop).

## Step 2 — Architecture and folder structure

Read `references/architecture.md` for the principles. In short: organize by feature/domain once the project is beyond tiny; dependencies point one way (core never imports UI or infrastructure); use types to make invalid states unrepresentable; one way to do each thing.

- Propose **two** structures that fit the brief, with trade-offs.
- Find 1–2 well-regarded open-source repos in the same stack and compare against how they actually do it. Real code outranks your opinion.
- ⛳ User chooses. Record the decision as a short ADR (3–5 lines: context, decision, why, what would make us revisit) in `docs/decisions/`. ADRs are for humans revisiting choices, not instructions for agents.

## Step 3 — Golden example

Build **one** feature end-to-end the way every future feature should look: types, error handling, tests, naming, file placement. Keep it small but complete.

This file set becomes the pattern agents copy, so review it harder than anything else in the project. Walk the user through each part and why it's shaped that way.

⛳ User reviews and approves the golden example before moving on.

## Step 4 — Lock it into static analysis

Turn the decisions from steps 1–3 into machine checks:

- Start **strict** on new projects (strict types, warnings as errors in CI). Tightening later is much harder than loosening.
- Encode architecture as rules: forbidden imports across boundaries, banned APIs/patterns that were rejected in step 2.
- Wire the checks into three places: pre-commit hook, CI, and the coding agent's hooks (e.g. run the linter after every file edit and feed failures back to the agent; block creating stray files like unrequested markdown). Agent hooks run every time, unlike prose rules the agent may skip.
- **Every rule must have a reason the user can state.** If they can't, explain until they can, or drop the rule.
- Keep the agent rules file (CLAUDE.md / AGENTS.md) short: commands to run, folder map, pointer to the golden example, and only the guidance that genuinely needs judgment.

Verify the setup actually works: run each tool once, introduce a deliberate violation, confirm it gets caught, then revert it. Report what you ran and what you could not verify.

## Step 5 — Feedback loop

Hand the user these habits:

- **Second time, it's a rule.** When an agent repeats a mistake, ask "can this be a lint rule, a type, or an API change?" before adding a line of prose.
- **Garden the repo.** Workarounds and inconsistent code get copied. Removing bad precedent is worth more than fixing one bad change.
- **Independent audits.** Periodically review the codebase in a fresh session (or with a different model): where does code drift from the golden example, where are there two ways of doing the same thing. The session that wrote the code is bad at seeing its own drift.

## Learning loop

Weave these through the steps rather than lecturing at the end:

- **Predict first.** At ⛳ checkpoints, invite the user to say what they'd pick before you reveal yours. Where they differ is where they learn most.
- **Teach-back.** After a major decision, ask the user to explain it back in their own words; correct misunderstandings directly.
- **Triangulate.** Point them to the official doc page and a reference repo for each major choice, so they're not trusting you alone. AI is confidently wrong sometimes.

## Slop red flags

Call these out when you see them, in the user's project or in your own proposals:

- Many dependencies for a small project
- Abstraction layers with a single implementation and no clear reason
- Deprecated config formats or tools (check current docs)
- Enterprise structure copied into a small app
- Comments explaining *what* code does, or justifying a workaround (this makes the workaround look like accepted practice)
- Multiple patterns for the same job (two state libs, two fetch wrappers, two date libs)
- Rules nobody can explain

## Output

At the end, give a short foundation summary: chosen tool per slot, chosen structure (with ADR links), where the golden example lives, which checks run where, what was verified, and what's still open.