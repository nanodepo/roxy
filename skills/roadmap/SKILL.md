---
name: roadmap
description: >
  Creates/updates project vision and roadmap canon in ./workflow/VISION.md and
  ./workflow/ROADMAP.md. Triggers: create roadmap, update roadmap, define
  project vision, set product direction, prioritize goals.
---

# Roadmap

## Purpose

Record current project meaning, goals, priorities, direction as canon.

Creates/updates:

- `./workflow/VISION.md`
- `./workflow/ROADMAP.md`
- roadmap service tails in `./workflow/PLAN.md` only for unresolved
  project-level direction follow-up.

Use after `initialize`, before feature-level work. Later stages read
`VISION.md` / `ROADMAP.md` for purpose, audience, priorities, constraints,
vocab.

Does not write `PROJECT.md`, choose architecture, create `feature.md`, write
PRDs, plan feature impl, create backlog/sprint/release plans, write code/tests,
or perform git ops.

## Params

Use `args` as optional roadmap intent:

```txt
<roadmap intent, goal, priority change, or product direction>
```

- Args present -> user's current roadmap request.
- Args empty -> elicit user's current project stage, goals, horizon, and
  direction before using workflow files as evidence and constraints.
- Request/context too vague -> ask open-ended intake questions in Step 2 before
  local validation.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout, worktree.
- Own only `./workflow/VISION.md`, `./workflow/ROADMAP.md`, roadmap service
  tails in `./workflow/PLAN.md`.
- Do not create/modify feature folders, `feature.md`, `plan.md`, `design.md`,
  `tests.md`, `NN-task.md`, source code, architecture files, design guideline
  files, or tech project files.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 5. Direction
  work = analytical work.
- Write `VISION.md` / `ROADMAP.md` as current canon. No change history,
  decision biography, temp notes, or "previously/now" comparisons.
- Keep roadmap outcome-focused. Items explain result, rationale, sequence,
  constraints, confidence; not feature list only.
- Separate facts from assumptions. Do not present guesses as facts.
- Treat developer-stated project stage, business goals, product direction,
  horizon, and priority as the primary direction input. Use workflow files,
  docs, and code as evidence, constraints, and contradiction checks. Do not
  override user-stated direction from local inference without asking.
- Use simplest prioritization lens that explains decision. Prefer light lens
  over formal scoring unless local canon already uses stronger method.
- Ask only blocking direction questions: users, goals, horizon, constraints,
  priority, success signals. Do not ask impl, architecture, UI, test, or docs
  questions.
- If tests appear as future roadmap tails, phrase as live project invariants,
  not incidents or removed behavior.

## Direction Canon Discipline

Treat `VISION.md` and `ROADMAP.md` as current direction canon, not an idea
archive or priority history.

- Keep only statements that guide a current product choice: audience, problem,
  scope, priority, sequencing, constraint, success signal, or non-goal.
- Replace superseded goals, assumptions, and priorities with the current
  direction. Do not preserve old direction as context.
- `Not Now` is an active boundary, not a parking lot. Keep an item there only
  when it prevents wrong near-term choices.
- Label assumptions only when they affect current prioritization or risk. Remove
  assumptions that no longer guide decisions.
- Existing roadmap docs are claims to reconcile. Current user direction is the
  leading strategic input; current product evidence confirms, constrains, or
  challenges it. Resolve contradictions before replacing canon.

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

Use task planning mode for this multi-step flow. Close items one by one.

### 1. Read minimal project ctx

Read if exists:

- `./workflow/PROJECT.md`: stack, run, deploy, project shape.
- `./workflow/VISION.md`: current meaning.
- `./workflow/ROADMAP.md`: current goals/priorities.
- `./workflow/PLAN.md`: workflow status + service tails.
- `./workflow/ARCHITECTURE.md` / `./workflow/DESIGN.md`: only when they affect
  product constraints/direction.

Missing file -> continue. Do not create unrelated workflow files to compensate.
Treat existing `VISION.md` and `ROADMAP.md` as current-canon candidates, not
history to preserve automatically.

Use this first read to detect language, existing canon, and obvious project
shape. Do not synthesize final direction from local files before user direction
intake unless the user's request already provides enough current direction.

### 2. Elicit user direction first

If `args` / current user message does not already answer the current direction,
ask concise open-ended intake questions before code or docs validation.

Cover only blocking roadmap inputs:

- current project stage;
- near-term and longer-term direction;
- business / product goals;
- target users or stakeholders;
- planning horizon;
- current constraints;
- success signals;
- explicit `Not Now`.

Ask in the user's language. Do not offer preselected options in this first
intake round. The developer knows project intent better than code can infer it.

If user input already covers these points, proceed without interruption and
treat the provided direction as the primary input.

### 3. Inspect only needed local ctx

Inspect code/docs/feature briefs after user direction intake, and only to
validate, constrain, or challenge the stated direction.

Keep narrow:

- prefer `rg --files`, top-level dir reads, focused reads;
- avoid broad code analysis unless roadmap request depends on actual product
  state;
- use impl details as constraints/evidence, not roadmap output.
- use changelogs, old tickets, and archived feature notes only when they reveal
  an active constraint or decision risk.
- when local evidence conflicts with user-stated direction, record the conflict
  for clarification instead of silently choosing the local inference.

### 4. Extract direction inputs

Identify:

- user-stated project stage, goals, horizon, direction, constraints, and
  success signals;
- local evidence that confirms the user-stated direction;
- local evidence that conflicts with, constrains, or is missing for the
  user-stated direction;
