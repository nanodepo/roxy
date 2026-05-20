---
name: task
description: >
  Splits feature plan into self-contained NN-task.md files for sequential impl
  and marks feature split in ./workflow/PLAN.md. Triggers: split plan into
  tasks, break down the plan, create task files, decompose feature into tasks.
---

# Task

## Purpose

Turn `./workflow/features/{slug}/plan.md` into self-contained
`./workflow/features/{slug}/NN-task.md` files, each executable alone; set
feature status `[+]` in `./workflow/PLAN.md` when task files are created.

Use after `planning` or `improve`. Read feature brief, plan, design if present.
Cut work into tasks sized for sequential `implement`. Rewrite `plan.md` into a
compact shared context + phase/task map for downstream work.

Task files feed `implement`; `test` / `docs` may read them for real scope. Own
only `NN-task.md` and task-map normalization of `plan.md`. Do not write code,
`tests.md`, or other `./workflow/` docs beyond feature status.
`./workflow/PLAN.md` status means feature ready for step-by-step impl.

## Params

Use `args`:

```txt
<feature-slug>
```

- `feature-slug`: dir under `./workflow/features/`.
- No `feature-slug`: infer from user msg + active `./workflow/PLAN.md` entries
  only when exactly one feature has `plan.md` and matches request.
- Still unknown: ask one short slug question, stop.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout, worktree.
  Dirty tree expected; user owns git.
- Own only `./workflow/features/{slug}/NN-task.md`, task map in
  `./workflow/features/{slug}/plan.md`, matching service status in
  `./workflow/PLAN.md`.
- Do not write code, `tests.md`, test code, `design.md`, user docs, or dev docs.
  Those belong to `implement`, `test`, `design`, `docs`.
- Move task-specific substance from `plan.md` into matching `NN-task.md` files.
  Keep only shared context, cross-task constraints, phase order, task links,
  task checkboxes, and active tails in `plan.md`.
- Do not duplicate `feature.md` in `plan.md`; keep product intent and boundaries
  in the brief.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4 before
  final questions or task files. Decomposition = analytical work.
- Ask only blocking questions. Blocking = cannot slice safely without answer.
  Else proceed and record gap as tail.
- Write current task set only. No chat biography, previous plan states,
  "now/previously" comparisons, or migration notes.

## Language Notice

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
- `./workflow/features/{slug}/plan.md` missing;
- several active features match.

Do not create missing plan. If feature has only `feature.md` and no `plan.md`,
report `planning` must run first, stop.

If `NN-task.md` files already exist, treat as update: rebuild task set from the
current task map, existing task files, feature brief, and design constraints;
do not append stale tasks.

### 2. Read feature + plan ctx

Read in order:

- `./workflow/features/{slug}/plan.md`: scope authority.
- existing `./workflow/features/{slug}/NN-task.md` files, if present: current
  task-specific scope for update runs.
- `./workflow/features/{slug}/feature.md`: feature intent plan serves.
- `./workflow/features/{slug}/design.md`, if exists: UI slicing constraints.
- `./workflow/PROJECT.md` and `./workflow/ARCHITECTURE.md`, when present: stack
  and code placement boundaries only.

Use `plan.md` as implementation-scope authority and `feature.md` as feature
intent authority. Do not add implementation work absent from plan.

### 3. Survey whole plan before slicing

Walk entire plan once. Cover all work categories together: foundation,
data/model, API/contracts, UI/interaction, integrations, rollout/runtime.
List every implied work unit before cutting tasks so no scope falls out.

If change is obviously small + single-file, do not create `NN-task.md`. Leave
`plan.md` as working scope, final report says feature goes straight to
`implement` from `plan.md` or `feature.md`, and leave feature status planned
unless the local workflow has a stronger marker for simple direct impl.

### 4. Decompose with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking`. Reason through:

- full work-unit list, grouped into ordered tasks;
- task size: one focused impl+verify session; split task spanning many files /
  subsystems or whose name contains "and";
- exec order + deps on earlier tasks;
- file boundaries: files touched vs left alone, so tasks do not collide;
- per task: input docs, goal, scope, verifiable expected result;
- which details belong in each `NN-task.md`;
- which shared context belongs in the compact `plan.md` task map;
- blocking gaps vs tails.

Self-contained task = executor can act without chat history and without reading
sibling `NN-task.md` files except named deps. Each task carries ctx + short
reason. If not self-contained, split further or name missing ctx in task file.
The compact `plan.md` map must not be required to understand task-specific
implementation steps.

