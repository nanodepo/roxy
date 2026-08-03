---
name: implement
description: >
  Writes product code for one NN-task.md, one plan.md item, or a simple
  feature.md; syncs task and task-map checkboxes and the feature status, and
  finishes with a closing sweep of the touched code. Use for "implement task",
  "implement feature", "write code", "build feature", "continue
  implementation".
---

# Implement — Turn One Defined Scope into Working Code

## Purpose

This skill turns one defined scope into working product code. The scope is one
`NN-task.md`, one `plan.md` item or phase, or a simple `feature.md` with a
clear result. It is the normal stage after `task`, and it also works straight
from a compact plan or a simple brief. As the work lands, the skill syncs real
progress: the task's status checkbox, the task-map checkbox in `plan.md`, and
the feature status in `./workflow/PLAN.md`.

Context economy is the contract with `task`: when task files exist, the skill
reads the compact `plan.md` map plus exactly one selected task — not the full
plan and not sibling tasks, except dependencies the task names explicitly. The
task file is expected to be self-contained; if it is not, that is a defect to
report, not a reason to read everything.

The skill owns implementation only. It does not create or rewrite plans, write
`tests.md` or test code, or write documentation — those belong to `planning`,
`improve`, `test`, and `docs`. Existing tests may be run for verification.

## Parameters

`args` names the feature and optionally the scope:

```txt
<feature-slug> [scope]
```

- `feature-slug` is a directory under `./workflow/features/`.
- `scope` is an optional task number (`03`), phase or item name, or `next`.
- When `args` is empty, infer the feature from the user's request and the
  active entries in `./workflow/PLAN.md`. If it stays ambiguous, ask one short
  question and stop.
- When `scope` is omitted, take the lowest unchecked task row in the map, the
  nearest unchecked plan item, or the simple feature as a whole — in that
  order of availability.

## Strict Rules

- Do not perform git operations of any kind; the user owns git, a dirty tree
  is expected.
- Implement only the defined scope. A complex feature with no usable scope
  source means stop and report that `planning` or `task` must run first.
- Do not write `tests.md`, test code, user docs, or dev docs.
- No broad cleanup or unrelated refactoring while implementing; only local
  changes the scope directly needs. The closing sweep at the end cleans the
  touched code — and only the touched code.
- Workflow artifacts — `NN-task.md`, `plan.md`, `feature.md`, `design.md` —
  change only through the checkbox, tail, and divergence updates of step 5.
  Leave their wording, structure, and style as the authoring skill wrote them.
- Toggle a checkbox done only after the matching behavior actually works in
  code. Set the feature `[x]` in `./workflow/PLAN.md` only when the whole
  implementation scope is done.
- If the task file, `plan.md`, or `design.md` contradicts the actual code,
  stop and report the contradiction instead of guessing.

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

This is a multi-step flow with an inner implement-and-verify loop; track it in
task planning mode.

1. **Resolve feature and scope.** Identify the feature per Parameters and
   choose the scope source: a selected `NN-task.md` when task files exist
   (via the `plan.md` map), else a `plan.md` item, else a simple `feature.md`
   as a whole.

2. **Read narrow context.** Read the selected task file and the compact map;
   `feature.md` for intent; `design.md` and `./workflow/DESIGN.md` when the
   change is user-visible; `./workflow/ARCHITECTURE.md` for module boundaries;
   `./workflow/PROJECT.md` for stack and commands. Do not read sibling task
   files except named dependencies. Before editing, confirm you understand the
   required behavior, know the relevant constraints, and have identified risky
   actions (schema, dependencies, auth, destructive operations) — each risky
   action is grounded in the scope or raised with the user first.

3. **Locate files and pattern.** Find the exact files to change and one
   similar local pattern; follow its structure, naming, and boundaries. When
   the change depends on an unstable API or a library convention, check
   current official docs instead of memory.

4. **Implement in thin slices.** Implement one minimal complete slice of
   behavior, verify it with whatever check is available (build, lint,
   typecheck, existing tests, manual check, static review), and repeat until
   the scope is done or blocked. Each slice leaves the project working or
   explicitly diagnosed. On a failed check, stop adding feature work, diagnose
   the minimal cause, and fix it inside the scope; a cause outside the scope
   becomes a question to the user or a tail, never silent scope expansion.

5. **Sync workflow artifacts.** When the scope's behavior works: toggle the
   task's `## Status` checkbox, the matching task-map checkbox in `plan.md`,
   or the direct plan item — whichever applies. A partial item stays unchecked
   with a short tail for the remainder. If the implementation diverged from
   the plan wording, reword that one item to current truth and leave the rest
   of the file untouched. Task-specific tails live in the task file; only
   shared tails go to `plan.md`.

6. **Closing sweep.** Walk the touched code once and remove what the change
   orphaned: unused methods, classes, fields and database tables, dead
   imports, obsolete flags and branches, temporary scaffolding, stale
   comments. Orphans outside the scope are recorded as tails, not chased.
   Then normalize the code and comments you touched — present tense, no
   session biography, no "was X, now Y" wording, one canonical statement per
   rule; run the `markov` skill on the touched code when the skill mechanism
   is available, otherwise apply the same discipline by hand. The sweep covers
   code only: the task file, `plan.md`, `feature.md`, and `design.md` are not
   normalized or restyled here.

7. **Update status and report.** Set the feature `[x]` in
   `./workflow/PLAN.md` when all task-map rows or the whole direct scope are
   done; record cross-stage tails there. Report to chat what works now, what
   was verified and how, and which tails remain. End with the next step:
   unchecked tasks remain → `implement` again with the next task; everything
   is implemented → `test`; the feature has no testable logic → `docs`.

## Artifact Requirements

No report file. The artifacts are:

- product code changes, scoped to the chosen task, item, or simple feature;
- the selected `NN-task.md` with current status and tails, when task files
  exist;
- `plan.md` with checkboxes, wording, and shared tails matching reality;
- `./workflow/PLAN.md` with `[x]` only when the whole implementation scope is
  complete.

The code describes the current implementation surface — what is true now, not
what happened during the session. In the workflow artifacts only the status
markers, tails, and a diverged item carry that update; the rest of their text
stays as it is.

## Updating PLAN.md

Status markers in `./workflow/PLAN.md`: `[ ]` new, `[-]` planned, `[+]` split
into tasks, `[x]` implemented, `[*]` tested, `[/]` archived. Touch the file
only to set this feature `[x]` or to append cross-stage tails. Do not alter
other entries.

## Notes

- Missing `design.md`, `DESIGN.md`, `ARCHITECTURE.md`, or `PROJECT.md` does
  not stop the skill; proceed from available context and note the unknown
  constraint.
- If the request is really planning, design, testing, or docs work, name the
  matching skill instead of doing it here.
- If the scope is too vague to implement safely, do not invent requirements:
  ask one short question or hand back to `planning`.
