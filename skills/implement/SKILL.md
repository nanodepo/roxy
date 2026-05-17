---
name: implement
description: >
  Writes product code for a feature and toggles completed plan checkboxes from
  NN-task.md, plan.md item, or simple feature.md. Triggers: implement task,
  implement feature, write code, build feature, continue implementation.
---

# Implement

## Purpose

Turn defined scope -> working product code. Scope = one task, one plan phase, or
simple `feature.md` with clear result. Sync real progress in feature `plan.md`
and status in `./workflow/PLAN.md`.

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
- Toggle `plan.md` checkbox done only after matching behavior works in code.
- Set feature `[x]` in `./workflow/PLAN.md` only when whole impl scope done.
- If `plan.md`, task file, or `design.md` conflicts with actual code, stop and
  report contradiction.

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

Use task planning mode for this multi-step flow + inner impl/verify loop. Close
items one by one.

### 1. Identify feature + scope

Read `./workflow/features/{slug}/feature.md` first.

Choose scope source:

1. If `./workflow/features/{slug}/NN-task.md` files exist, use task named by
   `scope`, else lowest-number unfinished task.
2. Else if `plan.md` exists, use item named by `scope`, user-pointed item, or
   nearest unchecked item.
3. Else work from `feature.md` only for simple bounded feature.

Complex feature + no usable source -> stop, report `planning` or `task` needed.

### 2. Read narrow context + gate

Read only inputs that affect change:

- `./workflow/features/{slug}/design.md` and `./workflow/DESIGN.md` for UI /
  user-visible interaction.
- `./workflow/ARCHITECTURE.md` for architecture / module boundaries.
- `./workflow/PROJECT.md` for stack, run, build commands.

Pass gate before edits:

- Scope source clear: `NN-task.md`, `plan.md` item, or simple `feature.md`.
- Required behavior understood.
- Design, architecture, stack constraints known where relevant.
- At least one local pattern found, or absence understood.
- Risky actions identified: schema, deps, auth/security, migration, destructive
  ops.
- Each risky action grounded in scope, or raised with user first.

Keep context narrow: touched files, related types/interfaces/configs/neighbor
modules. No whole-codebase sweep.

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

### 7. Sync `plan.md`

After scope item works:

- Toggle checkbox done only when behavior works.
- Partial item stays unchecked; add short tail for remainder.
- If impl differs from plan wording, reword item to current truth.
- If mandatory sub-item found, add unchecked tail.

### 8. Self-check

Before finish, verify:

- Correctness: stated behavior works; in-scope edge cases handled.
- Fit: local patterns, naming, module boundaries.
- Simplicity: no extra abstraction, broad cleanup, unrelated rewrite.
- Safety: user input, auth, secrets, external data, destructive ops.
- Verification: check run, or reason not run.
- Workflow: checkboxes, feature status, tails match reality.

### 9. Update status + tails

If whole feature impl scope done, set `[x]` in `./workflow/PLAN.md`.

If work remains for `test`, `docs`, `planning`, `design`, or user, record tail:
feature-level in `plan.md`, cross-stage in `./workflow/PLAN.md`. Do not mark
unfinished work done.

## Artifact Requirements

No report file. Artifacts:

- Product code changes, scoped to chosen task/phase/simple feature.
- Updated `plan.md`: real checkboxes, wording matching impl, new tails.
- Updated `./workflow/PLAN.md`: `[x]` only when whole impl scope complete.

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
