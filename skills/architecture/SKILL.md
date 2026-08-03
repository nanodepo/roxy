---
name: architecture
description: >
  Selects and records the project's architectural canon — the pattern, module
  boundaries, and code placement rules — in ./workflow/ARCHITECTURE.md. Use when
  the user says "record architecture", "choose an architecture pattern",
  "describe the project architecture", or asks where code should live.
---

# Architecture — Record the Architectural Canon

## Purpose

This skill selects the project's current architecture and records it in
`./workflow/ARCHITECTURE.md`: the pattern, module boundaries, dependency
rules, and code placement rules. It runs after `initialize`, ideally after
`roadmap`. Later stages read this canon to plan, split, implement, test, and
document features without re-deriving where code belongs.

The skill records canon; it does not execute it. It writes no product code,
performs no refactoring, and creates no feature plans or tests — follow-up
work becomes architectural tails in `./workflow/PLAN.md`.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty tree is expected.
- `ARCHITECTURE.md` states the current architecture in the present tense: no
  decision logs, proposed/accepted/deprecated lifecycle, or "previously X,
  now Y". Superseded rules are replaced, not kept.
- Keep only rules that answer a future coding question: where code belongs,
  which direction dependencies flow, what public surface exists, how a
  cross-cutting concern is handled. When a historical reason protects an
  active non-obvious constraint, write the constraint, not the story.
- Use one canonical term and one canonical rule per concept; resolve
  contradictions to one current rule instead of blending old ones.
- Do not edit product code, refactor, write feature plans, or write tests.

## Language Notice

Write user-facing chat output and generated or rewritten project artifacts in
the working language of the target project. Detect it from existing
`./workflow/` files, project documentation, and the user's request. If the
project language is unclear, use the user's current language.

When editing an existing artifact, preserve its language unless the user
explicitly asks to translate it.

Apply the chosen artifact language to all prose, headings, table headers,
labels, placeholders, and examples. Keep file paths, commands, tool names, code
identifiers, framework names, package names, status markers, and established
product terms in their original spelling.

Do not mix languages inside one artifact unless the existing project canon
already does so or a quoted or source term requires it.

## Steps

Use task planning mode (todo list) for this multi-step flow.

1. **Gather context.** Read `./workflow/PROJECT.md`, `VISION.md`,
   `ROADMAP.md`, and any existing `ARCHITECTURE.md` or other architecture
   document. Scan the source tree: top-level directories, nesting, where
   business logic lives, how modules reference each other. Historical docs
   and ADR-style files are clues, not authority, unless current code or the
   user confirms their rule. Missing inputs do not stop the skill.

2. **Detect the acting pattern.** Determine the de facto architecture before
   choosing canon: directory shape, dependency direction, business-logic
   location, explicit boundaries (ports, adapters, module manifests). State
   it plainly as the baseline for the keep/change decision.

3. **Choose the canon.** Compare candidates against project scale, team size,
   and roadmap goals. If the acting pattern fits, confirm it; otherwise
   select a better fit with a one- or two-line rationale. Define module
   boundaries with allowed and forbidden dependencies, code placement for
   each kind of code, cross-cutting concerns (auth, errors, logging,
   validation, config), and naming conventions. Boundary test: related logic
   stays together, public surface stays small. Common candidates, by fit:
   layered (small apps with simple flows), modular monolith (growing
   products), clean/hexagonal (logic-heavy testable systems), strategic DDD
   (large complex domains), microservices (large teams, independent scaling),
   event-driven (async, loose coupling), MVVM/MVU (client UI state),
   serverless (event-triggered, low-ops workloads). If the chosen canon
   implies refactoring existing code, ask the user before recording it.

4. **Write `./workflow/ARCHITECTURE.md`** per Artifact Requirements. Replace
   superseded rules in place; do not document aspirational structure as
   current canon unless the user accepted it as the target.

5. **Record architectural tails.** If the canon needs follow-up work, append
   architectural tails to `./workflow/PLAN.md`. A tail names the active
   missing boundary, violation, or normalization need — not how the
   architecture changed. Do not add feature entries or change feature
   statuses.

6. **Report.** State the path written, the chosen pattern with its rationale,
   and any tails added. End with the next-step recommendation: the next
   missing canon artifact — `roadmap` when `VISION.md`/`ROADMAP.md` is
   absent, `design-guideline` when `DESIGN.md` is absent — or `feature` when
   the project canon is complete.

## Artifact Requirements

`./workflow/ARCHITECTURE.md` answers what feature agents need. Core sections:

- **Pattern** — the chosen pattern with a one- or two-line rationale tied to
  scale, team, and roadmap.
- **Module boundaries and dependency rules** — allowed and forbidden
  dependencies.
- **Code placement** — where new code of each kind goes.
- **Cross-cutting concerns** — auth, errors, logging, validation, config.

Add further sections (naming conventions, diagrams) only when they carry a
rule an agent will act on. Every line is a rule a feature agent can apply; no
evolution biography and no restatement of the obvious directory tree.
