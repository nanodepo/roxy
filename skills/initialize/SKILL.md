---
name: initialize
description: >
  Bootstraps project workflow: creates ./workflow/, records tech ctx in
  ./workflow/PROJECT.md, seeds ./workflow/PLAN.md tails. Triggers:
  инициализируй проект, подготовь проект к работе, создай workflow,
  забутстрапь проект.
---

# Initialize

## Purpose

Bootstrap project so agent workflow has canon home. Owns:
`./workflow/` dirs, tech ref `./workflow/PROJECT.md`, base
`./workflow/PLAN.md` routing next stages.

First workflow stage. After it: `roadmap`, `architecture`,
`design-guideline`.

Cheap + safe bootstrap: create canon place, make no product/architecture/design
decisions. Do not write `VISION.md`, `ROADMAP.md`, `ARCHITECTURE.md`, pick
architecture, describe design, or create features. `PLAN.md` tails route later
stages; `initialize` does not own them.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, `git init`.
  Dirty tree expected; user owns git.
- Stay in boundary. Do not write `VISION.md`, `ROADMAP.md`,
  `ARCHITECTURE.md`, `DESIGN.md`; do not select architecture, describe design,
  create `feature.md`, or write feature folder content. Route as `PLAN.md`
  tails only.
- Do not touch app. Do not scaffold app code, generate `.gitignore`,
  `.env.example`, `README.md`, edit `AGENTS.md`, `CLAUDE.md`, `.claude/`,
  hooks, or symlinks.
- Safe idempotence. If `./workflow/` exists, never overwrite user text. Fill
  missing sections only, keep existing `PLAN.md` entries/tails, add tail only
  for truly absent artifact.
- Facts only. `PROJECT.md` records observable project-file facts. Unknown =
  `Needs clarification`, not guess.
- Current state only. `PROJECT.md` / `PLAN.md` contain no biography, decision
  logs, or "previously/now" comparisons.
- Do not auto-call downstream skills. Stop after structure, `PROJECT.md`,
  `PLAN.md` ready.

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

### 1. Detect `./workflow/` state

Check whether these exist:

- `./workflow/`;
- `./workflow/PROJECT.md`;
- `./workflow/PLAN.md`;
- `./workflow/VISION.md`;
- `./workflow/ROADMAP.md`;
- `./workflow/ARCHITECTURE.md`;
- `./workflow/DESIGN.md`;
- `./workflow/archive/`;
- `./workflow/features/`.

If none exist: fresh bootstrap. Create all required structure.

If `./workflow/` exists: idempotent update. Read existing files, preserve
content, complete only missing pieces.

### 2. Collect tech ctx

Inspect project root for stack. Read sources in priority order; later sources
fill only gaps:

1. Package manifests + lock files: `package.json`, `pyproject.toml`, `go.mod`,
   `Cargo.toml`, `pom.xml`, locks.
2. Language/build config: compiler, bundler, framework config.
3. `Dockerfile`, Compose, devcontainer, CI.
4. Test/lint/format config.
5. Env templates: `.env.example`, similar.
6. `README.md` and `docs/` as support only, never source of truth.
7. User, for facts no file reveals.

Record observable facts only. If critical fact (stack, run cmd, key services)
is unknown, write `Needs clarification` in `PROJECT.md` and add clarification
tail in `PLAN.md`. Ask user only for facts that block usable tech ref; keep
questions few + short.

### 3. Create dirs

Create missing dirs:

- `./workflow/`;
- `./workflow/archive/`;
- `./workflow/features/`.

Leave existing dirs/content untouched.

### 4. Write `./workflow/PROJECT.md`

Create `./workflow/PROJECT.md` from Artifact Requirements sections, or update
existing file.

On update: keep user text, fill missing/empty sections only. Do not rewrite
valid content. Replace section only when project files clearly contradict it.

### 5. Create/update `./workflow/PLAN.md`

Create base `./workflow/PLAN.md` if missing, or update existing one per
Updating PLAN.md. Add routing tails for project-stage artifacts that do not
exist.

## Artifact Requirements

### `./workflow/PROJECT.md`

Tech ref for later agents. They should not re-derive stack or need chat
history. Include: project location, run, verify, chosen tools, open decisions.

Use semantic sections below. Fill with observable facts only. Translate visible
headings, field labels, table headers, placeholders, examples before writing
artifact. If data missing, keep short or localized `Needs clarification`; do not
pad.

- Project: name/type when reliable.
- Stack: languages, frameworks, versions.
- Package manager and commands: package mgr + main cmds.
- Run locally: local start steps.
- Tests: test cmd.
- Build: build cmd.
- Lint / format / typecheck: change checks.
- Environment and external services: env vars + external services.
- Docker / CI / deploy: only when files present.
- Open questions: facts not determined from files; each becomes clarification
  tail in `PLAN.md`.

Keep tight. Each line = actionable fact. No best-practice filler, history, time
estimates, team/onboarding prose.

### `./workflow/PLAN.md`

Light status index: feature list + service tails. Fresh bootstrap has no
features; create empty feature list + routing tails. Use base shape when no
stronger local pattern exists. Translate visible headings, field labels, table
headers, placeholders, examples:

```md
# PLAN

## Features

<empty until the feature skill adds entries>

## Tails

- [ ] roadmap — ...
- [ ] architecture — ...
```

Feature statuses: `[ ]` new, `[-]` planned, `[+]` split into tasks, `[x]` done,
`[*]` tested, `[/]` archived. `initialize` adds no feature entries; only
structure + tails.

## Updating PLAN.md

At end, ensure `./workflow/PLAN.md` has service tail for each missing next-stage
artifact:

- `roadmap` tail if `./workflow/VISION.md` or `./workflow/ROADMAP.md` absent;
- `architecture` tail if `./workflow/ARCHITECTURE.md` absent;
- `design-guideline` tail if `./workflow/DESIGN.md` absent;
- tech-clarification tail if `PROJECT.md` has Open questions.

Preserve feature entries, statuses, tails. No duplicate tail for existing
artifact or existing tail. Do not change feature status; `initialize` is not a
feature stage.

## Notes

- Missing input files do not stop skill. Use available project files; record
  rest as open questions.
- Existing `./workflow/`: every file = input. Merge conservatively; complete
  missing parts, never discard user content.
- Project `README.md` = support hint only, never stack authority.
- Ready state: `./workflow/` structure exists, `PROJECT.md` is fact-based tech
  ref, `PLAN.md` lists next open project stages.
