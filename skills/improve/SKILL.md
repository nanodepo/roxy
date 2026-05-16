---
name: improve
description: >
  Improves an existing feature plan after planning and optional design review.
  Use for "improve plan", "review plan", "second pass", "strengthen plan",
  or "find gaps in plan".
---

# Improve

## Purpose

Make a second pass over an existing feature plan and improve only
`./workflow/features/{slug}/plan.md`.

Use this skill after `planning` and, when applicable, after `design`. It checks
the plan against the feature brief, project workflow files, and the actual
codebase. It finds holes, wrong dependencies, weak wording, unsafe rewrites,
unnecessary scope, and hidden prerequisites, then rewrites the plan so it is
more executable and self-contained.

The skill owns plan quality only. It does not write tests, task files, product
code, or documentation. Test and documentation ideas are recorded only as
service tails in `./workflow/PLAN.md` when they affect later pipeline stages.

## Parameters

Use `args` to name the feature:

```txt
<feature-slug>
```

- `feature-slug` is the directory name under `./workflow/features/`.
- If `feature-slug` is absent, infer it from the user message and active
  entries in `./workflow/PLAN.md` only when exactly one matching feature has
  both `feature.md` and `plan.md`.
- If the feature still cannot be identified, ask one short question for the
  slug and stop until answered.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs,
  branches, commits, pushes, checkout commands, or worktree commands.
- Own only `./workflow/features/{slug}/plan.md` and the matching service status
  or service tails in `./workflow/PLAN.md`.
- Do not write `tests.md`, `NN-task.md`, product code, user documentation, or
  developer documentation.
- Do not expand the feature beyond `feature.md`, `plan.md`, and current project
  constraints. Remove speculative future work from the implementation plan.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4 before
  editing `plan.md`. Plan improvement is analytical work.
- Keep test ideas out of `plan.md` unless they are necessary implementation
  acceptance or verification constraints. Record discovered test risks as
  service tails for `test` in `./workflow/PLAN.md`.
- Preserve the current project language and local document style in generated
  workflow artifacts. Keep paths, tool names, code identifiers, and status
  markers in their original spelling.
- Write current plan truth only. Do not leave biography, "previously / now"
  comparisons, migration notes, stale decisions, or removed behavior in
  `plan.md`.

## Steps

For this multi-step procedure, use the agent's task planning mode (todo list /
task plan, whichever is available) and close items one by one.

### 1. Identify the feature

Resolve `{slug}` from `args`, the user message, or `./workflow/PLAN.md`.

Stop and ask for the slug if:

- `./workflow/features/{slug}/` does not exist;
- `./workflow/features/{slug}/feature.md` is missing;
- `./workflow/features/{slug}/plan.md` is missing;
- several active features could match the request.

Do not create a missing feature or plan in this skill.

### 2. Load the plan context

Read, in this order:

- `./workflow/features/{slug}/feature.md`;
- `./workflow/features/{slug}/plan.md`;
- `./workflow/features/{slug}/design.md`, if it exists;
- `./workflow/PLAN.md`, if it exists;
- `./workflow/PROJECT.md`, `./workflow/ARCHITECTURE.md`,
  `./workflow/DESIGN.md`, `./workflow/VISION.md`, and
  `./workflow/ROADMAP.md` when they exist and affect this plan.

Use the feature brief as the scope authority. Use `design.md` and workflow
canon files as constraints, not as permission to add unrelated work.

### 3. Re-check the codebase

Inspect the codebase narrowly but concretely. Start from files, modules,
symbols, routes, commands, and dependencies named by `feature.md`, `plan.md`,
and `design.md`. Use `rg`, `rg --files`, and direct reads to verify plan
claims.

Check for:

- paths, files, modules, symbols, routes, commands, or components that do not
  exist;
- parent paths that must exist before a file can be created;
- reuse claims that are unsupported by local code;
- functionality that already exists and should not be rebuilt;
- public API, CLI, database, or UI contracts that the plan names too vaguely;
- local patterns the plan should follow instead of inventing new structure.

Keep the scan focused on evidence needed to improve `plan.md`.

### 4. Analyze the plan with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking` and produce a prioritized
defect list plus a rewrite strategy.

Analyze these dimensions:

- **Clarity** - steps name concrete files, modules, commands, tools, and
  expected outcomes.
- **Completeness** - the plan covers the feature scope, edge cases, and
  required integration points without leaving gaps.
- **Feasibility** - every step is achievable with available code, tools, and
  context.
- **Consistency** - ordering, dependencies, terminology, and constraints do
  not contradict each other.
