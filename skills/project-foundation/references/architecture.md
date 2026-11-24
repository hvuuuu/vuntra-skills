# Architecture principles (language-agnostic)

Use these to generate and judge structure options. Each principle includes how to enforce it by machine, because enforced principles survive agents and unenforced ones don't.

## 1. Organize by feature once past tiny

Group code by what it does for the user (`billing/`, `auth/`, `profile/`), not by file type (`components/`, `utils/`, `services/`). Type-based folders scatter one change across many folders and grow junk drawers like `utils/`.

Tiny projects (a script, a one-page app) can stay flat. Say where the threshold is for this project.

Enforce: lint or import rules that stop features reaching into each other's internals; only a feature's public entry may be imported.

## 2. Dependencies point one way

Core/domain logic never imports UI, framework, database or network code. Outer layers depend on inner ones, never the reverse. This keeps logic testable and swappable.

Enforce: boundary rules (see the slots table), separate packages/modules where the language supports it.

## 3. Make invalid states unrepresentable

Model data so wrong combinations can't be built: tagged/discriminated unions instead of several booleans, constructor functions that validate, newtypes for IDs, required fields actually required.

Enforce: the type checker in strict mode; ban escape hatches (`any`, unchecked casts, `# type: ignore`) or require a justification the linter can find.

## 4. One way to do each thing

One state approach, one data-fetching wrapper, one error-handling pattern, one date library. Agents pick randomly between existing patterns, so two patterns become four.

Enforce: banned imports for the rejected alternatives.

## 5. Boundaries at the edges

Validate and parse external data (HTTP, files, env, user input) once, at the edge, into trusted types. Inside the boundary, code can trust its inputs.

Enforce: a schema/validation library at the edge; lint against reading raw env or raw request bodies outside the edge module.

## 6. Small, explicit public surfaces

Each module exposes a small, deliberate API. Internals stay private. A small surface is easier for both humans and agents to use correctly.

Enforce: language visibility rules, index/entry files plus lint against deep imports.

## Presenting options

When proposing two structures, show for each: the folder tree (top two levels), where the golden example would live, which principles it leans on, what it costs, and the signal that would tell us to switch to the other one later.
