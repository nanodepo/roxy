---
name: planning
description: >
  Turns a feature brief into ./workflow/features/{slug}/plan.md and marks the
  feature planned in ./workflow/PLAN.md. Use for work too large to implement
  in one pass. Triggers: plan feature, create plan, write plan.md, turn
  feature into a plan.
---

# Planning — Turn a Feature Brief into an Implementation Plan

## Purpose

This skill turns `./workflow/features/{slug}/feature.md` into an
implementation plan at `./workflow/features/{slug}/plan.md` and marks the
feature `[-]` in `./workflow/PLAN.md`.

A plan exists for work an agent cannot reliably finish in one pass: it splits
the process into phases that can each be executed and verified on their own.
The plan directs a competent implementer rather than dictating keystrokes —
checkboxes name outcomes, not micro-steps. A simple feature gets a flat list
of steps instead of phases, and a feature simple enough to implement straight
from the brief gets a recommendation to skip planning, not an inflated plan.

The plan complements the brief, never restates it: `feature.md` owns the
product intent and boundaries; `plan.md` adds the chosen approach, verified
codebase context, non-trivial decisions, and the work structure. The plan
does not own test plans, UI design, task files, or code — those belong to
`test`, `design`, `task`, and `implement`.

## Parameters

`args` names the feature:

```txt
<feature-slug>    # directory under ./workflow/features/
```

- `args` is empty → infer the slug from the user's message and the active
  entries in `./workflow/PLAN.md`, but only when exactly one feature with a
  `feature.md` matches the request. Otherwise stop and ask one short
  question.
- `./workflow/features/{slug}/feature.md` is missing → stop and report it.
  Do not create a brief; that is the `feature` skill's work.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty working tree is
  expected.
- Own only `./workflow/features/{slug}/plan.md` and the matching status line
  in `./workflow/PLAN.md`. Do not write `tests.md`, `design.md`,
  `NN-task.md`, code, or documentation.
- Read product code to plan; never modify it.
- Name only verified paths, modules, and dependencies. Anything the plan
  asserts about the codebase must have been checked, not assumed.
- Ask all open questions in one batch before writing the plan, with the
  recommended option first marked "(Recommended)". The written plan contains
  no open question the user could have answered in that batch.
- Write the plan in the present tense as the current intended state — no
  chat biography, no "previously/now" wording, no record of rejected
  alternatives.
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
   `./workflow/PLAN.md` into one `{slug}` as described in Parameters. If
   `plan.md` already exists, treat the run as a rebuild from current inputs,
   not an append to old text.

2. **Read the brief and the project canon.** Read
   `./workflow/features/{slug}/feature.md` (the scope authority),
   `design.md` in the same directory if it exists, `./workflow/PROJECT.md`,
   `./workflow/ARCHITECTURE.md`, `./workflow/DESIGN.md` when the feature
   touches UI, and `./workflow/PLAN.md`. Canon files are constraints, not
   permission for unrelated work.

3. **Verify against the codebase.** Inspect as deeply as a realistic plan
   needs: the modules and local patterns the work builds on, what already
   exists versus what must be created, the contracts shared with the rest of
   the system, and the architectural boundaries the plan must not cross.

4. **Shape the plan.** Decide the approach and the decisions worth
   recording, then structure the work. Use phases only when the work
   genuinely does not fit one implementation pass; prefer vertical slices
   that each end in verifiable working behavior. A simple feature gets a
   flat step list. If the brief alone is enough to implement, do not write a
   hollow plan — report that and recommend `implement` directly.

5. **Ask the user.** Gather every question that survived the brief, the
   canon, and the codebase, and ask them in one batch per Strict Rules. Fold
   the answers into the plan; non-blocking leftovers become tails.

6. **Write `plan.md`** per Artifact Requirements.

7. **Update `./workflow/PLAN.md`.** Set this feature's line to `[-]`,
   pointing at `./workflow/features/{slug}/plan.md` unless the file uses a
   stronger local format. Touch nothing else.

8. **Report and recommend.** Summarize the plan in a few chat lines, then
   recommend the next step from the actual result: a compact plan →
   `implement` directly; the feature touches UI with non-trivial UX →
   `design` first; the feature is complex or architecturally significant →
   `improve` as a verification pass, ideally by another model; a large
   multi-phase plan → `task`.

## Artifact Requirements

Write `./workflow/features/{slug}/plan.md` from this core, following the
Language Notice. Add a section only when it carries a decision; omit any
that would hold boilerplate:

```md
# plan.md

## Goal

The chosen implementation approach in a few sentences. Refer to feature.md
for product intent instead of restating it.

## Context

Verified files, modules, and local patterns the work builds on; the
constraints from architecture and stack that bound the approach.

## Decisions

Non-trivial technical decisions and the reasons that constrain
implementation, including shared contracts between parts of the work.

## Phases

### Phase 1: <name>

- [ ] Outcome the implementer can check
- [ ] Outcome the implementer can check

## Tails

Discovered work that is out of scope; hand-offs for design, task, test,
docs, or the user.
```

- For a feature without real phases, replace `## Phases` with a flat
  `## Steps` checkbox list.
- Plan size is proportional to feature complexity: a few sentences of goal
  plus a short step list is a complete plan for a simple feature.
- The plan stands alone: the next stage works from `feature.md` plus
  `plan.md` with no chat retelling.

## Updating PLAN.md

Set the matching feature to `[-]` planned. Status markers: `[ ]` new, `[-]`
planned, `[+]` split into tasks, `[x]` implemented, `[*]` tested, `[/]`
archived. Change only this feature's line; preserve every other entry and
status.

## Notes

- A missing `PROJECT.md`, `ARCHITECTURE.md`, or `DESIGN.md` does not stop
  the skill; plan from what exists and do not invent constraints.
- When the codebase contradicts `feature.md`, trust the verified codebase
  for what exists, plan around it, and record the conflict as a tail.
- `improve` may later verify this plan and will preserve the structure
  written here — keep that structure honest rather than ornate.
