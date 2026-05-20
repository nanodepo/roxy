---
name: implement
description: >
  Writes product code for one NN-task.md, plan.md item, or simple feature.md;
  syncs task-map progress. Triggers: implement task, implement feature, write
  code, build feature, continue implementation.
---

# Implement

## Purpose

Turn defined scope -> working product code. Scope = one task, one plan phase, or
simple `feature.md` with clear result. Sync real progress in the task file,
feature `plan.md` task map, and status in `./workflow/PLAN.md`.

Normal stage: after `task`. Also can work from `plan.md` or simple
`feature.md`. Next stages: `test`, `docs`.

Own impl only. Do not create/rewrite plans, write `tests.md`, write test code,
or write docs. Those belong to `planning`, `improve`, `test`, `docs`.
`./workflow/PLAN.md` status reflects impl only.

## Params

Use `args`:

```txt
<feature-slug> [scope]
```

- `feature-slug`: dir under `./workflow/features/`.
- `scope`: optional task number (`03`), phase name, or `next`.

Resolve missing:

- No `feature-slug`: infer from user msg + active features in
  `./workflow/PLAN.md`.
- No `scope`: choose source by Step 1 order.
- Still unknown feature: ask one short question, stop.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout, worktree.
  Dirty tree expected; user owns git.
- Impl only defined scope. Do not plan complex feature or rewrite `plan.md` as
  planning. If complex feature has no usable scope source, stop: run
  `planning` or `task` first.
- Do not write `tests.md`, test code, user docs, or dev docs. Existing tests OK
  for verification.
- No broad cleanup / unrelated refactor. Only small local refactor directly
  needed by change.
- When `NN-task.md` files exist, implement exactly one selected task. Do not
  read sibling task files except named dependencies from the selected task.
- Toggle task and `plan.md` task-map checkboxes done only after matching
  behavior works in code.
- Set feature `[x]` in `./workflow/PLAN.md` only when whole impl scope done.
- If `plan.md`, task file, or `design.md` conflicts with actual code, stop and
  report contradiction.

## Current-State Discipline

Leave the project ready for the next agent to continue from current files alone.
Implementation output is not a session log.

- Code expresses current behavior. Names, comments, configs, and local contracts
  describe what is true now, not what changed during this session.
- Remove obsolete in-scope code, flags, comments, TODOs, and transitional
  scaffolding when they no longer serve current runtime behavior.
- Keep compatibility, migration, or fallback paths only when they are active
  product, operational, security, or data-safety constraints.
- Rewrite nearby current-state artifacts instead of appending delta notes. A
  completed item says what works; a remaining tail says what is still required.
- Do not preserve history in hot code, `plan.md`, or `./workflow/PLAN.md` unless
  that history controls current behavior.

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

Use task planning mode for this multi-step flow + inner impl/verify loop. Close
items one by one.

### 1. Identify feature + scope

Read `./workflow/features/{slug}/feature.md` first.

Choose scope source:

1. If `./workflow/features/{slug}/NN-task.md` files exist, read compact
   `plan.md` task map and use task named by `scope`, else the lowest-number
   unchecked task row in `plan.md`, else the lowest-number unfinished task file.
2. Else if `plan.md` exists, use item named by `scope`, user-pointed item, or
   nearest unchecked item.
3. Else work from `feature.md` only for simple bounded feature.

Complex feature + no usable source -> stop, report `planning` or `task` needed.

### 2. Read narrow context + gate

Read only inputs that affect change:

- selected `./workflow/features/{slug}/NN-task.md`, when task files exist;
- compact `./workflow/features/{slug}/plan.md`, for shared context, task order,
  and task-map progress;
- `./workflow/features/{slug}/design.md` and `./workflow/DESIGN.md` for UI /
  user-visible interaction.
- `./workflow/ARCHITECTURE.md` for architecture / module boundaries.
- `./workflow/PROJECT.md` for stack, run, build commands.

Treat these files as current operating context. Changelogs, commit history, old
task notes, archived features, and historical comments are evidence only when
they explain an active constraint in the current scope.

Pass gate before edits:

- Scope source clear: `NN-task.md`, `plan.md` item, or simple `feature.md`.
- Required behavior understood.
- Selected task contains enough task-specific context; read named dependencies
  only when the task declares them.
- Design, architecture, stack constraints known where relevant.
- At least one local pattern found, or absence understood.
- Risky actions identified: schema, deps, auth/security, migration, destructive
  ops.
