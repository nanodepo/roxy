---
name: planning
description: >
  Turns a feature brief into an implementation plan at
  ./workflow/features/{slug}/plan.md and marks the feature planned in
  ./workflow/PLAN.md. Use for "plan feature", "create plan", "write plan.md",
  or "turn feature into a plan".
---

# Planning

## Purpose

Turn `./workflow/features/{slug}/feature.md` into an implementation plan at
`./workflow/features/{slug}/plan.md` and set the feature status to `[-]` in
`./workflow/PLAN.md`.

Use this skill after `feature`. It answers "how to build this": it reads the
feature brief and project context, inspects the codebase as deeply as a
realistic plan requires, builds a dependency graph, and decomposes the work
into phases with concrete checkboxes, likely touched files, dependencies,
risks, and exit criteria.

The plan is read by `design`, `improve`, `task`, `implement`, `test`, and
`docs`. It does not own the test plan, the feature UI design, task files, or
product code.

## Parameters

Use `args` to name the feature:

```txt
<feature-slug>
```

- `feature-slug` is the directory name under `./workflow/features/`.
- If `feature-slug` is absent, infer it from the current user message and the
  active entries in `./workflow/PLAN.md`, but only when exactly one feature has
  a `feature.md` and matches the request.
- If the feature still cannot be identified, ask one short question for the
  slug and stop until answered.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs,
  branches, commits, pushes, checkout commands, or worktree commands.
- Own only `./workflow/features/{slug}/plan.md` and the matching service status
  in `./workflow/PLAN.md`.
- Do not write `tests.md`, `design.md`, `NN-task.md`, product code, user
  documentation, developer documentation, or a separate PRD or SPEC artifact.
- Stay read-only toward product code. Inspect the codebase to plan; do not
  modify it.
- Do not call downstream skills automatically. Stop after `plan.md` and the
  `PLAN.md` status are ready.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4 before
  asking final questions or writing the plan. Turning a feature into a phased
  implementation plan is analytical work.
- Ask only blocking clarifying questions. A gap is blocking only when the plan
  cannot be made realistic without the answer. Record non-blocking gaps as open
  questions in `plan.md` instead of interrupting the user.
- Write the current plan only. Do not include conversation biography, previous
  plan states, "now/previously" comparisons, migration notes, or removed
  behavior.
- Keep test ideas out of `plan.md`. Verification checkpoints in the plan check
  implementation progress; they do not replace `tests.md`. Phrase any
  verification need as live behavior or a product invariant, not as an incident
  or a deletion check.
- Preserve the working language and local document style of the existing
  `./workflow/` files. Keep paths, tool names, code identifiers, and status
  markers in their original spelling.

## Steps

For this multi-step procedure, use the agent's task planning mode (todo list /
task plan, whichever is available) and close items one by one.

### 1. Identify the feature

Resolve `{slug}` from `args`, the user message, or `./workflow/PLAN.md`.

Stop and ask for the slug if:

- `./workflow/features/{slug}/` does not exist;
- `./workflow/features/{slug}/feature.md` is missing;
- several active features could match the request.

Do not create a missing feature brief in this skill.

If `./workflow/features/{slug}/plan.md` already exists, treat this run as an
update: rebuild the plan from current inputs rather than appending to the old
text.

### 2. Read the feature and project context

Read, in this order:

- `./workflow/features/{slug}/feature.md` — the scope authority for the plan.
- `./workflow/features/{slug}/design.md`, if it exists — UI constraints.
- `./workflow/PROJECT.md` — stack, run, deploy, and project shape.
- `./workflow/ARCHITECTURE.md` — the architectural pattern and code placement
  rules the plan must respect.
- `./workflow/DESIGN.md` — only when the feature touches UI or user
  interaction.
- `./workflow/PLAN.md` — existing feature list and workflow status.

Use the feature brief as the scope authority. Use architecture, design, and
project files as constraints, not as permission to add unrelated work.

### 3. Reconnoiter the codebase

Inspect the codebase as deeply as a realistic plan requires, but stay focused
on evidence the plan needs. Use `rg`, `rg --files`, and direct reads.

Find and verify:

- similar modules, routes, components, services, schemas, migrations, configs,
  and conventions the plan should follow;
- what already exists, what must change, what must be created;
- public API, CLI, database, or UI contracts that several parts of the system
  share;
- boundaries the architecture or stack does not allow the plan to cross;
- deployment or runtime concerns that constrain the implementation.

Record what is verified versus assumed, so the plan does not name phantom
paths, modules, or dependencies.

### 4. Analyze with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking` and reason through:

- the behavior the feature must deliver and the technical approach that fits
  the existing architecture and stack;
- vague feature requirements reframed as verifiable success criteria;
- what is `In` scope and what must be explicitly `Out` to stop scope creep;
- a dependency graph across foundation, data/model, API/contracts,
  UI/interaction, integrations, and rollout/runtime constraints;
- a decomposition into phases that each lead to working, verifiable behavior —
  prefer vertical slices that produce real behavior over horizontal layers of
  isolated infrastructure;
- for each phase: goal, concrete ordered checkboxes, likely touched files,
  dependencies, risks with practical mitigations, and exit criteria;
