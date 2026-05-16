---
name: feature
description: >
  Creates a self-contained feature brief from a raw request in
  ./workflow/features/{slug}/feature.md and registers it in ./workflow/PLAN.md.
  Use for "new feature", "capture feature", "write feature brief", or "start
  feature".
---

# Feature

## Purpose

Turn a raw user request into a self-contained feature brief at
`./workflow/features/{slug}/feature.md` and register the feature in
`./workflow/PLAN.md` with status `[ ]`.

Use this skill as the first feature workflow stage. It captures what the user
asks for, why it matters, which context is known, what belongs in scope, what is
out of scope, and which questions remain open for later stages.

The brief is richer than a one-line idea and smaller than a PRD or plan. It
does not decide implementation, architecture, UI design, tests, documentation,
or task breakdown.

## Parameters

Use `args` as the raw feature request:

```txt
<feature request>
```

- If `args` is present, treat the full string as the feature request.
- If `args` is empty, infer the request from the current user message.
- If the request is too vague to identify the feature essence, ask one short
  clarification before writing files.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs,
  branches, commits, pushes, checkout commands, or worktree commands.
- Own only `./workflow/features/{slug}/feature.md`, the feature directory
  `./workflow/features/{slug}/`, and the matching entry in
  `./workflow/PLAN.md`.
- Do not write `plan.md`, `design.md`, `tests.md`, `NN-task.md`, product code,
  PRDs, acceptance criteria, or implementation tasks.
- Do not call downstream skills automatically. Stop after the brief and
  `PLAN.md` entry are ready.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4 before
  asking final questions or writing the brief. Creating a feature brief from a
  raw request is analytical work.
- Ask only blocking clarifying questions. A question is blocking only when its
  answer changes the feature essence, problem or need, user or context,
  expected result, or scope boundary.
- Do not ask implementation, architecture, UI design, test, or documentation
  questions. Record those as open questions only when they affect later
  planning.
- Write current feature intent only. Do not include conversation biography,
  previous states, "now/previously" comparisons, migration notes, or removed
  behavior.
- If tests are mentioned as later work, phrase the need as a live product
  invariant to protect, not as an incident or deletion check.

## Steps

For this multi-step procedure, use the agent's task planning mode (todo list /
task plan, whichever is available) and close items one by one.

### 1. Capture the raw request

Read the feature request from `args` or the current user message.

Extract:

- the requested work type: problem, solution, improvement, fix, refactor, or
  unclear
- the main capability or change
- domain terms and named objects
- the user, actor, team, or usage context when present
- the expected result in the user's words
- explicit limits, exclusions, urgency, or constraints

If the request cannot identify a feature at all, ask one concise clarification
question and stop until answered.

### 2. Read only needed project context

Read whichever of these exist and help interpret the feature:

- `./workflow/PROJECT.md` — stack, run, deploy, and project shape.
- `./workflow/VISION.md` — product direction and vocabulary.
- `./workflow/ROADMAP.md` — current goals and priorities.
- `./workflow/DESIGN.md` — only when the request explicitly touches UI or user
  interaction.
- `./workflow/PLAN.md` — existing feature list and workflow status.

Use `rg --files`, `find`, and direct reads when you need to inspect existing
feature folders under `./workflow/features/`.

Keep context narrow. Do not scan product code unless the feature request names
a concrete code area and the workflow files are insufficient to understand the
feature meaning.

### 3. Check for duplicates and related active features

Read existing feature entries in `./workflow/PLAN.md` and folder names under
`./workflow/features/`.

If an active feature appears to cover the same request:

- Read that feature's `feature.md` when it exists.
- If the new request is a duplicate, stop and report the existing slug.
- If the request is a distinct extension, continue and record the relationship
  in the new brief.

Do not archive, merge, rename, or change existing feature folders during this
skill.

### 4. Synthesize with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking` and reason through:

- what the user is asking for in one current-state sentence
- whether the request is primarily a problem, solution, improvement, fix,
  refactor, or mixed request
- the problem or need behind the request, using project context only when it is
  grounded in local files
- the user, actor, team, system, or context affected
- the expected result without turning it into acceptance criteria
- hidden assumptions about the user, problem, or result
- obvious edge cases and exceptional situations that planning should not miss
- what belongs in the feature scope
- what adjacent work must be listed as non-goals to prevent scope creep
- which questions are truly blocking and which belong in open questions
- whether the request duplicates or relates to an active feature
- a stable `kebab-case` slug that names the feature by domain intent, not by
  implementation detail

