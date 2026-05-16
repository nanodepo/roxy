---
name: task
description: >
  Splits a feature plan into self-contained NN-task.md files for sequential
  implementation and marks the feature split in ./workflow/PLAN.md. Use for
  "split plan into tasks", "break down the plan", "create task files", or
  "decompose feature into tasks".
---

# Task

## Purpose

Turn `./workflow/features/{slug}/plan.md` into a set of self-contained task
files at `./workflow/features/{slug}/NN-task.md`, each executable on its own,
and set the feature status to `[+]` in `./workflow/PLAN.md`.

Use this skill after `planning` or `improve`. It reads the feature brief, the
plan, and the design when present, then cuts the work into tasks sized for
sequential execution by `implement`. It may also rewrite `plan.md` as a phase
and task map when that makes downstream work clearer.

The task files are consumed by `implement`; `test` and `docs` may read them to
understand the real scope. This skill owns only `NN-task.md` and the
navigational normalization of `plan.md`. It does not write product code, does
not write `tests.md`, and does not change other `./workflow/` documents beyond
the feature's status. The status in `./workflow/PLAN.md` only signals that the
feature is ready for step-by-step implementation.

## Parameters

Use `args` to name the feature:

```txt
<feature-slug>
```

- `feature-slug` is the directory name under `./workflow/features/`.
- If `feature-slug` is absent, infer it from the current user message and the
  active entries in `./workflow/PLAN.md`, but only when exactly one feature has
  a `plan.md` and matches the request.
- If the feature still cannot be identified, ask one short question for the
  slug and stop until answered.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs,
  branches, commits, pushes, checkout commands, or worktree commands. The
  working tree is expected to be dirty; the user manages git.
- Own only `./workflow/features/{slug}/NN-task.md`, the navigational map in
  `./workflow/features/{slug}/plan.md`, and the matching service status in
  `./workflow/PLAN.md`.
- Do not write product code, `tests.md`, test code, `design.md`, user
  documentation, or developer documentation. Those concerns belong to
  `implement`, `test`, `design`, and `docs`.
- Do not change the substance of `plan.md`. Rewriting it is allowed only to
  turn it into a clear map of phases and the `NN-task.md` files created here.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4 before
  asking final questions or writing task files. Decomposing a plan into
  self-contained tasks is analytical work.
- Ask only blocking clarifying questions. A gap is blocking only when the plan
  cannot be sliced safely without the answer. Otherwise proceed and record the
  gap as a tail.
- Write this skill's text in English. Keep project prose and artifacts in the
  working language of the existing `./workflow/` files; keep paths, tool names,
  code identifiers, and status markers in their original spelling.
- Write the current task set only. Do not include conversation biography,
  previous plan states, "now/previously" comparisons, or migration notes.

## Steps

For this multi-step procedure, use the agent's task planning mode (todo list /
task plan, whichever is available) and close items one by one.

### 1. Identify the feature

Resolve `{slug}` from `args`, the user message, or `./workflow/PLAN.md`.

Stop and ask for the slug if:

- `./workflow/features/{slug}/` does not exist;
- `./workflow/features/{slug}/plan.md` is missing;
- several active features could match the request.

Do not create a missing plan in this skill. If the feature has only a
`feature.md` and no `plan.md`, report that `planning` must run first and stop.

If `NN-task.md` files already exist for this feature, treat this run as an
update: rebuild the task set from the current `plan.md` rather than appending
to the old files.

### 2. Read the feature and plan context

Read, in this order:

- `./workflow/features/{slug}/plan.md` — the scope authority for the task set.
- `./workflow/features/{slug}/feature.md` — the feature intent the plan serves.
- `./workflow/features/{slug}/design.md`, if it exists — UI constraints that
  affect how UI work is sliced.
- `./workflow/PROJECT.md` and `./workflow/ARCHITECTURE.md`, when present — only
  to keep task file boundaries consistent with the stack and code placement
  rules.

Use `plan.md` as the scope authority. Do not add work that the plan does not
contain.

### 3. Survey the whole plan before slicing

Walk the entire plan once and cover every category of work at the same time —
foundation, data and model, API and contracts, UI and interaction,
integrations, rollout and runtime concerns. List every unit of work the plan
implies before cutting any task, so no part of the scope falls out of the task
set.

If the change the plan describes is obviously small and single-file, do not
create `NN-task.md` files. Instead leave `plan.md` as the working scope, note
in the final report that the feature goes straight to `implement` off
`plan.md` or `feature.md`, and still update `./workflow/PLAN.md` as described
in Step 8.

### 4. Decompose with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking` and reason through:

- the full list of work units from the survey, grouped into ordered tasks;
- the size of each task: small enough to implement and verify in one focused
  session — split any task that spans many files or many subsystems, or whose
  name contains "and";
- the execution order and the dependency of each task on earlier tasks;
- the file boundaries of each task: which files it touches and which it must
  leave alone, so two tasks do not collide;
- for each task: input documents, goal, scope of work, and a verifiable
  expected result that states plainly when the task is done;
- whether `plan.md` should be rewritten as a phase and task map, and what that
  map looks like;
- which gaps genuinely block a safe slicing and which can be recorded as tails.

A task is self-contained when an executor can pick it up and act on it without
chat history and without reading sibling `NN-task.md` files, beyond the named
task dependencies. Each task carries its own context and a short reason it
exists. If a task cannot be made self-contained, split it further or state in
the task file what context is still missing.

### 5. Ask blocking questions only

Try to close gaps from `plan.md`, `feature.md`, the workflow files, and the
codebase before asking the user.

Ask at most three concise questions in one block. For each question, put the
recommended answer first with `(Recommended)` and a short reason, and offer
only materially different options.

If a gap does not block a safe slicing, record it as a tail instead of
interrupting the user.

### 6. Write the `NN-task.md` files

Write one `./workflow/features/{slug}/NN-task.md` per task, using the Artifact
Requirements below. Number the files with a zero-padded `NN` (`01`, `02`, ...)
in execution order. Describe the current task only, with no history of how the
decomposition was produced.

### 7. Normalize `plan.md` as a map

If a phase and task map makes downstream work clearer, rewrite
`./workflow/features/{slug}/plan.md` so it lists the phases and points each
phase at its `NN-task.md` files. Keep the plan's content; turn it into a map,
do not strip its substance. If the existing `plan.md` is already clear, leave
it as is.

### 8. Update `./workflow/PLAN.md`

Set the feature status to `[+]` (split into tasks).

Preserve the file's structure, other feature entries, and their status
markers. Change only the line for this feature. Use the existing local format.

### 9. Final verification

Reread the written `NN-task.md` files, the `plan.md` map, and the changed
`./workflow/PLAN.md` entry.

Confirm:

- every work unit from the survey landed in a task; nothing fell out;
- each task is sized for one focused implement-and-verify session;
- each task has an input, a goal, file boundaries, and a verifiable expected
  result;
- each task can be handed to `implement` with no chat retelling;
- task dependencies and numbering reflect a workable execution order;
- no product code, `tests.md`, `design.md`, or documentation was created;
- the task set describes the current desired work without biography or delta
  wording.

## Artifact Requirements

Write each `./workflow/features/{slug}/NN-task.md` in the working language of
the existing `./workflow/` files.

Use this structure, scaled to the task's size:

```md
# NN. <task title>

## Зачем

One or two lines: why this task exists and what part of the feature it moves.

## Вход

- Source plan items and documents this task derives from.

## Цель

The behavior or result the task must produce.

## Границы файлов

- Likely files to touch
- Files or areas to leave alone

## Объём работ

- Concrete, ordered, verb-first units of work.

## Зависимости

- None / task NN / external answer.

## Ожидаемый результат

A clear, verifiable done-signal: what must be true for the task to count as
complete. This is a definition of done, not a list of test cases — the test
plan belongs to `test`.
```

The task set is ready when:

- another agent can act on any single task without conversation history;
- each task is self-contained and carries its own context and reason;
- file boundaries keep two tasks from colliding;
- the expected result of each task is verifiable;
- the `plan.md` map, if rewritten, points cleanly at the task files.

If the plan is too thin to slice safely, keep this skill's boundary: do not
fabricate tasks. Report what input is missing and which earlier workflow stage
must run first.

## Updating PLAN.md

At the end, set the matching feature in `./workflow/PLAN.md` to status `[+]`.

The status markers are:

- `[ ]` new;
- `[-]` planned;
- `[+]` split into tasks;
- `[x]` implemented;
- `[*]` tested;
- `[/]` archived.

Change only this feature's line. Do not touch statuses or entries for other
features.

## Notes

- A missing `design.md`, `PROJECT.md`, or `ARCHITECTURE.md` does not stop this
  skill. Proceed with the available context and avoid inventing constraints.
- If `plan.md` contradicts the actual codebase, slice around what is verified
  and record the conflict as a tail rather than guessing.
- If the user's request is really to plan, improve, design, implement, test,
  or document the feature, report the matching skill instead of doing that work
  here.
- Keep the output operational: the next agent should see the current task set,
  not a story about how it was built.
