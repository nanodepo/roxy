---
name: architecture
description: >
  Selects and records the project's architectural canon — the pattern, module
  boundaries, and code placement rules — in ./workflow/ARCHITECTURE.md. Use when
  the user says "зафиксируй архитектуру", "выбери архитектурный паттерн",
  "опиши архитектуру проекта", or asks where code should live.
---

# Architecture

## Purpose

This skill picks the project's architectural pattern and records it as the
current canon in `./workflow/ARCHITECTURE.md`. It owns one concern: the
architectural pattern, module boundaries, and the rules for where code goes.

It runs after `initialize` and ideally after `roadmap`. Its canon is read by
later stages — planning, plan improvement, task breakdown, implementation,
testing, and documentation — so a feature-implementing agent can tell where to
put new code, which boundaries not to cross, and which architectural style is
current.

The skill does not write product code, does not perform refactoring, does not
create feature plans, and does not write tests. Architecture is a decision
recorded as canon; executing that decision belongs to the feature stages.

## Strict Rules

- **No git operations.** Do not check status, diff, branch, commit, or push.
  The working tree is expected to be dirty; the user manages git.
- **`mcp__sequential-thinking__sequentialthinking` is required** at the
  reasoning step (Step 3). Selecting a pattern and defining boundaries is
  analytical work and must not be improvised.
- **Canon, not history.** `ARCHITECTURE.md` describes the architecture that is
  current — in the present tense. Never write decision logs, status lifecycles
  (proposed / accepted / deprecated), or "previously X, now Y" comparisons. A
  superseded rule is simply absent.
- **Stay inside the boundary.** Do not edit product code, do not refactor, do
  not write feature plans or tests. Follow-up work goes into `./workflow/PLAN.md`
  as architectural tails only.
- Write `ARCHITECTURE.md` and this skill's text in English-style precision but
  in the user's working language for prose; keep pattern names, tool names,
  paths, and code identifiers in their original spelling.
- Use one term for one concept throughout the artifact.

## Steps

For this multi-step procedure, use the agent's task planning mode (todo list /
task plan, whichever is available) and close items one by one.

### 1. Gather context

Read whichever of these exist:

- `./workflow/PROJECT.md` — tech stack, run, deploy.
- `./workflow/VISION.md` — ideological vision.
- `./workflow/ROADMAP.md` — development goals.
- `./workflow/ARCHITECTURE.md` or any other existing architecture document.

Then scan the source tree: top-level directory names and nesting, where
business logic lives, and how modules reference each other.

If an input file is missing, continue with the remaining input — note in the
reasoning step that the decision rests on a narrower base.

### 2. Detect the pattern that is already in force

Before choosing anything, determine which pattern the project de facto follows
now, using these heuristics:

- **Directory names and nesting** — `controllers/services/repositories` or
  `domain/application/infrastructure` or `features/*` signal different styles.
- **Dependency direction between layers** — does it flow inward toward the
  domain, or do layers depend on each other freely?
- **Where business logic sits** — inside controllers, inside dedicated domain
  modules, or scattered.
- **Explicit boundaries** — presence of ports, adapters, module manifests, or
  package isolation.

State plainly which pattern is acting now. This is the baseline for deciding
whether to keep it or change it.

### 3. Choose the canon with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking` and work through:

- **Compare candidates** from the catalog below against the project context —
  scale, team size, and the goals in `ROADMAP.md`. A small project does not get
  microservices because they exist; the pattern must match the context.
- **Decide keep vs. change.** If the acting pattern fits, confirm it. If it does
  not, select a better one and capture the rationale in 1–2 lines.
- **Define module boundaries** — what may depend on what, and what must not.
- **Define code placement** — where new code of each kind goes.
- **Define cross-cutting concerns** — authentication, error handling, logging,
  validation, configuration.
- **Define naming and structure conventions.**

A good boundary keeps related logic together and exposes a small surface;
prefer that as the test of a boundary.

**Pattern catalog** (pick by fit; do not include code samples):

- **Layered** — clear horizontal layers; small-to-medium apps with simple flows.
- **Modular monolith** — one deployable, strong internal module boundaries;
  most growing products.
- **Clean / Hexagonal** — domain isolated behind ports and adapters; logic-heavy
  systems that must stay testable and framework-agnostic.
- **DDD (strategic)** — bounded contexts and a shared language; large domains
  with real business complexity.
- **Microservices** — independently deployable services; large teams and
  independent scaling needs only.
- **Event-driven** — components communicate via events; asynchronous,
  high-throughput, or loosely coupled flows.
- **MVVM / MVU** — UI state patterns; client and front-end applications.
- **Serverless** — functions as the unit of deployment; event-triggered,
  spiky, or low-ops workloads.

If a change of pattern would force refactoring of existing code, confirm the
change with the user before writing it as canon, since it carries follow-up
cost.

### 4. Write `./workflow/ARCHITECTURE.md`

Create or update `./workflow/ARCHITECTURE.md` per the section checklist in
*Artifact Requirements*. Overwrite outdated content; do not append it as a
revision history.

### 5. Record architectural tails in `./workflow/PLAN.md`

If the chosen canon requires follow-up work (e.g. a module must be moved to fit
the new boundaries), append those items to `./workflow/PLAN.md` as architectural
tails. Do not create feature plans and do not change feature statuses — this
skill only adds architectural follow-up notes.

## Artifact Requirements

`./workflow/ARCHITECTURE.md` must answer every question a feature agent needs.
Include these sections:

- **Pattern** — the chosen pattern and a 1–2 line rationale tied to project
  scale, team, and roadmap goals.
- **Module boundaries and dependency rules** — what may depend on what; what is
  forbidden.
- **Code placement** — where new code of each kind goes.
- **Cross-cutting concerns** — authentication, error handling, logging,
  validation, configuration.
- **Naming and structure conventions.**

Diagrams (C4 / UML / Mermaid) are optional — add one only if it clarifies the
canon; never make it a required step.

Keep the document tight:

- Write only the current state, in the present tense.
- No biography of how the architecture evolved, no "previously / now", no
  decision lifecycle.
- No operational chatter — every line is a rule a feature agent can act on.

## Updating PLAN.md

At the end, touch `./workflow/PLAN.md` only to append architectural tails from
Step 5 — follow-up work the chosen canon implies. Do not add feature entries and
do not modify feature statuses; architecture is not a feature.

## Notes

- The skill works with partial input: if `PROJECT.md`, `VISION.md`, or
  `ROADMAP.md` is missing, proceed on the available context and the source tree.
- If `ARCHITECTURE.md` already exists, treat it as input, then rewrite it to the
  current canon — do not keep stale rules side by side with new ones.
