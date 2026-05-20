---
name: docs
description: >
  Creates or updates the project's user and technical documentation — the root
  README.md and pages under ./docs/concept, ./docs/spec, ./docs/tech — recording
  the current state of the product. Use when the user says "write documentation",
  "update documentation", "document the project", "document a feature", or asks
  to document implemented behavior.
---

# Docs

## Purpose

Write/maintain project user + tech docs. Record current product: purpose,
behavior, build/run. Reader understands project without dev history.

Runs late, usually after `test`. Reads canon + feature artifacts; owns only root
`README.md` and `./docs/concept/`, `./docs/spec/`, `./docs/tech/`.

In feature-update mode, marks the documented feature `[/]` archived in
`./workflow/PLAN.md` once its behavior is captured.

Does not write audit report, product code, tests, or feature `./workflow/`
artifacts. No git ops. Mismatch recs belong to `audit`; `docs` fixes docs and
only notes missing docs as docs tails.

## Parameters

Optional `args`:

```txt
[<feature-slug>]
```

- `<feature-slug>` under `./workflow/features/{slug}/` -> feature-update mode:
  document implemented feature behavior.
- No args -> infer mode from project docs state.

## Strict Rules

- No git ops: no status, diff, branch, commit, push.
- Step 5 must call `mcp__sequential-thinking__sequentialthinking`.
- Current state only. Present tense. No changelog, release notes, version
  history, `previously/now`, breaking-change markers. Replace superseded text.
- Stay in boundary: no code, no tests, no audit report, no feature
  `feature.md`/`plan.md`/`design.md`/`tests.md` rewrites.
- `./workflow/PLAN.md` write only appends docs tails and, in feature-update
  mode, sets the documented feature to `[/]` archived.
- One term per concept across all docs.

## Canonical Documentation Discipline

Treat owned docs as current operating context for future agents and users. They
are not an audit trail.

- Every retained statement must be true now, useful to a future reader, and
  placed in one canonical home.
- Rewrite superseded text into the current rule or behavior. Delete it when the
  code, UI, config, or a clearer doc already carries the same truth.
- Do not preserve creation history, migration narrative, removed behavior,
  abandoned decisions, or temporary notes in `README.md` or `./docs/`.
- Keep historical reason only when it explains an active non-obvious constraint.
  Write the constraint as present-tense guidance; omit the story.
- When docs disagree, resolve to the current source of truth instead of blending
  versions. Leave one current statement and link to it when other docs need it.

## Language Notice

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

### 1. Mode

Split `args` by spaces.

- One token -> `<feature-slug>`. Confirm
  `./workflow/features/{slug}/feature.md`; missing -> stop, ask correct slug.
  Run feature-update mode.
- No token -> inspect `README.md` + `./docs/`.
  - No docs -> from-scratch mode, document whole project.
  - Docs exist -> ambiguous; ask one question: whole project or feature update
    plus slug.

### 2. Read Project Context

Read if exists:

- `./workflow/PROJECT.md` — stack/run/deploy.
- `./workflow/VISION.md` — vision.
- `./workflow/ROADMAP.md` — goals.

Feature-update mode: also read `./workflow/features/{slug}/feature.md`
(required), plus `plan.md`, `design.md`, `tests.md` when present.

### 3. Scan Code + Docs

Read implemented code in scope: modules, entry points, cmds, behavior. Read
current docs: `README.md`, `./docs/concept/`, `./docs/spec/`, `./docs/tech/`.

Code, config, routes, schemas, and runnable commands = source of truth for what
exists. Feature artifacts = intent, risk, and acceptance context. Existing docs
are claims to verify, not truth by default.

### 4. Reconcile Docs

For existing docs, check statements against live code: names, signatures,
behavior, cmds, config. Mark confirmed vs mismatch. Preserve structure; plan
in-place fixes. Remove or replace stale owned-doc text instead of adding
corrections beside it. From-scratch with no docs -> skip.

### 5. Analyze

Call `mcp__sequential-thinking__sequentialthinking`. Decide:

- docs to create/update and folder:
  - `./docs/concept/` -> why: purpose, ideas, value
  - `./docs/spec/` -> what: behavior/contracts/reference
  - `./docs/tech/` -> how: structure, setup, run, extend
- do not split one doc across explanation/reference/instruction
- do not add full four-quadrant docs taxonomy
- reader + task per doc before writing
- root `README.md` coverage
- Step 4 mismatches to fix
- canonical home for each fact; other docs link or summarize only when useful
- text to delete because it is duplicate, stale, biographical, or unsupported
- unsupported gaps -> docs tails

Fix result as structured docs plan.

### 6. Write Docs

From plan:

- create/update `README.md` per Artifact Req
- create/update `./docs/concept/`, `./docs/spec/`, `./docs/tech/` pages
- each page serves chosen reader/task
- existing doc -> keep structure, fix mismatches in place
- current state only; no history/deltas/temp notes
- unsupported behavior stays out of docs; record a docs tail only when a real
  documentation need remains
- remove empty sections, stale TODOs, and placeholders that no longer help the
  reader

### 7. Self-Check

Verify docs:

- factual accuracy: code/names/behavior match impl
- clarity: plain, unambiguous
- consistent terminology
- no duplication: one fact in one doc; others link
- self-contained present: no history/external explanation needed
- retention: every kept paragraph answers a current reader need
- authority: contradictions resolved to one current statement

### 8. Update `./workflow/PLAN.md`

Append docs tails: needed docs unsupported by current material. Tail wording
states the active missing documentation, not how the gap was discovered.

In feature-update mode, set the documented feature to `[/]` once its behavior is
documented. Change only that feature line; do not touch other entries or
statuses. From-scratch mode changes no feature status.

If `PLAN.md` missing, list gaps in report.

Status markers: `[ ]` new, `[-]` planned, `[+]` split into tasks, `[x]`
implemented, `[*]` tested, `[/]` archived.

### 9. Report

Short report: mode, docs created/updated, mismatches fixed, docs tails appended,
feature marked `[/]` if feature-update mode.

## Artifact Req

### Root `README.md`

Minimum:

- project name
- short purpose
- quick start or usage example
- config + run

Front door: concise, current, accurate.

### `./docs/` Pages

- `./docs/concept/*.md`: purpose, core ideas, value.
- `./docs/spec/*.md`: behavior/contracts, precise lookup reference.
- `./docs/tech/*.md`: structure/ops, setup, run, extend.

Format rules:

- Current state, present tense.
- No change history, release notes, versions, `previously/now`.
- No "changed from", "new", "legacy", "old", or "after migration" phrasing
  unless the word is part of a live product term or active compatibility
  contract.
- No ops chatter; every line helps reader.
- One canonical home per fact; link instead of duplicating detail.
- Diagram optional only when clarifies.

## Notes

- Partial input ok. Missing `PROJECT.md`, `VISION.md`, or `ROADMAP.md` -> use
  available ctx + code.
- If tests are documented, describe current guarantees and active gaps, not bug
  incidents that caused the tests to exist.
