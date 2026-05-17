---
name: planning
description: >
  Turns feature brief into ./workflow/features/{slug}/plan.md and marks feature
  planned in ./workflow/PLAN.md. Triggers: plan feature, create plan, write
  plan.md, turn feature into a plan.
---

# Planning

## Purpose

Turn `./workflow/features/{slug}/feature.md` into impl plan at
`./workflow/features/{slug}/plan.md`; set feature status `[-]` in
`./workflow/PLAN.md`.

Use after `feature`. Answers "how to build": read brief + project ctx, inspect
codebase enough for realistic plan, build dep graph, split work into phases
with concrete checkboxes, likely files, deps, risks, exit criteria.

Plan feeds `design`, `improve`, `task`, `implement`, `test`, `docs`. It does
not own test plan, UI design, task files, or code.

## Params

Use `args`:

```txt
<feature-slug>
```

- `feature-slug`: dir under `./workflow/features/`.
- No `feature-slug`: infer from user msg + active `./workflow/PLAN.md` entries
  only when exactly one feature has `feature.md` and matches request.
- Still unknown: ask one short slug question, stop.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout, worktree.
- Own only `./workflow/features/{slug}/plan.md` + matching service status in
  `./workflow/PLAN.md`.
- Do not write `tests.md`, `design.md`, `NN-task.md`, code, user docs, dev
  docs, PRD, or SPEC.
- Product code read-only. Inspect to plan; do not modify.
- Do not auto-call downstream skills. Stop after `plan.md` + `PLAN.md` status.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4 before
  final questions or writing plan. Planning = analytical work.
- Ask only blocking questions. Blocking = no realistic plan without answer.
  Record non-blocking gaps as `Open questions` in `plan.md`.
- Write current plan only. No chat biography, prior plan states, "now/previously"
  comparisons, migration notes, or removed behavior.
- Keep test ideas out of `plan.md`. Checkpoints verify impl progress; they do
  not replace `tests.md`. Phrase verification as live behavior / product
  invariant, not incident or deletion check.

## Language Notice

Write this `SKILL.md` in English.

Write chat output and generated/rewritten project artifacts in target project
working language. Detect from `./workflow/`, docs, user request. If unclear,
use user language.

When editing existing artifact, preserve language unless user asks translate.

Apply artifact language to prose, headings, table headers, labels,
placeholders, examples. Keep paths, commands, tools, code ids, frameworks,
packages, status markers, product terms as-is.

Do not mix languages in one artifact unless project canon already does or quote
/ source term requires it.

## Steps

Use task planning mode for this multi-step flow. Close items one by one.

### 1. Identify feature

Resolve `{slug}` from `args`, user msg, or `./workflow/PLAN.md`.

Stop and ask slug if:

- `./workflow/features/{slug}/` missing;
- `./workflow/features/{slug}/feature.md` missing;
- several active features match.

Do not create missing feature brief.

If `./workflow/features/{slug}/plan.md` exists, treat run as update: rebuild
from current inputs, not append old text.

### 2. Read feature + project ctx

Read in order:

- `./workflow/features/{slug}/feature.md`: scope authority.
- `./workflow/features/{slug}/design.md`, if exists: UI constraints.
- `./workflow/PROJECT.md`: stack, run, deploy, project shape.
- `./workflow/ARCHITECTURE.md`: architecture + code placement rules.
- `./workflow/DESIGN.md`: only when feature touches UI / interaction.
- `./workflow/PLAN.md`: feature list + workflow status.

Use feature brief as scope authority. Use architecture, design, project files as
constraints, not permission for unrelated work.

### 3. Recon codebase

Inspect as deeply as realistic plan needs, focused on plan evidence. Use `rg`,
`rg --files`, direct reads.

Find + verify:

- similar modules, routes, components, services, schemas, migrations, configs,
  conventions;
- existing vs changed vs created pieces;
- public API, CLI, DB, UI contracts shared across system;
- architecture/stack boundaries plan must not cross;
- deploy/runtime constraints.

Record verified vs assumed so plan names no phantom paths/modules/deps.

### 4. Analyze with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking`. Reason through:

- required behavior + technical approach fitting architecture/stack;
- vague reqs -> verifiable success criteria;
- `In` scope + explicit `Out` scope to stop creep;
- dep graph: foundation, data/model, API/contracts, UI/interaction,
  integrations, rollout/runtime;
- phase decomposition into working verifiable behavior; prefer vertical slices
  over isolated horizontal infra;
- per phase: goal, ordered checkboxes, likely files, deps, risks + mitigations,
  exit criteria;
