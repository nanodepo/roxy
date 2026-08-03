---
name: task
description: >
  Splits a feature plan into self-contained NN-task.md files plus a compact
  task map in plan.md, and marks the feature split in ./workflow/PLAN.md. Use
  when the user says "split plan into tasks", "break down the plan", "create
  task files", or "decompose feature into tasks".
---

# Task — Split a Feature Plan into Self-Contained Tasks

## Purpose

This skill turns `./workflow/features/{slug}/plan.md` into a set of
self-contained `./workflow/features/{slug}/NN-task.md` files sized for
sequential `implement` runs, and rewrites `plan.md` into a compact map: short
shared context plus task rows with checkboxes and links. It runs after
`planning` or `improve`, when a plan is too large to implement in one pass.

The split exists for context economy. During implementation the executor reads
the map plus exactly one task file — not the full plan and not sibling tasks —
so each task must carry everything it needs: its reason, its own context, its
steps, its file boundaries, and a verifiable done-signal. The map must not be
required to understand any task's implementation steps.

The skill owns only the `NN-task.md` files, the map form of `plan.md`, and the
feature's status line in `./workflow/PLAN.md`. It does not write code,
`tests.md`, `design.md`, or documentation — those belong to `implement`,
`test`, `design`, and `docs`.

## Parameters

`args` names the feature:

```txt
<feature-slug>
```

- `feature-slug` is a directory under `./workflow/features/`.
- When `args` is empty, infer the slug from the user's request and the active
  entries in `./workflow/PLAN.md`, but only when exactly one planned feature
  matches. Otherwise ask one short question and stop.
- When `./workflow/features/{slug}/plan.md` does not exist, report that
  `planning` must run first and stop. Do not fabricate a plan.

## Strict Rules

- Do not perform git operations of any kind; the user owns git, a dirty tree
  is expected.
- Write only `./workflow/features/{slug}/NN-task.md`, the map form of
  `./workflow/features/{slug}/plan.md`, and this feature's line in
  `./workflow/PLAN.md`. No code, no `tests.md`, no `design.md`, no docs.
- Treat `plan.md` as the scope authority and `feature.md` as the intent
  authority. Do not add implementation work absent from the plan.
- Move task-specific substance out of `plan.md` into the matching task file;
  the map keeps only what most tasks share. Do not duplicate the feature brief.
- Describe the current task set in the present tense — no decomposition
  history, no "previously/now" comparisons, no migration notes.

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

This is a multi-step flow; track it in task planning mode.

1. **Resolve the feature.** Identify `{slug}` per Parameters and read, in
   order: `plan.md`, existing `NN-task.md` files when present (an update run
   rebuilds the task set from current truth, never appends stale tasks),
   `feature.md`, `design.md` if present, and `PROJECT.md` /
   `ARCHITECTURE.md` for stack and code-placement boundaries.

2. **Judge whether a split is needed.** If the plan is small enough for one
   focused implementation pass, create no task files: leave `plan.md` as the
   working scope, keep the feature `[-]` in `./workflow/PLAN.md`, and report
   that the feature goes straight to `implement`.

3. **Survey and decompose.** Walk the whole plan once and list every implied
   unit of work so no scope falls out. Group the units into ordered tasks,
   each sized for one focused implement-and-verify session — split a task that
   spans many subsystems or whose name needs an "and". For each task decide
   its goal, its own context, its file boundaries (so tasks do not collide),
   its dependencies on earlier tasks, and a verifiable expected result. Decide
   what little context is genuinely shared and belongs in the map.

4. **Ask blocking questions.** Close gaps from the plan, the brief, the
   workflow files, and the codebase first. Ask the user only what blocks safe
   slicing, in one batch, with the recommended answer first marked
   `(Recommended)`. Non-blocking gaps become tails, not questions.

5. **Write the task files.** One `NN-task.md` per task, zero-padded in
   execution order (`01`, `02`, ...), per Artifact Requirements.

6. **Rewrite `plan.md` as the map.** Keep: the feature-level implementation
   goal in a few lines, links to shared inputs (`feature.md`, `design.md`,
   canon files), constraints and ordering rules most tasks need, the task rows
   with checkboxes, and active cross-task tails. Everything task-specific
   lives only in its task file. Task rows:

   ```md
   - [ ] <task result> | `./workflow/features/{slug}/NN-task.md`
   ```

7. **Update `./workflow/PLAN.md`.** Set this feature to `[+]` only when task
   files were created. Change only this feature's line.

8. **Verify and report.** Reread the task files and the map: every surveyed
   unit landed in a task, each task stands alone, file boundaries do not
   collide, the map carries no task-specific detail. Report the result to
   chat and end with the next step: run `implement` on task `01` (or, when no
   split was needed, run `implement` directly from `plan.md`).

## Artifact Requirements

Each `NN-task.md` is self-contained: the executor acts on the map plus this
one file, without chat history and without reading sibling tasks except
dependencies the task names explicitly. Core sections, scaled to the task —
a simple task may cover each in a line or two:

```md
# NN. <task title>

## Status

- [ ] Done

## Why

Why this task exists and what part of the feature it moves.

## Context

What the executor needs that is specific to this task: relevant plan
decisions, contracts, file boundaries (what to touch, what to leave alone),
dependencies on earlier tasks.

## Steps

Concrete, ordered units of work — results to achieve, not keystroke dictation.

## Done

A verifiable done-signal: what must be true for the task to count as
complete. A definition of done, not a test plan — tests belong to `test`.
```

Add a section beyond these only when it carries a decision.

## Updating PLAN.md

Status markers in `./workflow/PLAN.md`: `[ ]` new, `[-]` planned, `[+]` split
into tasks, `[x]` implemented, `[*]` tested, `[/]` archived. Set this feature
to `[+]` only when task files exist; otherwise leave it `[-]`. Do not touch
other entries.

## Notes

- Missing `design.md`, `PROJECT.md`, or `ARCHITECTURE.md` does not stop the
  skill; work from available context without inventing constraints.
- If `plan.md` contradicts the codebase, slice around verified truth and
  record the conflict as a tail instead of guessing.
- If the plan is too thin to slice safely, do not fabricate tasks: report
  which earlier stage must run first.
