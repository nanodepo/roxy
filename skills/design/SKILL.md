---
name: design
description: >
  Creates feature-level UI/UX design docs in ./workflow/features/{slug}/design.md after planning. Use for "design feature", "create feature design", "write design.md", or "spec UI for feature".
---

# Design

## Purpose

Create or update the feature-level UI/UX design document at `./workflow/features/{slug}/design.md`.

Use the feature description, the feature plan, the project design canon, and existing frontend patterns to turn a planned UI feature into concrete interface decisions. The result must let `implement` build the UI without repeating the design step.

This skill applies `./workflow/DESIGN.md` to one feature. It does not change the project design canon, write code, write tests, rewrite `plan.md`, or perform product discovery.

## Parameters

Use `args` as:

```txt
<slug>
```

- `<slug>` is the folder name under `./workflow/features/{slug}/`.
- If `args` is empty or has more than one token, list available folders under `./workflow/features/` and ask for one feature slug.
- If `./workflow/features/{slug}/feature.md` or `./workflow/features/{slug}/plan.md` is missing, stop and explain which upstream artifact is missing.
- If the feature does not touch UI, stop and explain that `design` runs only for UI-affecting features.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs, branches, commits, pushes, or checkout commands.
- Do not edit product code, tests, `./workflow/DESIGN.md`, `./workflow/features/{slug}/feature.md`, or `./workflow/features/{slug}/plan.md`.
- Own only `./workflow/features/{slug}/design.md`, plus a service tail in `./workflow/PLAN.md` when the feature is blocked by missing or insufficient project design canon.
- Treat `./workflow/DESIGN.md` as the visual and interaction canon. Do not invent a new design system for one feature.
- Reuse existing frontend components, layout patterns, tokens, and interaction patterns whenever they fit the feature.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4 before writing `design.md`. Feature design is analytical work.
- Write current rules and current UI decisions only. Do not include biography, previous states, "now/previously" comparisons, migration notes, or removed behavior as design requirements.
- If design notes mention future tests, phrase them as current UI invariants to protect, not as incident or deletion checks.
- Keep `design.md` concrete and compact: enough for implementation, no marketing copy, no exhaustive matrices for simple features.

## Steps

For this multi-step procedure, use the agent's task planning mode (todo list / task plan, whichever is available) and close items one by one.

### 1. Resolve the feature

Parse `args` and identify `./workflow/features/{slug}/`.

Read:

- `./workflow/features/{slug}/feature.md`
- `./workflow/features/{slug}/plan.md`
- `./workflow/features/{slug}/design.md`, if it already exists
- `./workflow/DESIGN.md`, if it exists
- `./workflow/PROJECT.md`, if it helps identify the frontend stack
- `./workflow/ARCHITECTURE.md`, if it helps identify UI boundaries or code placement
- `./workflow/PLAN.md`, if it exists

If `./workflow/DESIGN.md` is missing, continue only far enough to identify the gap. Do not replace the project canon with local taste.

### 2. Inspect existing frontend patterns

Read only the frontend files needed to understand current UI conventions:

- app routes, pages, or views related to the feature
- shared layout components
- form, table, list, dialog, navigation, feedback, and empty-state components related to the feature
- theme, token, styling, or component-library configuration

Prefer `rg --files`, `rg`, `find`, and direct file reads. Do not run build, format, codegen, package installation, or mutation commands.

Record which existing patterns the feature should reuse. If no relevant frontend exists, state that in `design.md` and keep decisions at implementation-ready structure rather than fake component names.

### 3. Build the feature context map

Extract:

- user and user goal
- primary scenario
- alternative scenarios
- dependencies and constraints from `feature.md` and `plan.md`
- applicable rules from `./workflow/DESIGN.md`
- reusable frontend patterns
- data sources and dynamic content that affect UI
- permissions, roles, platform, or device differences that affect UI

Choose the needed fidelity before writing:

- `conceptual` when the feature still needs a flow, framing, or decision between approaches
- `low-fi` when implementation needs screen structure, content priority, and states
- `implementation-ready` when implementation needs components, layout, responsive behavior, states, content rules, and constraints

Use the lowest fidelity that still lets the next stage move without a new design round.

### 4. Synthesize with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking` and reason through:

- whether the feature truly affects UI
- what the user must accomplish and what the interface must make obvious first
- the happy path and required alternative scenarios
- required states: default, loading, empty, error, disabled or unavailable, success or confirmation, and permission or role differences when relevant
- layout as content priority: primary information, primary action, secondary actions, filters, forms, results, errors, and help text
- mobile and desktop behavior when the feature can be used on multiple viewport sizes
- component contracts: anatomy, required and optional elements, variants needed by the feature, states, behavior, reuse, and accessibility
- interaction and feedback: immediate response, progress, inline errors, confirmations, interruptions, and reduced-motion requirements
- content rules: heading hierarchy, text length, empty copy, error copy, overflow, localization, and data source constraints
- accessibility: keyboard access, focus behavior, labels, screen-reader feedback for dynamic states, color-independent meaning, motion, and touch targets
- implementation constraints: performance, reuse, data availability, and what not to build
- design decisions that have 2-3 viable options and need user confirmation
- whether missing `./workflow/DESIGN.md` or missing canon blocks feature design

Prefer one recommended solution. Present alternatives only for decisions that materially affect implementation or user experience.

### 5. Ask only blocking questions

Try to answer open questions from `feature.md`, `plan.md`, `./workflow/DESIGN.md`, existing frontend code, and project workflow files.

Ask the user only when a decision blocks `design.md` or would otherwise force arbitrary invention. Group independent questions together. For each question, give a recommended option first with `(Recommended)` and a short reason.

Do not run a mandatory questionnaire when local context is sufficient.

### 6. Write `./workflow/features/{slug}/design.md`

Create or update `design.md` using the structure in Artifact Requirements.

When updating an existing `design.md`, rewrite it as the current feature design. Remove stale decisions, biography, deltas, and operational notes.

Do not include code snippets unless a tiny identifier or component name is necessary to point to an existing reusable pattern. The artifact describes UI behavior and constraints, not implementation.

### 7. Update `./workflow/PLAN.md`

If `./workflow/PLAN.md` exists, preserve the feature's existing pipeline status marker. The design stage has no separate status symbol.

Add or update a concise note under the feature entry only when it helps the workflow, for example:

```md
- design: `./workflow/features/{slug}/design.md`
```

If project design canon is missing or insufficient, append a service tail that names the missing canon and why it blocks or weakens this feature design. Keep the tail current-state and actionable; do not record the history of discovery.

If `./workflow/PLAN.md` does not exist, do not create it from this skill. Mention the missing plan in the final response.

## Artifact Requirements

`./workflow/features/{slug}/design.md` must be written in the user's working language unless the surrounding workflow files clearly use another language.

Use this structure:

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

Adapt the template to the feature's complexity:

- Keep a simple feature short. Merge sections only when no required decision is lost.
- Do not create a full component API unless the feature truly introduces or changes a reusable component.
- Do not add pixel-perfect values when `./workflow/DESIGN.md` already defines the level of visual precision.
- Do not create separate wireframe, prototype, token, or asset artifacts from this skill.
- Keep open questions explicit and mark whether each blocks implementation.

`design.md` is ready when it states:

- the user goal and primary scenario
- the chosen fidelity
- layout and responsive behavior
- states beyond the happy path
- component reuse or new component contracts
- content and dynamic-data constraints
- accessibility and motion constraints
- implementation boundaries and explicit non-goals
- blocking open questions, if any

## Updating PLAN.md

At the end, use `./workflow/PLAN.md` only for workflow coordination:

- Preserve existing status markers such as `[ ]`, `[-]`, `[+]`, `[x]`, `[*]`, and `[/]`.
- Add or update the feature's design note when the local format supports notes.
- Append a service tail only for missing project design canon or other design-level blockers that a later stage must resolve.
- Do not invent a new status marker for design.
- Do not archive, complete, test, or task-split a feature from this skill.

## Notes

- If `./workflow/DESIGN.md` is absent, `design.md` may still capture scenario and structural facts from `feature.md`, `plan.md`, and existing frontend code, but it must clearly mark visual-canon decisions as blocked.
- If the feature needs broad product discovery, record the gap as an open question. Do not expand this skill into research, usability testing, design sprint work, or metrics planning.
- If a source design file such as Figma is mentioned in the feature docs, use it as input only when the necessary local or MCP access already exists. Do not require Figma MCP, asset download, or pixel-perfect parity unless the project explicitly requires it.
- Motion must communicate feedback, orientation, continuity, focus, or confirmation. Do not add decorative animation as a requirement.
- Accessibility is part of the design contract, not a later polish task.
