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

Turn raw request into self-contained brief at
`./workflow/features/{slug}/feature.md`; register in `./workflow/PLAN.md` as
`[ ]`.

First feature workflow stage. Capture ask, why, known ctx, scope, non-goals,
open questions. Richer than idea, smaller than PRD/plan.

Does not decide impl, architecture, UI design, tests, docs, or task breakdown.

## Parameters

Use `args` as raw request:

```txt
<feature request>
```

- Present -> full string = request.
- Empty -> infer from current user msg.
- Too vague to identify feature essence -> ask one short clarification before
  writing files.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout, worktree.
- Own only `./workflow/features/{slug}/feature.md`,
  `./workflow/features/{slug}/`, matching `./workflow/PLAN.md` entry.
- Do not write `plan.md`, `design.md`, `tests.md`, `NN-task.md`, product code,
  PRDs, acceptance criteria, impl tasks.
- Do not call downstream skills. Stop after brief + `PLAN.md` entry.
- Step 4 must call `mcp__sequential-thinking__sequentialthinking`.
- Ask only blocking clarifications: answer changes essence, problem/need,
  user/context, expected result, or scope boundary.
- Do not ask impl/architecture/UI/test/docs questions. Record as open questions
  only when relevant for later planning.
- Current feature intent only. No biography, deltas, migration notes, removed
  behavior.
- Later test mention = live product invariant to protect, not incident/deletion.

## Language Notice

Write this `SKILL.md` in English.

Write user-facing chat output and generated/rewritten artifacts in target
project working language. Detect from `./workflow/`, docs, user msg. If unclear,
use user language.

When editing existing artifact, preserve language unless user asks translation.

Apply artifact language to prose, headings, table headers, labels,
placeholders, examples. Keep paths, cmds, tool names, code identifiers,
framework/package names, status markers, established product terms unchanged.

Do not mix languages in one artifact unless canon already does so or source term
requires it.

## Flow

Use task planning mode. Close items one by one.

### 1. Capture Request

Read request from `args` or current user msg. Extract:

- work type: problem/solution/improvement/fix/refactor/unclear
- main capability/change
- domain terms/named objects
- user/actor/team/context when present
- expected result in user words
- explicit limits/exclusions/urgency/constraints

No identifiable feature -> ask one concise clarification, stop.

### 2. Read Needed Context

Read only helpful files:

- `./workflow/PROJECT.md`
- `./workflow/VISION.md`
- `./workflow/ROADMAP.md`
- `./workflow/DESIGN.md` only for UI/interaction request
- `./workflow/PLAN.md`

Use `rg --files`, `find`, direct reads for `./workflow/features/`.

Keep ctx narrow. Do not scan product code unless request names concrete code
area and workflow files cannot explain feature meaning.

### 3. Check Duplicates

Read `./workflow/PLAN.md` entries + `./workflow/features/` folders.

If active feature may cover request:

- read existing `feature.md`
- duplicate -> stop, report existing slug
- distinct extension -> continue; record relationship

Do not archive/merge/rename/change existing features.

### 4. Synthesize

Call `mcp__sequential-thinking__sequentialthinking`. Decide:

- one current-state sentence for ask
- request type: problem/solution/improvement/fix/refactor/mixed
- problem/need, grounded in local ctx when used
- affected user/actor/team/system/context
- expected result without acceptance criteria
- hidden assumptions
- edge cases planning must notice
- in-scope work
- adjacent non-goals to prevent scope creep
- blocking vs later open questions
- duplicate/related active feature
- stable `kebab-case` slug by domain intent, not impl detail

Filters:

- Classify clarifications before asking; ask only if answer changes essence,
  problem, user/context, result, boundaries.
- Pressure-test assumptions. Blocking unsupported assumption -> ask; otherwise
  record open question.
- Edge cases/exceptions -> boundaries or open questions only. Do not define
  expected behavior here.
- Non-goal = adjacent work deliberately outside feature.

### 5. Ask Blockers Only

Resolve from request, workflow files, existing briefs first.

Ask max 3 concise questions in one block. For each:

- recommended answer first with `(Recommended)` + short reason
- only materially different options
- no impl/architecture/design/test/docs/task sequencing

Non-blocking unclear detail -> `Open Questions` in `feature.md`.

### 6. Create Brief

Choose slug:

- lowercase Latin letters, numbers, hyphens
- 2-5 words
- start/end letter or number
- avoid tech unless feature is about that tech
- existing slug -> add meaningful qualifier, not numeric suffix when possible

Create `./workflow/features/{slug}/`; write `feature.md` via Artifact Req.

### 7. Update `PLAN.md`

Add one service line with `[ ]`.

If `./workflow/PLAN.md` exists, preserve structure/statuses. Add near active
features or append if no clear section. Do not rewrite other entries; avoid
duplicate line for slug.

If missing, create minimal `./workflow/PLAN.md` with new entry only. No other
canon files.

Default entry shape, unless local pattern stronger. Localize visible labels:

```md
- [ ] `{slug}` - <short feature title>
  - brief: `./workflow/features/{slug}/feature.md`
```

## Artifact Req

Create self-contained `./workflow/features/{slug}/feature.md`. Follow Language
Notice.

Use shape below. Translate all visible headings, labels, placeholders, examples:

```md
# Feature: <feature name>

## Summary

<1-3 sentences describing current feature intent.>

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

Adapt:

- Simple feature -> short.
- Omit empty optional bullets.
- `Open Questions`: mark blocks planning vs later.
- No acceptance criteria, EARS, required user-story format, impl tasks, design
  decisions, test plans, code snippets.

Ready when later `planning` can proceed without chat history:

- essence clear
- problem/need stated
- user/context known when available
- expected result not over-designed
- scope/non-goals explicit
- terms defined
- edge cases visible
- open questions named with impact

## Notes

- Missing `PROJECT.md`, `VISION.md`, `ROADMAP.md`, `DESIGN.md` ok. Use
  available ctx.
- Missing `./workflow/features/` ok. Create needed dir.
- Request is plan/design/test/docs/impl for existing feature -> report matching
  downstream skill; do not create new brief.
- Several unrelated features -> ask which one to capture first. One `feature.md`
  = one coherent feature.