- Each risky action grounded in scope, or raised with user first.

Keep context narrow: touched files, related types/interfaces/configs/neighbor
modules, selected task, compact task map, and named task dependencies. No
whole-codebase sweep.

### 3. Locate files + pattern

Find exact files to change and one similar local pattern. Follow its structure,
naming, module boundaries. Avoid accidental abstraction.

If change depends on unstable API, new library version, framework convention, or
deprecation, check current official docs instead of memory. Final report notes
what docs influenced code.

### 4. Write short outline

For self: what changes, where, verify how. Complex task: order by dependency.

### 5. Impl thin slices

Work chosen scope until done or blocked:

- Impl one minimal complete behavior slice.
- Update local names, comments, types, configs, and guards so the touched area
  states the current behavior plainly.
- Delete replaced in-scope branches or scaffolding once the current path covers
  the requirement.
- Verify before large next code body.
- Verify via available check: build, lint, typecheck, existing tests, manual
  check, or static review.
- Each slice leaves project working or explicitly diagnosed.
- No dependent edit parallelism without need.
- Repeat.

### 6. Failed verification

On build/lint/runtime/manual failure:

- Stop adding feature work.
- Capture evidence: command, error, affected file, observed behavior.
- Diagnose minimal cause; fix inside current scope.
- If outside task, raise with user or record tail. Do not expand scope.

### 7. Sync workflow artifacts

After scope item works:

- If working from `NN-task.md`, toggle its `## Status` checkbox done only when
  behavior works.
- Toggle matching `plan.md` task-map checkbox done only when behavior works.
- If working directly from a pre-task `plan.md` item, toggle that item only when
  behavior works.
- Partial item stays unchecked; add short tail for remainder.
- If impl differs from plan wording, reword item to current truth.
- If mandatory sub-item found, add unchecked tail.
- Keep task-specific remainder in the task file when task files exist; keep only
  shared tails in `plan.md`.
- Remove or rewrite stale wording in touched workflow artifacts. Do not add
  implementation diary notes, "changed from" explanations, or completed-session
  summaries.

### 8. Self-check

Before finish, verify:

- Correctness: stated behavior works; in-scope edge cases handled.
- Fit: local patterns, naming, module boundaries.
- Simplicity: no extra abstraction, broad cleanup, unrelated rewrite.
- Safety: user input, auth, secrets, external data, destructive ops.
- Verification: check run, or reason not run.
- Workflow: selected task status, plan task-map checkbox, feature status, and
  tails match reality.
- Handoff: touched code and workflow artifacts are enough to continue from;
  no obsolete comments, temporary notes, or inactive scaffolding remain in scope.

### 9. Update status + tails

If all task-map rows or the whole direct impl scope are done, set `[x]` in
`./workflow/PLAN.md`.

If work remains for `test`, `docs`, `planning`, `design`, or user, record tail:
feature-level in `plan.md`, cross-stage in `./workflow/PLAN.md`. Tail wording is
an active next requirement or blocker, not a recap. Do not mark unfinished work
done.

## Artifact Requirements

No report file. Artifacts:

- Product code changes, scoped to chosen task/phase/simple feature.
- Updated selected `NN-task.md` status and tails when task files exist.
- Updated `plan.md`: task-map checkbox, direct plan checkbox, shared wording, or
  shared tails matching impl.
- Updated `./workflow/PLAN.md`: `[x]` only when whole impl scope complete.

Artifacts must describe the current implementation surface. Do not create
handoff reports, migration notes, or historical summaries as implementation
outputs.

## Updating PLAN.md

At end, touch `./workflow/PLAN.md` only to:

- set feature `[x]` when entire impl scope done;
- append cross-stage tails found by impl.

Do not alter other feature statuses or unrelated entries. Status markers:
`[ ]` new, `[-]` planned, `[+]` split into tasks, `[x]` implemented, `[*]`
tested, `[/]` archived.

## Notes

- Missing `design.md`, `ARCHITECTURE.md`, `DESIGN.md`, or `PROJECT.md` does not
  stop skill. Proceed from available context; note unknown constraint.
- If request is planning, design, test, or docs, report matching skill; do not
  implement outside scope.
- If scope too vague for safe impl, do not invent reqs. Ask one short question
  or hand back to `planning`.
- Write current state only. No biography, "previously/now" comparisons, or
  migration commentary in code or `plan.md`.