- strict sequence vs parallelizable work;
- safe assumptions vs gaps blocking realistic plan.

Filters:

- Split broad phase/checkbox if it spans many files/subsystems or name contains
  "and". Checkbox = concrete, ordered, verb-first. Avoid vague "handle backend"
  / "do auth".
- Keep each phase context-safe: limited scope, named deps, concrete files, clear
  exit criterion.
- If phase cannot be context-safe, say so in plan and propose smaller split.
- If planning decision changes feature meaning, record in `plan.md` decisions
  or raise as open question.

### 5. Ask blocking questions only

Close gaps from `feature.md`, workflow files, codebase before asking user.

Ask at most three concise questions in one block, grouped by category: scope,
user interaction, data, integrations, constraints. For each question, put
recommended answer first with `(Recommended)` + short reason; offer only
materially different options.

If gap does not block realistic plan, write under `Open questions` in `plan.md`
instead of interrupting user.

### 6. Write `plan.md`

Write `./workflow/features/{slug}/plan.md` per Artifact Requirements. Describe
current plan state only, with no production history.

### 7. Update `./workflow/PLAN.md`

Set feature status `[-]`.

Preserve structure, other feature entries, status markers. Change only this
feature line. Use local format; if no stronger pattern, point entry at
`./workflow/features/{slug}/plan.md`.

### 8. Final verification

Reread `plan.md` + changed `./workflow/PLAN.md` entry.

Confirm:

- plan can pass to `design`, `improve`, or `task` with no chat retelling;
- no phantom path/module/dep remains as instruction;
- `In` and `Out` explicit;
- every phase has goal, ordered checkboxes, likely files, deps, exit criterion;
- no `tests.md`, `design.md`, `NN-task.md`, code, or docs created;
- plan states current desired state, no biography/delta wording.

## Artifact Requirements

Create `./workflow/features/{slug}/plan.md` as self-contained artifact following
Language Notice.

Use semantic structure below, scaled to feature complexity. Translate visible
heading, field label, table header, placeholder, example before writing
artifact. Omit optional sections that add no info for simple feature:

```md
# plan.md

## Goal

What behavior the feature must deliver and the chosen approach.

## Scope

### In

- What the implementation covers

### Out

- What the implementation does not cover

## Context

- Input documents
- Relevant modules
- Local patterns to follow
- Architecture, design, or stack constraints

## Decisions

- Technical decisions and their reasons
- Contracts shared between parts of the system
- Assumptions safe enough to proceed on

## Dependency graph

- Foundation
- Data/model
- API/contracts
- UI/interaction
- Integrations
- Rollout/runtime constraints

## Phases

### Phase 1: <name>

Goal: ...

- [ ] Concrete, ordered, verb-first action
- [ ] Concrete, ordered, verb-first action

Likely files:

- `path/to/file`

Dependencies:

- None / Phase N / external answer

Risks:

- Risk -> practical mitigation

Exit criteria:

- What must be true after the phase

### Phase 2: <name>

...

## Checkpoints

- [ ] After foundation: ...
- [ ] After core flow: ...
- [ ] Before handing off to `task`: ...

## Open questions

- Only questions that affect implementation

## Tails

- What to pass on to `design`, `task`, `test`, `docs`, or user
```

Plan ready when:

- another agent can act without chat history;
- `In` / `Out` stop scope spread;
- each phase has goal, ordered actions, likely files, deps, exit criteria;
- risks include practical mitigation, not generic warning;
- oversized phases cut to manageable size;
- checkpoints verify impl progress, not replace test plan;
- open questions are short and affect impl;
- plan contains no test canon, UI design, code, or docs.

If feature too thin to plan safely, keep boundary: write minimal honest
`plan.md` naming missing inputs + next required workflow stage. Do not
fabricate phases.

## Updating PLAN.md

At end, set matching feature in `./workflow/PLAN.md` to `[-]`.

Status markers:

- `[ ]` new;
- `[-]` planned;
- `[+]` split into tasks;
- `[x]` implemented;
- `[*]` tested;
- `[/]` archived.

Change only this feature line. Do not touch other statuses/entries.

## Notes

- Missing `PROJECT.md`, `ARCHITECTURE.md`, or `DESIGN.md` does not stop skill.
  Use available ctx; avoid invented constraints.
- If codebase contradicts `feature.md`, trust verified codebase for existence,
  plan around it, record conflict as open question or tail.
- If user asks design/improve/task split/implement/test/docs, report matching
  downstream skill instead of doing that work here.
- Keep output operational: current plan, not story of creation.