- **Scope discipline** - every step is justified by the current feature, uses
  the simplest workable approach, and avoids speculative abstraction.

Use this defect taxonomy:

- phantom paths, files, modules, symbols, commands, or dependencies;
- hidden prerequisites that must happen before a later step can work;
- task boundaries that mix unrelated concerns or split dependent work badly;
- unsafe rewrites of existing code without a containment or verification plan;
- overconfident dependency graphs or parallelization assumptions;
- vague implementation claims where a real contract or local pattern is needed;
- design or architecture conflicts;
- implementation plan content that belongs to `test`, `docs`, `task`, or the
  user instead;
- tests described as incidents or deletion checks rather than live invariants.

Classify each finding as:

- **must fix in `plan.md`** - the current plan would mislead implementation;
- **service tail for `PLAN.md`** - later `test`, `docs`, `design`, `planning`,
  or user work is needed but should not be inserted into the implementation
  plan;
- **ignore** - unsupported, speculative, already covered, or outside this
  skill's scope.

If the plan is too weak to repair safely, still keep this skill's boundary:
rewrite `plan.md` into a minimal honest plan that names the missing inputs and
next required workflow stage instead of fabricating details.

### 5. Rewrite `plan.md`

Edit only `./workflow/features/{slug}/plan.md`.

Choose the smallest safe edit:

- Rewrite the whole file when the structure is misleading, stale, or too weak
  to patch.
- Make targeted edits when the current format is sound and only specific
  sections need correction.

The improved plan must:

- stand on its own for `task` and `implement`;
- name concrete files, modules, commands, and local patterns when known;
- order work by real dependencies;
- separate implementation work from tests, docs, and user decisions;
- remove unsupported future-proofing and premature abstractions;
- preserve useful constraints from `feature.md`, `design.md`, and workflow
  canon files;
- keep open questions only when they block safe implementation and cannot be
  resolved from local context;
- phrase verification needs as live behavior or product invariants, not as
  memories of bugs or removed behavior.

Do not add a review report to `plan.md`. The artifact is the improved plan.

### 6. Update `./workflow/PLAN.md`

Touch `./workflow/PLAN.md` only when the improvement changes service status or
reveals cross-stage tails.

Allowed updates:

- keep the feature in planned status `[-]` when the plan is still the current
  pipeline artifact;
- add a concise tail for `test` when the improved plan reveals a test risk;
- add a concise tail for `docs`, `design`, `planning`, or the user only when
  that later-stage work is required and outside `plan.md`;
- preserve unrelated feature entries and existing statuses.

Use the existing local format when `./workflow/PLAN.md` has one. If no stronger
format exists, append a tail under the feature entry:

```md
  - tail/test: <live invariant or risk to cover later>
```

Do not mark the feature as `[+]`, `[x]`, `[*]`, or `[/]` in this skill.

### 7. Final verification

Reread the updated `plan.md` and any changed `./workflow/PLAN.md` entry.

Confirm:

- every must-fix defect from Step 4 is addressed or explicitly converted into a
  blocking open question;
- no phantom path or unsupported dependency remains as an instruction;
- no test, task, code, or documentation artifact was created;
- test risks are service tails, not implementation-plan clutter;
- the plan describes the current desired state without biography or delta
  wording;
- the next pipeline stage can act from the updated artifacts without knowing
  this review session.

## Artifact Requirements

This skill produces:

- **Improved `./workflow/features/{slug}/plan.md`** - a current, executable,
  self-contained implementation plan.
- **Optional update to `./workflow/PLAN.md`** - only service status or service
  tails for later stages.

This skill does not create `references/`, `tests.md`, `NN-task.md`, product
code, documentation files, reports, or review scorecards.

## Updating PLAN.md

At the end, update `./workflow/PLAN.md` only for the matching feature.

The status markers are:

- `[ ]` new;
- `[-]` planned;
- `[+]` split into tasks;
- `[x]` implemented;
- `[*]` tested;
- `[/]` archived.

Normally leave the feature at `[-]`. Add service tails when needed so later
pipeline stages inherit the current risk without mixing their work into
`plan.md`.

## Notes

- A missing `design.md`, `PROJECT.md`, `ARCHITECTURE.md`, `DESIGN.md`,
  `VISION.md`, or `ROADMAP.md` does not stop this skill. Proceed with the
  available context and avoid inventing constraints.
- If local code contradicts the plan, trust the verified codebase for what
  exists and rewrite `plan.md` so future implementation is not misled.
- If the user's request is to implement, test, document, or split tasks, report
  the matching downstream skill instead of doing that work here.
- Keep the output boring and operational: the next agent should see the
  current plan, not a story about how the plan was improved.
