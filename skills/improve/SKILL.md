---
name: improve
description: >
  Verifies an existing feature plan as a second pass and applies minimal
  targeted fixes without changing the plan's structure. Triggers: improve
  plan, review plan, second pass, strengthen plan, find gaps in plan.
---

# Improve — Verify a Plan and Fix It Pointwise

## Purpose

This skill is a verification pass over `./workflow/features/{slug}/plan.md`
for complex or architecturally significant features, often run by a
different model than the one that wrote the plan. It checks the plan against
the feature brief, the project canon, and the actual codebase, then applies
minimal targeted edits: phantom paths and modules, hidden prerequisites,
holes in ordering, scope beyond the brief, claims the code does not support.

The skill is a reviewer with a pen, not a second planner. It preserves the
structure and language the plan received from `planning` — the same
sections, the same phase or step layout, the same mode. A full rewrite
happens only when the plan fundamentally misleads the implementer, and only
after the user confirms it.

When `NN-task.md` files exist, `plan.md` is a compact task map; the skill
then checks only the map, its shared constraints, and the ordering. Task
substance belongs to the `task` skill and stays untouched.

## Parameters

`args` names the feature:

```txt
<feature-slug>    # directory under ./workflow/features/
```

- `args` is empty → infer the slug from the user's message and the active
  entries in `./workflow/PLAN.md`, but only when exactly one feature has
  both `feature.md` and `plan.md` and matches the request. Otherwise stop
  and ask one short question.
- `feature.md` or `plan.md` is missing for the slug → stop and report it.
  Do not create either file.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty working tree is
  expected.
- Own only `./workflow/features/{slug}/plan.md` plus service status and
  tails in `./workflow/PLAN.md`. Do not write `tests.md`, `NN-task.md`,
  code, documentation, report files, or scorecards.
- Preserve the plan's structure, language, and mode; edits are pointwise.
  A full rewrite requires the user's explicit confirmation.
- Ground every fix in verified evidence — the brief, the canon files, or
  code actually read. Do not rephrase for taste.
- Do not expand scope beyond `feature.md`; remove speculative work instead
  of adding it.
- Keep the plan in the present tense. Edits leave no review narrative,
  no "fixed X" traces, no comparison with the previous plan text.
- Do not invoke downstream skills; finish with a recommendation.

## Language Notice

Write user-facing chat output and generated or rewritten project artifacts in
the working language of the target project. Detect it from existing
`./workflow/` files, project documentation, and the user's request. If the
project language is unclear, use the user's current language.

When editing an existing artifact, preserve its language unless the user
explicitly asks to translate it.

Apply the chosen artifact language to all prose, headings, table headers,
labels, placeholders, and examples. Keep file paths, commands, tool names,
code identifiers, framework names, package names, status markers, and
established product terms in their original spelling.

Do not mix languages inside one artifact unless the existing project canon
already does so or a quoted or source term requires it.

## Steps

Use task planning mode (todo list) for this multi-step flow.

1. **Resolve the feature.** Turn `args`, the user's message, and
   `./workflow/PLAN.md` into one `{slug}` as described in Parameters.

2. **Read the plan context.** Read
   `./workflow/features/{slug}/feature.md` (the scope authority),
   `plan.md`, `design.md` if it exists, and `./workflow/PLAN.md`. Read
   `PROJECT.md`, `ARCHITECTURE.md`, and `DESIGN.md` when relevant. Detect
   task-map mode from the presence of `NN-task.md` files; read only their
   names and status lines to verify the map.

3. **Verify against the codebase.** Start from every path, module, symbol,
   route, and command the plan names and check that each exists or is
   plausibly creatable where stated. Look for hidden prerequisites the plan
   skips, reuse claims the local code does not support, existing
   functionality the plan would rebuild, and ordering that breaks real
   dependencies. Keep the scan bounded to what the plan asserts.

4. **Decide the fixes.** Classify each finding: fix in `plan.md` when the
   plan as written would mislead implementation; service tail in
   `./workflow/PLAN.md` when the finding belongs to `test`, `docs`,
   `design`, or the user; leave alone when it is speculative or out of
   scope. If the plan is fundamentally misleading — wrong approach,
   fictional architecture — stop and ask the user whether to rewrite it;
   never rebuild it silently.

5. **Apply the edits.** Make the pointwise fixes inside the plan's existing
   structure. After editing, the plan still stands alone for the next
   stage, names only verified paths and dependencies, orders work by real
   dependencies, and contains nothing owned by `test`, `docs`, `task`, or
   the brief.

6. **Update `./workflow/PLAN.md`.** The feature stays `[-]`. Add a concise
   service tail only when verification revealed later-stage work, using the
   local format or, absent one, a line under the feature such as
   `- tail/test: <live invariant or risk to cover later>`. Touch nothing
   else.

7. **Report and recommend.** List the applied fixes in chat, one line per
   fix; a clean pass with no findings is a valid result and is reported as
   such. End with the next step: a large multi-phase plan → `task`; a
   compact plan → `implement`.

## Artifact Requirements

- Improved `./workflow/features/{slug}/plan.md`: the same structure,
  language, and mode it had, with only the content of the fixes changed.
- Optional `./workflow/PLAN.md` update: service status and tails only.
- Chat report: a short list of edits and the next-step recommendation. No
  report file is created.

## Updating PLAN.md

The feature keeps `[-]` planned: this skill verifies the plan; it does not
split, implement, test, or archive. Status markers: `[ ]` new, `[-]`
planned, `[+]` split into tasks, `[x]` implemented, `[*]` tested, `[/]`
archived. Change nothing beyond this feature's tails.

## Notes

- A missing `design.md` or canon file does not stop the skill; verify
  against what exists and do not invent constraints.
- When local code contradicts the plan, trust the verified codebase for
  what exists and fix the plan so implementation is not misled.
- If the user actually wants splitting, implementation, tests, or docs,
  name the matching skill instead of doing that work here.
