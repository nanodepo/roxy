---
name: implement
description: >
  Writes the product code for a feature and toggles completed plan checkboxes,
  working from an NN-task.md, a plan.md item, or a simple feature.md. Use for
  "implement task", "implement feature", "write the code", "build the feature",
  or "continue implementation".
---

# Implement

## Purpose

This skill turns an already-defined scope into working product code. It
implements one task, one plan phase, or a simple feature taken directly from
`feature.md`, then synchronizes actual progress in the feature's `plan.md` and
the service status in `./workflow/PLAN.md`.

It runs in the feature pipeline normally after `task`, but it can also work off
`plan.md` or directly off `feature.md` for a simple feature with a clear
result. The stages after it are `test` and `docs`.

The skill owns implementation only. It does not create or rewrite a feature
plan, does not write `tests.md`, does not write test code, and does not write
documentation. Those concerns belong to `planning`, `improve`, `test`, and
`docs`. The status in `./workflow/PLAN.md` only reflects implementation
completion.

## Parameters

Use `args` to name the feature and, optionally, the scope:

```txt
<feature-slug> [scope]
```

- `feature-slug` — the feature directory under `./workflow/features/`.
- `scope` — optional: a task number (e.g. `03`), a phase name, or `next`.

Resolve missing parameters:

- If `feature-slug` is absent, infer the feature from the current user message
  and the active features in `./workflow/PLAN.md`.
- If `scope` is absent, pick the scope source by the order in Step 1.
- If the feature still cannot be identified, ask one short question and stop
  until answered.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs,
  branches, commits, pushes, checkout, or worktree commands. The working tree
  is expected to be dirty; the user manages git.
- Implement only an already-defined scope. Do not create a new plan for a
  complex feature and do not rewrite `plan.md` as a planning act — that is the
  work of `planning` or `improve`. If a complex feature has no usable scope
  source, stop and report that `planning` or `task` must run first.
- Do not write `tests.md`, do not write test code, and do not write user or
  developer documentation. Running existing tests as a verification step is
  allowed; authoring tests is not.
- Do not perform broad cleanup or unrelated refactoring. Allow only the small,
  local refactor that the stated change directly requires.
- Toggle a `plan.md` checkbox to done only when the matching behavior is
  actually implemented in working code — never by intention.
- Set a feature's status to `[x]` in `./workflow/PLAN.md` only when the whole
  implementation scope of that feature is complete.
- If `plan.md`, a task file, or `design.md` conflicts with the actual codebase,
  stop and surface the contradiction to the user instead of guessing.
- Write this skill's text in English. Keep project prose, code, and artifacts
  in the project's working language; keep paths, tool names, and identifiers in
  their original spelling.

## Steps

For this multi-step procedure with an inner implement-and-verify cycle, use the
agent's task planning mode (todo list / task plan, whichever is available) and
close items one by one.

### 1. Identify the feature and working scope

Read `./workflow/features/{slug}/feature.md` first.

Then choose the scope source in this order:

1. If `./workflow/features/{slug}/NN-task.md` files exist, work from the task
   named by `scope`, or the lowest-numbered unfinished task.
2. If there is no task file but a `plan.md` exists, take the plan item named by
   `scope`, the item the user pointed at, or the nearest unchecked item.
3. If there is no plan at all, work directly from `feature.md` — but only for a
   simple feature with a clear, bounded result.

If the feature is complex and no usable scope source exists, stop and report
that `planning` or `task` must run first.

### 2. Pass the readiness gate and load narrow context

Read the inputs that affect the change:

- `./workflow/features/{slug}/design.md` and `./workflow/DESIGN.md` — when the
  task touches UI or user-visible interaction.
- `./workflow/ARCHITECTURE.md` — when the task changes architecture or module
  boundaries.
- `./workflow/PROJECT.md` — for stack, run, and build commands.

Confirm the readiness gate before editing:

- The scope source is clear: an `NN-task.md`, a `plan.md` item, or a simple
  `feature.md`.
- The behavior that must appear is understood.
- The constraints from design, architecture, and the project stack are known.
- At least one local pattern is found, or it is clear why none exists.
- Risky actions are identified up front: schema changes, dependency changes,
  auth or security changes, data migration, destructive operations.
- Each risky action has an explicit basis in the scope, or it is raised with
  the user before proceeding.

