---
name: improve
description: >
  Improves existing feature plan after planning/design review. Triggers:
  improve plan, review plan, second pass, strengthen plan, find gaps in plan.
---

# Improve

## Purpose

Improve one existing feature plan:
`./workflow/features/{slug}/plan.md`.

Use after `planning`; after `design` when relevant. Check plan vs feature brief,
workflow canon, actual codebase. Find gaps, wrong deps, weak wording, unsafe
rewrites, extra scope, hidden prereqs. Rewrite plan into executable,
self-contained, current-feature-bounded artifact.

Own plan quality only. May update matching entry in `./workflow/PLAN.md` only
for service status or later-stage tails. Do not write tests, task files, code,
docs, reports, or scorecards.

## Params

Use `args`:

```txt
<feature-slug>
```

- `feature-slug`: dir under `./workflow/features/`.
- No `feature-slug`: infer from user msg + active `./workflow/PLAN.md` entries
  only when exactly one match has both `feature.md` and `plan.md`.
- Still unknown: ask one short slug question, stop.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout, worktree.
- Own only `./workflow/features/{slug}/plan.md` plus matching service status /
  service tails in `./workflow/PLAN.md`.
- Do not write `tests.md`, `NN-task.md`, code, user docs, or dev docs.
- Do not expand beyond `feature.md`, `plan.md`, current project constraints.
  Remove speculative future work.
- Call `mcp__sequential-thinking__sequentialthinking` during Step 4 before
  editing `plan.md`. Plan improvement = analytical work.
- Keep test ideas out of `plan.md` unless they are required impl acceptance /
  verification constraints. Put discovered test risks as `test` service tails
  in `./workflow/PLAN.md`.
- Write current plan truth only. No biography, "previously/now" comparisons,
  migration notes, stale decisions, or removed behavior in `plan.md`.

## Language Notice

Write this `SKILL.md` in English.

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

### 1. Identify feature

Resolve `{slug}` from `args`, user msg, or `./workflow/PLAN.md`.

Stop and ask slug if:

- `./workflow/features/{slug}/` missing;
- `./workflow/features/{slug}/feature.md` missing;
- `./workflow/features/{slug}/plan.md` missing;
- several active features match.

Do not create missing feature or plan.

### 2. Load plan ctx

Read in order:

- `./workflow/features/{slug}/feature.md`;
- `./workflow/features/{slug}/plan.md`;
- `./workflow/features/{slug}/design.md`, if exists;
- `./workflow/PLAN.md`, if exists;
- `./workflow/PROJECT.md`, `./workflow/ARCHITECTURE.md`,
  `./workflow/DESIGN.md`, `./workflow/VISION.md`, `./workflow/ROADMAP.md`
  when present and relevant.

Use `feature.md` as scope authority. Use `design.md` + canon files as
constraints, not permission for unrelated work.

### 3. Re-check codebase

Inspect narrowly, concretely. Start from files, modules, symbols, routes,
commands, deps named by `feature.md`, `plan.md`, `design.md`. Use `rg`,
`rg --files`, direct reads to verify claims.

Check for:

- phantom paths, files, modules, symbols, routes, commands, components;
- parent paths needed before file creation;
- reuse claims unsupported by local code;
- existing functionality that should not be rebuilt;
- vague public API, CLI, DB, UI contracts;
- local patterns plan should follow instead of new structure.

Keep scan limited to evidence needed for `plan.md`.

### 4. Analyze with `mcp__sequential-thinking__sequentialthinking`

Call `mcp__sequential-thinking__sequentialthinking`. Produce prioritized defect
list + rewrite strategy.

Analyze:

- Clarity: concrete files, modules, commands, tools, outcomes.
- Completeness: scope, edge cases, integration points covered.
- Feasibility: steps achievable with available code/tools/context.
- Consistency: order, deps, terms, constraints align.
- Scope discipline: each step justified by feature, simplest workable route,
  no speculative abstraction.

Defect taxonomy:

- phantom paths/files/modules/symbols/commands/deps;
- hidden prereqs;
- task boundaries mixing unrelated concerns or splitting dependent work badly;
- unsafe rewrites without containment/verification;
- overconfident dep graph or parallelization;
- vague impl claim needing real contract/local pattern;
- design/architecture conflict;
- content belonging to `test`, `docs`, `task`, or user;
- tests framed as incidents/deletion checks instead of live invariants.

Classify each finding:

- `must fix in plan.md`: current plan would mislead impl.
- `service tail for PLAN.md`: later `test`, `docs`, `design`, `planning`, or
  user work needed; keep out of impl plan.
- `ignore`: unsupported, speculative, covered, or out of scope.

If plan too weak to repair safely, rewrite `plan.md` into minimal honest plan
that names missing inputs + next required workflow stage. Do not fabricate.

### 5. Rewrite `plan.md`

Edit only `./workflow/features/{slug}/plan.md`.

Smallest safe edit:

- Whole rewrite when structure misleading, stale, or too weak to patch.
- Targeted edits when format sound and only specific sections need correction.

Improved plan must:

- stand alone for `task` and `implement`;
- name concrete files, modules, commands, local patterns when known;
- order work by real deps;
- separate impl from tests, docs, user decisions;
- remove unsupported future-proofing and premature abstractions;
- preserve useful constraints from `feature.md`, `design.md`, canon files;
- keep open questions only when they block safe impl and local ctx cannot
  resolve them;
- phrase verification as live behavior / product invariants, not bug memories
  or removed behavior.

Do not add review report. Artifact = improved plan.

### 6. Update `./workflow/PLAN.md`

Touch `./workflow/PLAN.md` only when improvement changes matching feature
service status or reveals cross-stage tails.

Status markers:

- `[ ]` new;
- `[-]` planned;
- `[+]` split into tasks;
- `[x]` implemented;
- `[*]` tested;
- `[/]` archived.

Normally leave feature `[-]`: this skill improves plan; it does not split,
implement, test, or archive.

Allowed:

- keep feature `[-]` when plan remains current pipeline artifact;
- add concise `test` tail for revealed test risk;
- add concise `docs`, `design`, `planning`, or user tail only when required and
  outside `plan.md`;
- preserve unrelated entries/statuses.

Use local format. If none, append under feature:

```md
  - tail/test: <live invariant or risk to cover later>
```

Do not mark feature `[+]`, `[x]`, `[*]`, or `[/]`.

### 7. Final verification

Reread updated `plan.md` and changed `./workflow/PLAN.md` entry.

Confirm:

- each must-fix defect addressed or converted to blocking open question;
- no phantom path / unsupported dep remains as instruction;
- no test, task, code, doc artifact created;
- test risks are service tails, not impl-plan clutter;
- plan states current desired state with no biography/delta wording;
- next pipeline stage can act from artifacts without review-session context.

## Artifact Requirements

Produces:

- Improved `./workflow/features/{slug}/plan.md`: current, executable,
  self-contained impl plan.
- Optional `./workflow/PLAN.md` update: only service status/tails.

Does not create `references/`, `tests.md`, `NN-task.md`, code, docs, reports,
or scorecards.

## Notes

- Missing `design.md`, `PROJECT.md`, `ARCHITECTURE.md`, `DESIGN.md`,
  `VISION.md`, or `ROADMAP.md` does not stop skill. Use available ctx; avoid
  invented constraints.
- If local code contradicts plan, trust verified codebase for existence and
  rewrite `plan.md` so impl is not misled.
- If user asks implement/test/docs/task split, report matching downstream skill
  instead of doing it here.
- Keep output boring, operational: current plan, not review narrative.