Use reference ideas as filters:

- Classify each possible clarification before asking it. Ask only if the answer
  changes the brief's essence, problem, user/context, expected result, or
  boundaries.
- Pressure-test assumptions. If an assumption is important and unsupported,
  ask about it when blocking; otherwise record it as an open question.
- Record obvious edge cases and exceptions only as boundaries or open
  questions. Do not define expected behavior for them in this stage.
- Make scoping explicit. A non-goal is an adjacent piece of work deliberately
  kept outside this feature.

### 5. Ask blocking questions only

Try to answer open points from the request, workflow files, and existing feature
briefs before asking the user.

Ask at most three concise questions in one block. For each question:

- Put the recommended answer first with `(Recommended)` and a short reason.
- Offer only materially different options.
- Do not ask about implementation, architecture, design, tests, docs, or task
  sequencing.

If a non-blocking detail is unclear, write it under `Open Questions` in
`feature.md` instead of interrupting the user.

### 6. Create the feature directory and brief

Choose the final slug:

- Use lowercase Latin letters, numbers, and hyphens only.
- Prefer 2-5 words.
- Start and end with a letter or number.
- Avoid implementation technology unless the feature is specifically about
  that technology.
- If the slug already exists, add one meaningful qualifier instead of a numeric
  suffix when possible.

Create `./workflow/features/{slug}/` and write
`./workflow/features/{slug}/feature.md` using the Artifact Requirements below.

### 7. Update `./workflow/PLAN.md`

Add one service line for the feature with status `[ ]`.

If `./workflow/PLAN.md` exists, preserve its structure and status markers. Add
the new feature near the active feature list, or append it at the end when no
clear section exists.

If `./workflow/PLAN.md` is missing, create `./workflow/PLAN.md` with a minimal
feature list that includes the new entry. Do not create unrelated workflow
canon files.

Use this entry shape when the file has no stronger local pattern:

```md
- [ ] `{slug}` - <short feature title>
  - brief: `./workflow/features/{slug}/feature.md`
```

## Artifact Requirements

Write `./workflow/features/{slug}/feature.md` in the user's working language
unless the surrounding workflow files clearly use another language.

Use this structure:

```md
# Feature: <feature name>

## Summary

<1-3 sentences describing the current feature intent.>

## Request Type

- Type:
- Why:

## Problem or Need

- Problem:
- Evidence or context:

## User or Context

- User / actor:
- Situation:

## Expected Result

- Result:
- Success signal:

## Scope

- In:
- Related existing feature:

## Non-Goals

- Out:
- Reason:

## Terms

- Term:
- Meaning:

## Edge Cases and Exceptions

- Case:
- Why it matters:

## Open Questions

- Question:
- Impact:

## Source Context

- User request:
- Project files read:
```

Adapt the template to the request's complexity:

- Keep simple features short.
- Omit empty optional bullets when they add no information.
- Keep `Open Questions` explicit. Mark whether each question blocks planning or
  can be resolved later.
- Do not include acceptance criteria, EARS statements, user stories as a
  required format, implementation tasks, design decisions, test plans, or code
  snippets.

`feature.md` is ready when a later `planning` agent can understand the feature
without conversation history:

- the request essence is clear
- the problem or need is stated
- the user or context is identified when known
- the expected result is described without over-design
- scope and non-goals are explicit
- relevant terms are defined
- obvious edge cases are visible
- open questions are named with their impact

## Updating PLAN.md

At the end, ensure `./workflow/PLAN.md` contains one `[ ]` entry for the new
feature and points to `./workflow/features/{slug}/feature.md`.

Do not change statuses for other features. Do not rewrite old feature entries
except to avoid adding a duplicate line for the same new slug.

## Notes

- Missing `PROJECT.md`, `VISION.md`, `ROADMAP.md`, or `DESIGN.md` does not stop
  this skill. Continue with the request and the available workflow context.
- Missing `./workflow/features/` does not stop this skill. Create the directory
  needed for the new feature.
- If the request is really a plan, design, test, docs, or implementation
  request for an existing feature, report the matching downstream skill and do
  not create a new feature brief.
- If the request combines several unrelated features, ask the user which one to
  capture first. Keep one `feature.md` focused on one coherent feature.