Keep context narrow: read the files the change will touch, their related
types, interfaces, configs, and neighboring modules — not the whole codebase.

### 3. Locate the files to change and a local pattern

Find the exact files the change touches and at least one existing example of a
similar pattern in the codebase. Follow that pattern's structure, naming, and
module boundaries so the change does not introduce an accidental abstraction.

If the change depends on an unstable API, a new library version, framework
conventions, or a deprecation, check the current official documentation rather
than implementing from memory. Note briefly in the final report what was
checked when external documentation influenced the code.

### 4. Write a short execution outline

For yourself, state in a few lines: what changes, where, and how the result
will be verified. For a complex task, order the work by dependency, not by
visible importance.

### 5. Implement in thin, verifiable slices

Work the chosen scope item until it is done or blocked:

- Implement one minimal, complete slice of behavior at a time.
- Do not write a large body of code before the first verification.
- Verify each slice by the available means: build, lint, typecheck, existing
  tests, manual check, or careful static review.
- Each slice must leave the project in a working or explicitly diagnosed state.
- Do not parallelize dependent edits without need.
- Repeat the cycle for the next slice.

### 6. Handle a failed verification

If a build, linter, runtime, or manual check breaks:

- Stop adding new functionality.
- Capture short evidence: the command, the error, the affected file, and the
  observed behavior.
- Diagnose the minimal cause and fix it within the current scope, without
  mixing in new scope.
- If the error lies outside the current task, raise it with the user or record
  it as a tail instead of expanding the work.

### 7. Synchronize progress in plan.md

After a scope item is implemented:

- Toggle its `plan.md` checkbox to done only when the behavior is actually
  working.
- If an item is only partially done, leave its checkbox unchecked and add a
  short tail describing what remains.
- If the task was done differently than the plan describes, reword that
  `plan.md` item so the artifact reflects the real state.
- If a new mandatory sub-item is discovered, add it to `plan.md` as an
  unchecked tail.

### 8. Run a light self-check

Before finishing, review the change:

- Correctness — the stated behavior works; edge cases in the current scope are
  not ignored.
- Fit — the code follows local patterns, naming, and module boundaries.
- Simplicity — no extra abstraction, no broad cleanup, no unrelated rewriting.
- Safety — user input, auth, secrets, external data, and destructive actions
  are handled carefully.
- Verification — an available check was run, or it is stated explicitly why it
  could not be.
- Workflow — checkboxes, feature status, and tails reflect the real state.

### 9. Update the feature status and record tails

If the whole implementation scope of the feature is now complete, set its
status to `[x]` in `./workflow/PLAN.md`.

If work remains for `test`, `docs`, `planning`, `design`, or the user, record
it as a tail — in `plan.md` for feature-level follow-up, or in
`./workflow/PLAN.md` for cross-stage follow-up. Do not mask unfinished work as
done.

## Artifact Requirements

This skill produces no separate report file. Its artifacts are:

- **Product code changes** in the project, scoped to the chosen task, phase, or
  simple feature.
- **Updated `plan.md`** — checkboxes that match actual progress and item
  wording that matches the real implementation, plus any newly discovered
  tails.
- **Updated `./workflow/PLAN.md`** — the feature's service status set to `[x]`
  only when the whole implementation scope is complete.

## Updating PLAN.md

At the end, touch `./workflow/PLAN.md` only to:

- set the feature status to `[x]` when its entire implementation scope is done;
- append cross-stage tails the implementation revealed.

Do not change other features' statuses and do not rewrite unrelated entries.
The status markers are: `[ ]` new, `[-]` planned, `[+]` split into tasks,
`[x]` implemented, `[*]` tested, `[/]` archived.

## Notes

- A missing `design.md`, `ARCHITECTURE.md`, `DESIGN.md`, or `PROJECT.md` does
  not stop this skill — proceed on the available context and note any
  constraint that could not be confirmed.
- If the request is really a planning, design, test, or documentation request,
  report the matching skill and do not implement outside this skill's scope.
- If the scope is too vague to implement safely, do not invent requirements
  inside this skill — ask one short question or hand the work back to
  `planning`.
- Write only the current state of code and artifacts. Do not leave biography,
  "previously / now" comparisons, or migration commentary in the code or in
  `plan.md`.
