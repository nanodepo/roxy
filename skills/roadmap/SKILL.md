---
name: roadmap
description: >
  Creates or updates the project's vision and roadmap canon in
  ./workflow/VISION.md and ./workflow/ROADMAP.md. Use for "create roadmap",
  "update roadmap", "define project vision", "set product direction", or
  "prioritize goals".
---

# Roadmap

## Purpose

`roadmap` records the project's meaning, goals, priorities, and direction as
current project canon.

It creates or updates:

- `./workflow/VISION.md`
- `./workflow/ROADMAP.md`
- roadmap service tails in `./workflow/PLAN.md`, only when unresolved
  project-level direction needs follow-up.

Use this skill after `initialize` and before feature-level work. Later stages
read `VISION.md` and `ROADMAP.md` so they can use the project's purpose,
audience, priorities, constraints, and vocabulary without deriving them again.

This skill does not write `PROJECT.md`, choose architecture, create
`feature.md`, write PRDs, plan concrete feature implementation, create
backlogs, write sprint or release plans, write product code, write tests, or
perform git operations.

## Parameters

Use `args` as optional roadmap intent:

```txt
<roadmap intent, goal, priority change, or product direction>
```

- If `args` is present, treat it as the user's current roadmap request.
- If `args` is empty, infer the request from the current user message and
  available workflow files.
- If the request and local context are too vague to identify project direction,
  ask focused clarification questions in Step 5.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs,
  branches, commits, pushes, checkout commands, or worktree commands.
- Own only `./workflow/VISION.md`, `./workflow/ROADMAP.md`, and roadmap service
  tails in `./workflow/PLAN.md`.
- Do not create or modify feature folders, `feature.md`, `plan.md`,
  `design.md`, `tests.md`, `NN-task.md`, source code, architecture files,
  design guideline files, or technical project files.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4. Creating
  or updating project direction is analytical work.
- Write `VISION.md` and `ROADMAP.md` as current canon. Do not include change
  history, decision biography, temporary notes, or "previously / now"
  comparisons.
- Keep roadmap work outcome-focused. Roadmap items explain the result,
  rationale, sequencing, constraints, and confidence; they are not just a list
  of features.
- Separate confirmed knowledge from assumptions. Do not present guesses as
  facts.
- Use the simplest prioritization method that explains the decision. Prefer a
  light lens over formal scoring unless the project already uses a stronger
  local convention.
- Ask only blocking questions about direction, users, goals, horizon,
  constraints, priorities, and success signals. Do not ask implementation,
  architecture, UI design, testing, or documentation questions.
- If tests appear as future work in roadmap tails, phrase them around live
  project invariants, not around incidents or removed behavior.

## Steps

For this multi-step procedure, use the agent's task planning mode (todo list /
task plan, whichever is available) and close items one by one.

### 1. Read project context

Read these files if they exist:

- `./workflow/PROJECT.md` for stack, run, deploy, and project shape.
- `./workflow/VISION.md` for current project meaning.
- `./workflow/ROADMAP.md` for current goals and priorities.
- `./workflow/PLAN.md` for workflow status and existing service tails.
- `./workflow/ARCHITECTURE.md` and `./workflow/DESIGN.md` only when they affect
  product constraints or direction.

If a file is missing, continue with the remaining context. Do not create
unrelated workflow files to compensate.

### 2. Inspect only necessary local context

Inspect code, documentation, or existing feature briefs only when workflow
files do not explain the product, audience, current capabilities, constraints,
or active priorities.

Keep the scan narrow:

- prefer `rg --files`, top-level directory reads, and focused file reads;
- avoid broad code analysis unless the roadmap request depends on actual
  product state;
- treat implementation details as constraints or evidence, not as roadmap
  output.

### 3. Extract the current direction inputs

Identify:

- project purpose and target users;
- the problem space and user or project outcome the work should improve;
- positioning and differentiation;
- current capabilities and constraints;
- known goals, priorities, dependencies, and risks;
- explicit user intent from `args` or the current message;
- confirmed facts, assumptions, and low-confidence guesses;
- items that belong in `Not Now` because they are out of scope, deferred, or
  not aligned with the current direction.

Use goal-backward thinking: start from what should become true for the project,
then determine which priorities support that future state.

### 4. Synthesize with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking` and reason through:

- what the project exists to do;
- who the project serves and who it does not serve;
- which problem or need matters most;
- which principles should guide future choices;
- which goals are outcomes and which are merely outputs;
- which success signals are known and which are missing;
- how to sequence goals across `Now`, `Next`, and `Later`;
- how value, strategic fit, effort, risk, dependencies, and confidence affect
  priority;
- which decisions are confirmed and which are assumptions;
- which items are explicitly `Not Now`;
- which unresolved project-level decisions require roadmap tails in
  `./workflow/PLAN.md`;
- which questions are truly blocking before writing the canon.

Do not turn this reasoning into a PRD, backlog, sprint plan, release plan,
feature brief, or architecture decision.

### 5. Ask blocking questions only

Before asking the user, try to answer every open point from `./workflow/`
files, local documentation, product code, and the current request.

Ask at most three concise questions in one block, only when the answer changes
project purpose, target users, problem framing, priority, horizon, constraints,
or success signals.