- purpose + target users;
- problem space + user/project outcome to improve;
- positioning + differentiation;
- current capabilities + constraints;
- known goals, priorities, deps, risks;
- explicit user intent from `args` / current msg;
- confirmed facts, assumptions, low-confidence guesses;
- `Not Now` items: out of scope, deferred, not aligned.
- stale goals, assumptions, or `Not Now` items to remove because they no longer
  guide current direction.

Think goal-backward: future state first, then priorities that support it.

### 5. Synthesize with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking`. Reason through:

- the user-stated project stage, intended direction, goals, and horizon;
- what local evidence confirms;
- what local evidence contradicts, constrains, or fails to show;
- what project exists to do;
- who it serves / does not serve;
- most important problem/need;
- guiding principles for future choices;
- outcomes vs outputs;
- known/missing success signals;
- sequencing across `Now`, `Next`, `Later`;
- value, strategic fit, effort, risk, deps, confidence effects on priority;
- confirmed decisions vs assumptions;
- explicit `Not Now`;
- what current statements to keep, rewrite, or delete;
- unresolved project-level decisions needing roadmap tails in
  `./workflow/PLAN.md`;
- truly blocking questions before canon write.

Do not turn reasoning into PRD, backlog, sprint plan, release plan, feature
brief, or architecture decision.

### 6. Ask reconciliation questions only

Before asking user, answer open points from the intake answers, `./workflow/`,
local docs, product code, and current request.

Ask at most three concise questions in one block, only when answer changes
purpose, target users, problem framing, priority, horizon, constraints, or
success signals.

Use preselected options only in this reconciliation step, after local
validation. For each option question: recommended option first with
`(Recommended)` + short reason. Offer only materially different options.

Non-blocking question -> write as assumption or roadmap tail, not interruption.

### 7. Write `./workflow/VISION.md`

Create/update using Artifact Requirements.

Write current vision only. Replace stale content; do not preserve as history.
Keep the vision stable and compact: reader, problem, value, principles, success,
constraints, non-goals.

### 8. Write `./workflow/ROADMAP.md`

Create/update using Artifact Requirements.

Use outcome-oriented items. Each active item explains outcome/rationale, not
only feature/task name.
Remove answered assumptions, stale priorities, and inactive `Not Now` entries.

### 9. Update roadmap tails in `./workflow/PLAN.md`

Append/refresh only roadmap service tails: unresolved project-level questions
about vision, goals, priority, sequencing, success signals.
If a roadmap tail is answered by the new canon, do not keep or refresh it as
history. If preserving existing PLAN structure prevents deletion, leave it
untouched and do not duplicate it.

Do not add feature entries, change feature statuses, or modify non-roadmap
service tails.

If `./workflow/PLAN.md` exists, preserve structure and add roadmap tails in
most relevant existing service section. If no suitable section, add:

```md
## Roadmap Tails

- [ ] <roadmap-level decision needed>
  - source: `./workflow/ROADMAP.md`
```

If `./workflow/PLAN.md` missing, create it only when roadmap tails exist. Keep
limited to `Roadmap Tails` section.

### 10. Quality checklist

Verify:

- `VISION.md` and `ROADMAP.md` need no chat transcript.
- Both docs describe current project state, not creation history.
- User-stated project stage, direction, goals, horizon, constraints, and success
  signals are reflected or intentionally marked as assumptions / tails.
- Local evidence is used as validation, constraint, or contradiction check, not
  as a replacement for user-stated business direction.
- Any conflict between user direction and local evidence is resolved by
  clarification, represented as a risk, or recorded as a roadmap tail.
- `VISION.md` answers: users, problem, why matters, positioning, principles,
  success.
- `ROADMAP.md` explains direction, outcomes, sequencing, rationale, deps, risks,
  confidence, assumptions, `Not Now`.
- Priorities align with vision + constraints.
- Assumptions / low-confidence items labeled.
- Roadmap does not create feature briefs, impl plans, or backlog items.
- `./workflow/PLAN.md` contains only roadmap tails from this skill.
- Every kept item guides a current choice; stale ideas and answered questions
  are absent from owned artifacts.

### 11. Report

Report briefly:

- paths written;
- main vision/roadmap decisions;
- assumptions / low-confidence areas;
- roadmap tails added to `./workflow/PLAN.md`, if any;
- next recommended workflow stage.

## Artifact Requirements

Create `./workflow/VISION.md` as self-contained artifact per Language Notice.

Use semantic structure. Translate visible headings, field labels, table headers,
placeholders, examples:

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

Create `./workflow/ROADMAP.md` as self-contained artifact per Language Notice.

Use semantic structure. Translate visible headings, field labels, table headers,
placeholders, examples:

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
```

## Artifact Compression Rules

Keep artifacts complete but tight:

- current rules + direction, not discovery notes;
- short declarative sections over long explanations;
- examples only when they clarify canon;
- no duplicated points between `VISION.md` and `ROADMAP.md`;
- unresolved project-level direction in roadmap tails, not prose disclaimers;
- `Not Now` contains active boundaries only, not rejected-idea history;
- omit empty sections only when truly irrelevant.

## Updating PLAN.md

At end, touch `./workflow/PLAN.md` only for roadmap service tails from Step 9.
Do not create/update feature entries or change feature statuses.

Use `[ ]` for unresolved roadmap tails. Do not use feature status markers to
imply roadmap lifecycle.

## Notes

- If `initialize` has not run and `./workflow/PROJECT.md` missing, proceed only
  if direction can still be inferred from user answers + local ctx. Else ask
  user to initialize first or answer blocking ctx questions.
- Existing `VISION.md` / `ROADMAP.md` with current rules mixed stale notes:
  preserve current rules, remove stale notes.
- Local evidence conflicts with current user direction -> ask focused
  clarification before canon write.
- Roadmap = feasibility check + strategic narrative, not delivery-date promise.
- Follow steps literally; do not shorten them.
