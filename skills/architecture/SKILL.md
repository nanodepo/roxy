---
name: architecture
description: >
  Selects and records the project's architectural canon — the pattern, module
  boundaries, and code placement rules — in ./workflow/ARCHITECTURE.md. Use when
  the user says "record architecture", "choose an architecture pattern",
  "describe the project architecture", or asks where code should live.
---

# Architecture

## Purpose

Select current architecture canon; write `./workflow/ARCHITECTURE.md`.

Scope: pattern, module boundaries, code placement rules. Runs after `initialize`, ideally after `roadmap`. Later stages read this canon for feature planning, improvement, task split, implementation, testing, docs.

Do not write product code, refactor, create feature plans, or write tests. Architecture = recorded canon; feature stages execute it.

## Strict Rules

- **No git operations.** Do not check status, diff, branch, commit, push. Dirty tree expected; user owns git.
- **Use task planning mode.** Create todo/task plan for this multi-step work; close items one by one.
- **`mcp__sequential-thinking__sequentialthinking` required** in Step 3. Pattern + boundaries are analytical work.
- **Canon, not history.** `ARCHITECTURE.md` states current architecture in present tense. No decision logs, proposed/accepted/deprecated lifecycle, or "previously X, now Y". Superseded rules are absent.
- **Stay in boundary.** Do not edit product code, refactor, write feature plans, or write tests. Put follow-up only as architectural tails in `./workflow/PLAN.md`.
- Use one term per concept.

## Architecture Canon Discipline

Treat `./workflow/ARCHITECTURE.md` as the current placement and dependency
contract for future feature work. It is not an ADR or decision diary.

- Keep only rules that answer a future coding question: where code belongs,
  which direction dependencies flow, what public surface exists, or how a
  cross-cutting concern is handled.
- State constraints as present-tense rules. Omit selection history, rejected
  patterns, migration narrative, and obsolete alternatives.
- Existing source code shows the de facto pattern. Existing architecture docs,
  ADRs, changelogs, and comments are evidence only when current code or user
  direction confirms them.
- If historical reason protects an active non-obvious constraint, write the
  constraint, not the story.
- Resolve contradictions to one current rule. Do not blend multiple old rules
  into a compromise.

## Language Notice

Write chat output and generated/rewritten project artifacts in target project working language. Detect from `./workflow/`, docs, and user request. If unclear, use user's current language.

When editing existing artifact, preserve language unless user asks translation.

Apply artifact language to prose, headings, table headers, labels, placeholders, examples. Keep paths, commands, tool names, code identifiers, framework/package names, status markers, and established product terms unchanged.

Do not mix languages inside one artifact unless existing canon or source term requires it.

## Steps

### 1. Gather Context

Read existing inputs:

- `./workflow/PROJECT.md` — stack, run, deploy.
- `./workflow/VISION.md` — product ideology.
- `./workflow/ROADMAP.md` — development goals.
- `./workflow/ARCHITECTURE.md` or other architecture doc.

Scan source tree: top-level dirs, nesting, business-logic location, module refs.
Use historical docs or ADR-style files as clues, not authority, unless their
rule is active in current code or confirmed by the user.

If input missing, continue. Note narrower basis in reasoning.

### 2. Detect Acting Pattern

Determine de facto current pattern before choosing canon. Check:

- Directory names/nesting: `controllers/services/repositories`, `domain/application/infrastructure`, `features/*`.
- Dependency direction: inward to domain or free cross-layer refs.
- Business logic location: controllers, domain modules, scattered.
- Explicit boundaries: ports, adapters, module manifests, package isolation.

State acting pattern plainly. Use it as baseline for keep/change decision.

### 3. Choose Canon With Sequential Thinking

Call `mcp__sequential-thinking__sequentialthinking`. Work through:

- Compare candidates vs project context: scale, team size, `ROADMAP.md` goals.
- Decide keep/change. If acting pattern fits, confirm. If not, select better pattern and give 1-2 line rationale.
- Define module boundaries: allowed deps, forbidden deps.
- Define code placement: where each code kind belongs.
- Define cross-cutting concerns: auth, errors, logging, validation, config.
- Define naming and structure conventions.
- Omit rules that are only preference, biography, or obvious restatement of the
  directory tree.

Boundary test: related logic stays together; public surface stays small.

Pattern catalog, pick by fit, no code samples:

- **Layered** — horizontal layers; small/medium apps with simple flows.
- **Modular monolith** — one deployable, strong internal module boundaries; growing products.
- **Clean / Hexagonal** — domain behind ports/adapters; logic-heavy, testable, framework-agnostic systems.
- **DDD (strategic)** — bounded contexts + shared language; large complex domains.
- **Microservices** — independently deployable services; large teams + independent scaling only.
- **Event-driven** — event communication; async, high-throughput, loose coupling.
- **MVVM / MVU** — UI state patterns; client/front-end apps.
- **Serverless** — functions as deployment unit; event-triggered, spiky, low-ops workloads.

If new canon implies refactoring existing code, ask user before writing it as canon.

### 4. Write `./workflow/ARCHITECTURE.md`

Create/update `./workflow/ARCHITECTURE.md` using Artifact Requirements. Overwrite stale content; do not append revision history.
Replace superseded rules in place. Do not document aspirational structure as
current canon unless the user accepted it as the target architecture.

### 5. Record Architectural Tails

If canon needs follow-up work, append architectural tails to `./workflow/PLAN.md`.
Tail wording states the active missing boundary, violation, or normalization
need. It does not recap how the architecture changed.

Do not create feature plans, add feature entries, or change feature statuses. Architecture is not a feature.

## Artifact Requirements

`./workflow/ARCHITECTURE.md` must answer what feature agents need:

- **Pattern** — chosen pattern + 1-2 line rationale tied to scale, team, roadmap.
- **Module boundaries and dependency rules** — allowed deps, forbidden deps.
- **Code placement** — where new code of each kind goes.
- **Cross-cutting concerns** — auth, errors, logging, validation, config.
- **Naming and structure conventions.**

Diagrams (C4 / UML / Mermaid) optional. Add only when they clarify canon.

Keep document tight:

- Current state only, present tense.
- No evolution biography, "previously / now", or decision lifecycle.
- No operational chatter. Every line = rule feature agent can act on.
- One canonical term and one canonical rule per concept; link or reference
  rather than repeating variants.