### 5. Ask blocking questions only

Close gaps from `plan.md`, `feature.md`, workflow files, codebase before asking
user.

Ask at most three concise questions in one block. Put recommended answer first
with `(Recommended)` + short reason; offer only materially different options.

Non-blocking gap -> record as tail, do not interrupt user.

### 6. Write `NN-task.md` files

Write one `./workflow/features/{slug}/NN-task.md` per task. Number with
zero-padded `NN` (`01`, `02`, ...) in exec order. Follow Artifact Requirements.
Describe current task only, no decomposition history.

### 7. Normalize `plan.md` as map

Rewrite `./workflow/features/{slug}/plan.md` as compact shared context +
phase/task map.

Keep:

- feature-level implementation goal, stated briefly;
- links to `feature.md`, `design.md`, `PROJECT.md`, `ARCHITECTURE.md`, and
  other shared inputs when relevant;
- shared files, constraints, contracts, assumptions, and ordering rules needed
  by most tasks;
- phases with task checklist rows in execution order;
- active tails that apply beyond one task.

Remove from `plan.md` after placing in the matching task file:

- detailed task steps;
- task-specific file lists;
- task-specific risks and expected results;
- backend/frontend/API details that only one task needs;
- repeated feature brief content.

Task rows use this shape unless local format is stronger:

```md
- [ ] <task result> | `./workflow/features/{slug}/NN-task.md`
```

### 8. Update `./workflow/PLAN.md`

Set feature status `[+]` only when `NN-task.md` files were created.

If no task files were created because the change is small enough for direct
`implement`, keep the feature planned (`[-]`) and report that no split was
needed.

Preserve structure, other feature entries, status markers. Change only this
feature line. Use existing local format.

### 9. Final verification

Reread written `NN-task.md` files, `plan.md` map, changed `./workflow/PLAN.md`
entry.

Confirm:

- every surveyed work unit landed in a task;
- task-specific detail lives in the relevant `NN-task.md`, not in `plan.md`;
- `plan.md` contains only shared context, phase/task map, and active tails;
- each task fits one focused impl+verify session;
- each task has input, goal, file boundaries, verifiable expected result;
- each task can pass to `implement` with no chat retelling;
- deps + numbering form workable exec order;
- no code, `tests.md`, `design.md`, or docs created;
- task set states current desired work, no biography/delta wording.

## Artifact Requirements

Create each `./workflow/features/{slug}/NN-task.md` as self-contained artifact
per Language Notice.

Use semantic structure scaled to task size. Translate visible heading, field
label, table header, placeholder, example:

```md
# NN. <task title>

## Status

- [ ] Done

## Why

One or two lines: why this task exists and what part of the feature it moves.

## Input

- Source plan items and documents this task derives from.
- Shared context from `plan.md` that applies to this task.

## Goal

The behavior or result the task must produce.

## File boundaries

- Likely files to touch
- Files or areas to leave alone

## Scope of work

- Concrete, ordered, verb-first units of work.

## Dependencies

- None / task NN / external answer.

## Expected result

A clear, verifiable done-signal: what must be true for the task to count as
complete. This is a definition of done, not a list of test cases -- the test
plan belongs to `test`.
```

Task set ready when:

- any single task works without chat history;
- each task self-contained with own ctx + reason;
- file boundaries prevent collisions;
- expected result verifiable;
- `plan.md` is a compact task map and points cleanly at task files;
- no task-specific implementation detail remains duplicated in `plan.md`.

If plan too thin to slice safely, keep boundary: do not fabricate tasks. Report
missing input and which earlier workflow stage must run first.

## Updating PLAN.md

At end, set matching feature in `./workflow/PLAN.md` to `[+]` only when task
files exist for the feature.

Status markers:

- `[ ]` new;
- `[-]` planned;
- `[+]` split into tasks;
- `[x]` implemented;
- `[*]` tested;
- `[/]` archived.

Change only this feature line when status changes. Do not touch other
statuses/entries.

## Notes

- Missing `design.md`, `PROJECT.md`, or `ARCHITECTURE.md` does not stop skill.
  Use available ctx; avoid invented constraints.
- If `plan.md` contradicts codebase, slice around verified truth and record
  conflict as tail instead of guessing.
- If user asks plan/improve/design/implement/test/docs, report matching skill
  instead of doing that work here.
- Keep output operational: current task set, not story of creation.
