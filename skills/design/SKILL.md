---
name: design
description: >
  Creates feature-level UI/UX design docs in ./workflow/features/{slug}/design.md after planning. Use for "design feature", "create feature design", "write design.md", or "spec UI for feature".
---

# Design

## Purpose

Create/update feature UI/UX doc:
`./workflow/features/{slug}/design.md`.

Use `feature.md`, `plan.md`, `./workflow/DESIGN.md`, existing frontend
patterns. Result must let `implement` build UI without another design pass.

Applies project design canon to one feature. Does not change canon, code, tests,
`plan.md`, or product discovery.

## Parameters

Use `args`:

```txt
<slug>
```

- `<slug>` = folder under `./workflow/features/{slug}/`.
- Empty or >1 token -> list folders under `./workflow/features/`, ask for one
  slug.
- Missing `feature.md` or `plan.md` -> stop, name missing upstream artifact.
- Non-UI feature -> stop, explain `design` only runs for UI-affecting features.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout.
- Do not edit product code, tests, `./workflow/DESIGN.md`, `feature.md`,
  `plan.md`.
- Own only `./workflow/features/{slug}/design.md`, plus service tail in
  `./workflow/PLAN.md` when missing/weak design canon blocks feature design.
- Treat `./workflow/DESIGN.md` as visual/interaction canon. Do not invent local
  design system.
- Reuse existing frontend comps, layouts, tokens, interactions when fit.
- During Step 4 before writing `design.md`, call
  `mcp__sequential-thinking__sequentialthinking`.
- Current UI decisions only. No biography, deltas, migration notes, removed
  behavior.
- Future test notes = current UI invariants to protect, not incidents/deletions.
- Keep `design.md` compact + concrete: enough for impl, no marketing, no huge
  matrices for simple features.

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

### 1. Resolve Feature

Parse `args`; identify `./workflow/features/{slug}/`.

Read:

- `./workflow/features/{slug}/feature.md`
- `./workflow/features/{slug}/plan.md`
- existing `./workflow/features/{slug}/design.md`, if any
- `./workflow/DESIGN.md`, if any
- `./workflow/PROJECT.md`, if stack needed
- `./workflow/ARCHITECTURE.md`, if UI boundaries/code placement needed
- `./workflow/PLAN.md`, if any

If `./workflow/DESIGN.md` missing, continue only enough to identify gap. Do not
replace canon with taste.

### 2. Inspect Frontend Patterns

Read only frontend files needed for UI conventions:

- related routes/pages/views
- shared layouts
- related form/table/list/dialog/nav/feedback/empty-state comps
- theme/token/styling/component-library config

Prefer `rg --files`, `rg`, `find`, direct reads. Do not run build, format,
codegen, package install, mutation cmds.

Record patterns to reuse. If none exist, state that in `design.md`; use
implementation-ready structure, not fake comp names.

### 3. Build Context Map

Extract:

- user + goal
- primary scenario
- alt scenarios
- deps/constraints from `feature.md` + `plan.md`
- applicable `./workflow/DESIGN.md` rules
- reusable frontend patterns
- data sources + dynamic UI content
- permissions/roles/platform/device differences

Pick lowest sufficient fidelity:

- `conceptual`: flow/framing/approach choice needed.
- `low-fi`: screen structure, content priority, states needed.
- `implementation-ready`: comps, layout, responsive behavior, states, content
  rules, constraints needed.

### 4. Synthesize

Call `mcp__sequential-thinking__sequentialthinking`. Decide:

- UI effect real?
- user goal + first obvious interface priority
- happy path + required alt scenarios
- states: default/loading/empty/error/disabled/success/permission as relevant
- layout by content priority: info, primary action, secondary actions, filters,
  forms, results, errors, help
- mobile/desktop behavior
- comp contracts: anatomy, required/optional elems, variants, states, behavior,
  reuse, accessibility
- interaction/feedback: immediate response, progress, inline errors,
  confirmations, interruptions, reduced motion
- content rules: headings, text length, empty/error copy, overflow,
  localization, data constraints
- accessibility: keyboard, focus, labels, screen-reader dynamic feedback,
  color-independent meaning, motion, touch targets
- impl constraints: perf, reuse, data availability, what not to build
- choices with 2-3 viable options needing user confirmation
- whether missing/weak `./workflow/DESIGN.md` blocks feature design

Prefer one recommended solution. Show alternatives only when they materially
affect impl or UX.

### 5. Ask Only Blockers

Resolve questions from local docs/code first. Ask user only when `design.md`
would otherwise require arbitrary invention. Group independent questions. For
each, put recommended option first with `(Recommended)` + short reason.

No mandatory questionnaire when ctx sufficient.

### 6. Write `design.md`

Create/update `./workflow/features/{slug}/design.md` using Artifact Req.

When updating, rewrite as current feature design. Remove stale decisions,
biography, deltas, ops notes.

No code snippets except tiny identifier/component name needed to point to
existing reusable pattern. Artifact describes UI behavior + constraints.

### 7. Update `PLAN.md`

If `./workflow/PLAN.md` exists, preserve feature status marker. Design stage has
no separate marker.

Add/update concise note under feature only when useful:

```md
- design: `./workflow/features/{slug}/design.md`
```

If design canon missing/weak, append service tail naming missing canon + why it
blocks/weakens design. Current-state, actionable, no discovery history.

If `./workflow/PLAN.md` absent, do not create. Mention in final.

## Artifact Req

Create self-contained `./workflow/features/{slug}/design.md`. Follow Language
Notice.

Use semantic shape below. Translate all visible headings, labels, table headers,
placeholders, examples:

```md
# Design: <feature name>

## Context

- User:
- Goal:
- Input documents:
- Applicable rules from `./workflow/DESIGN.md`:
- Reused frontend patterns:

## Fidelity

- Level:
- Reason:

## Scenarios

### Primary Scenario

1. User step.
2. Interface response.
3. Resulting state.

### Alternative Scenarios

- Scenario:
- Behavior:

## Layout

- Content priority:
- Main zones:
- Primary action:
- Secondary actions:
- Mobile behavior:
- Desktop behavior:

## Components

### <Component>

- Purpose:
- Anatomy:
- Variants:
- States:
- Behavior:
- Reuse:
- Accessibility:

## States

| State | What is visible | User actions | Feedback |
| --- | --- | --- | --- |
| Default |  |  |  |
| Loading |  |  |  |
| Empty |  |  |  |
| Error |  |  |  |
| Success |  |  |  |

## Content

- Headings:
- Empty copy:
- Error copy:
- Length and overflow:
- Data source:

## Implementation Constraints

- Accessibility:
- Motion:
- Responsive:
- Performance:
- Do not build:

## Open Questions

- Question:
- Impact:
```

Adapt to complexity:

- Simple feature -> short. Merge sections only when no decision lost.
- No full comp API unless feature introduces/changes reusable comp.
- No pixel-perfect values when `./workflow/DESIGN.md` defines visual precision.