For each question, put the recommended option first with `(Recommended)` and a
short reason. Offer only materially different options.

If a question is not blocking, write it as an assumption or roadmap tail rather
than interrupting the user.

### 6. Write `./workflow/VISION.md`

Create or update `./workflow/VISION.md` using the structure in Artifact
Requirements.

Write the current project vision only. Replace stale content instead of
preserving it as history.

### 7. Write `./workflow/ROADMAP.md`

Create or update `./workflow/ROADMAP.md` using the structure in Artifact
Requirements.

Use outcome-oriented roadmap items. Each active item should explain the outcome
or rationale, not only the feature or task name.

### 8. Update roadmap tails in `./workflow/PLAN.md`

Append or refresh only roadmap service tails: unresolved project-level
questions about vision, goals, priority, sequencing, or success signals.

Do not add feature entries, do not change feature statuses, and do not modify
non-roadmap service tails.

If `./workflow/PLAN.md` exists, preserve its structure and add roadmap tails in
the most relevant existing service section. If no suitable section exists, add:

```md
## Roadmap Tails

- [ ] <roadmap-level decision needed>
  - source: `./workflow/ROADMAP.md`
```

If `./workflow/PLAN.md` is missing, create it only when there are roadmap tails
to record. Keep it limited to the `Roadmap Tails` section above.

### 9. Run the quality checklist

Verify:

- `VISION.md` and `ROADMAP.md` can be read without the chat transcript.
- Both documents describe the current project state, not its creation history.
- `VISION.md` answers who the project serves, what problem it solves, why it
  matters, how it is positioned, what principles guide it, and what success
  looks like.
- `ROADMAP.md` explains strategic direction, outcomes, sequencing, rationale,
  dependencies, risks, confidence, assumptions, and `Not Now`.
- Priorities align with the vision and known constraints.
- Assumptions and low-confidence items are labeled as such.
- The roadmap does not create feature briefs, implementation plans, or backlog
  items.
- `./workflow/PLAN.md` contains only roadmap tails from this skill.

### 10. Report

Give a short report with:

- paths written;
- the main vision and roadmap decisions recorded;
- assumptions or low-confidence areas;
- roadmap tails added to `./workflow/PLAN.md`, if any;
- next recommended workflow stage.

## Artifact Requirements

Write `./workflow/VISION.md` in the user's working language unless surrounding
workflow files clearly use another language.

Use this structure:

```md
# VISION.md

## Purpose

<Why the project exists.>

## Users

- Primary audience:
- Secondary audience:
- Not the target audience:

## Problem

- Main problem:
- Why it matters:
- What changes for the user:

## Positioning

- What the project is:
- What the project is not:
- How it differs from alternatives:

## Principles

- <Principle that guides product choices>

## Success Signals

- User outcome:
- Project outcome:
- Qualitative or quantitative signal:

## Constraints

- Technical:
- Product:
- Resource:

## Non-Goals

- <What the project intentionally does not do>
```

Write `./workflow/ROADMAP.md` in the user's working language unless surrounding
workflow files clearly use another language.

Use this structure:

```md
# ROADMAP.md

## Direction

<Current strategic focus.>

## Goals

### Goal 1: <Name>

Outcome: Enable <user or segment> to <desired outcome> so that <project impact>.

Success signals:

- <Signal>

Confidence:

- High / Medium / Low

Assumptions:

- <Assumption or condition>

## Horizons

### Now

- [ ] <Direction or goal>
  - Outcome:
  - Rationale:
  - Dependencies:
  - Risk:
  - Confidence:

### Next

- [ ] <Direction or goal>
  - Outcome:
  - Rationale:
  - Dependencies:
  - Risk:
  - Confidence:

### Later

- <Direction or goal without a time promise>

## Not Now

- <Deferred or out-of-scope item>
  - Reason:

## Risks and Trade-Offs

- <Risk> -> <mitigation or decision needed>

## Roadmap Tails

- <Project-level direction question or decision, if any>
```

## Artifact Compression Rules

Keep the artifacts complete but tight:

- Write current rules and current direction, not discovery notes.
- Prefer short declarative sections over long explanations.
- Keep examples only when they clarify the canon.
- Remove duplicated points between `VISION.md` and `ROADMAP.md`.
- Put unresolved project-level direction in roadmap tails, not in prose
  disclaimers.
- Omit empty sections only when they truly do not apply.

## Updating PLAN.md

At the end, touch `./workflow/PLAN.md` only for roadmap service tails from Step
8. Do not create or update feature entries and do not change feature statuses.

Use status `[ ]` for unresolved roadmap tails. Do not use feature status markers
to imply a roadmap lifecycle.

## Notes

- If `initialize` has not run and `./workflow/PROJECT.md` is missing, proceed
  only if the project direction can still be inferred from local context and
  user answers. Otherwise ask the user to initialize the project first or answer
  the blocking context questions.
- If existing `VISION.md` or `ROADMAP.md` contains useful current rules mixed
  with stale notes, preserve the current rules and remove the stale notes.
- If local evidence conflicts with the user's current direction, ask a focused
  clarification before writing canon.
- Treat the roadmap as a feasibility check and strategic narrative, not a
  promise of delivery dates.
- The skill follows these steps literally and does not shorten them.