- which work is strictly sequential and which can run in parallel;
- assumptions safe to keep versus gaps that block a realistic plan.

Use reference ideas as filters:

- Split any phase or checkbox that is too broad: it spans many files or many
  subsystems, or its name contains "and". Each checkbox is concrete, ordered,
  and verb-first; avoid vague steps like "handle backend" or "do auth".
- Keep each phase context-safe: a limited scope, named dependencies, concrete
  files, and a clear exit criterion, so it can be executed without holding the
  whole project in mind.
- If a phase still cannot be made context-safe, say so in the plan and propose
  how to cut it smaller.
- If a planning decision changes the meaning of the feature, record it in
  `plan.md` under decisions or raise it as an open question.

### 5. Ask blocking questions only

Try to close gaps from `feature.md`, the workflow files, and the codebase
before asking the user.

Ask at most three concise questions in one block, grouped by category — scope,
user interaction, data, integrations, constraints. For each question, put the
recommended answer first with `(Recommended)` and a short reason, and offer
only materially different options.

If a gap does not block a realistic plan, write it under `Open questions` in
`plan.md` instead of interrupting the user.

### 6. Write `plan.md`

Write `./workflow/features/{slug}/plan.md` using the Artifact Requirements
below. Describe the current plan state with no history of how the plan was
produced.

### 7. Update `./workflow/PLAN.md`

Set the feature status to `[-]` (planned).

Preserve the file's structure, other feature entries, and their status markers.
Change only the line for this feature. Use the existing local format; if no
stronger pattern exists, point the entry at
`./workflow/features/{slug}/plan.md`.

### 8. Final verification

Reread the written `plan.md` and the changed `./workflow/PLAN.md` entry.

Confirm:

- the plan can be handed to `design`, `improve`, or `task` with no chat
  retelling;
- no phantom path, module, or dependency remains as an instruction;
- `In` and `Out` scope are explicit;
- every phase has a goal, ordered checkboxes, likely files, dependencies, and
  an exit criterion;
- no `tests.md`, `design.md`, `NN-task.md`, code, or documentation was created;
- the plan describes the current desired state without biography or delta
  wording.

## Artifact Requirements

Write `./workflow/features/{slug}/plan.md` in the working language of the
existing `./workflow/` files.

Use this structure, scaled to the feature's complexity — omit optional sections
that add no information for a simple feature:

```md
# plan.md

## Цель

What behavior the feature must deliver and the chosen approach.

## Scope

### In

- What the implementation covers

### Out

- What the implementation does not cover

## Контекст

- Input documents
- Relevant modules
- Local patterns to follow
- Architecture, design, or stack constraints

## Решения

- Technical decisions and their reasons
- Contracts shared between parts of the system
- Assumptions safe enough to proceed on

## Dependency graph

- Foundation
- Data/model
- API/contracts
- UI/interaction
- Integrations
- Rollout/runtime constraints

## Фазы

### Phase 1: <name>

Goal: ...

- [ ] Concrete, ordered, verb-first action
- [ ] Concrete, ordered, verb-first action

Likely files:

- `path/to/file`

Dependencies:

- None / Phase N / external answer

Risks:

- Risk -> practical mitigation

Exit criteria:

- What must be true after the phase

### Phase 2: <name>

...

## Checkpoints

- [ ] After foundation: ...
- [ ] After the core flow: ...
- [ ] Before handing off to `task`: ...

## Open questions

- Only questions that affect the implementation

## Хвосты

- What to pass on to `design`, `task`, `test`, `docs`, or the user
```

The plan is ready when:

- another agent can act on it without conversation history;
- `In` and `Out` keep the scope from spreading;
- each phase has a goal, ordered actions, likely files, dependencies, and exit
  criteria;
- risks carry a practical mitigation, not a generic warning;
- oversized phases are cut to a manageable size;
- checkpoints verify implementation progress without standing in for a test
  plan;
- open questions are short and genuinely affect the implementation;
- the plan holds no test canon, feature UI design, code, or documentation.

If the feature is too thin to plan safely, still keep this skill's boundary:
write a minimal honest `plan.md` that names the missing inputs and the next
required workflow stage instead of fabricating phases.

## Updating PLAN.md

At the end, set the matching feature in `./workflow/PLAN.md` to status `[-]`.

The status markers are:

- `[ ]` new;
- `[-]` planned;
- `[+]` split into tasks;
- `[x]` implemented;
- `[*]` tested;
- `[/]` archived.

Change only this feature's line. Do not touch statuses or entries for other
features.

## Notes

- A missing `PROJECT.md`, `ARCHITECTURE.md`, `DESIGN.md`, `VISION.md`, or
  `ROADMAP.md` does not stop this skill. Proceed with the available context and
  avoid inventing constraints.
- If the codebase contradicts `feature.md`, trust the verified codebase for
  what exists and plan around it, then record the conflict as an open question
  or a tail.
- If the user's request is really to design, improve, split into tasks,
  implement, test, or document the feature, report the matching downstream
  skill instead of doing that work here.
- Keep the output operational: the next agent should see the current plan, not
  a story about how it was built.
