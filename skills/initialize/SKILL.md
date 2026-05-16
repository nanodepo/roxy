---
name: initialize
description: >
  Bootstraps a project for the agent workflow — creates the ./workflow/
  structure, records technical context in ./workflow/PROJECT.md, and seeds
  ./workflow/PLAN.md with routing tails. Use when the user says "инициализируй
  проект", "подготовь проект к работе", "создай workflow", or "забутстрапь
  проект".
---

# Initialize

## Purpose

This skill bootstraps a project so the rest of the agent workflow has a place to
write its canon. It owns three concerns: the `./workflow/` directory structure,
the technical reference in `./workflow/PROJECT.md`, and a base
`./workflow/PLAN.md` that routes the next project stages.

It is the first stage of the project workflow. After it, `roadmap`,
`architecture`, and `design-guideline` fill the created structure with content.

Bootstrap stays cheap and safe: it creates the place for canon but makes no
product, architectural, or visual decisions. It does not write `VISION.md`,
`ROADMAP.md`, or `ARCHITECTURE.md`, does not pick an architecture pattern, does
not describe design, and does not create features. The service tails it writes
into `PLAN.md` route those stages but do not make `initialize` their owner.

## Strict Rules

- **No git operations** in any form: no status checks, diffs, logs, branches,
  commits, pushes, or `git init`. The working tree is expected to be dirty; the
  user manages git.
- **Stay inside the boundary.** Do not write `VISION.md`, `ROADMAP.md`,
  `ARCHITECTURE.md`, or `DESIGN.md`; do not select an architecture pattern; do
  not describe design; do not create `feature.md` or any feature folder content.
  Route that work as tails in `PLAN.md` only.
- **Do not touch the application.** Do not scaffold app code, generate
  `.gitignore`, `.env.example`, or `README.md`, and do not edit `AGENTS.md`,
  `CLAUDE.md`, `.claude/`, hooks, or symlinks.
- **Safe, idempotent update.** When `./workflow/` already exists, never
  overwrite user-written text. Fill only missing sections, keep existing
  `PLAN.md` entries and tails, and add a tail only for an artifact that is
  genuinely absent.
- **Facts, not guesses.** Record in `PROJECT.md` only what is observable in
  project files. Mark anything undetermined as `Требует уточнения` instead of
  inventing it.
- **Current state only.** `PROJECT.md` and `PLAN.md` describe the project as it
  is now. No biography, no decision logs, no "previously X, now Y" comparisons.
- Write this skill's instruction text in English; write the artifacts
  (`PROJECT.md`, `PLAN.md`) in the user's working language. Keep tool names,
  paths, framework names, commands, and code identifiers in their original
  spelling.
- Do not call downstream skills automatically. Stop once the structure,
  `PROJECT.md`, and `PLAN.md` are ready.

## Steps

For this multi-step procedure, use the agent's task planning mode (todo list /
task plan, whichever is available) and close items one by one.

### 1. Detect the current `./workflow/` state

Check whether `./workflow/` and its files already exist:
`./workflow/PROJECT.md`, `./workflow/PLAN.md`, `./workflow/VISION.md`,
`./workflow/ROADMAP.md`, `./workflow/ARCHITECTURE.md`, `./workflow/DESIGN.md`,
and the `./workflow/archive/` and `./workflow/features/` directories.

- If nothing exists, this is a fresh bootstrap: create everything.
- If `./workflow/` exists, this is an idempotent update: read existing files,
  preserve their content, and only complete what is missing.

### 2. Collect technical context

Inspect the project root to determine the tech stack. Read sources in this
priority order; later sources only fill gaps the earlier ones leave:

1. package manifests and lock files (e.g. `package.json`, `pyproject.toml`,
   `go.mod`, `Cargo.toml`, `pom.xml`, and their lock files);
2. language and build configs (compiler, bundler, framework configs);
3. `Dockerfile`, Compose files, devcontainer, and CI files;
4. test, lint, and format configs;
5. environment templates (`.env.example` and similar);
6. `README.md` and `docs/` as supporting context only, never as the source of
   truth;
7. the user, for facts that no file reveals.

Record only observable facts. If a critical fact (stack, run command, key
services) cannot be determined from files, prefer writing `Требует уточнения`
in `PROJECT.md` and adding a clarification tail in `PLAN.md`. Ask the user at
most a few short questions, and only for facts that genuinely block a usable
technical reference — do not interrupt for minor gaps.

### 3. Create the `./workflow/` structure

Create any missing directories: `./workflow/`, `./workflow/archive/`, and
`./workflow/features/`. Leave existing directories and their contents untouched.

### 4. Write `./workflow/PROJECT.md`

Create `./workflow/PROJECT.md` from the section canon in *Artifact
Requirements*, or update an existing one.

When updating: keep all user-written text, fill only sections that are missing
or empty, and do not rewrite sections that already hold valid content. Replace a
section only when project files clearly contradict what it says.

### 5. Create or update `./workflow/PLAN.md`

Create a base `./workflow/PLAN.md` if it is missing, or update the existing one,
following *Updating PLAN.md* below. Add routing tails for the project stages
whose artifacts do not yet exist.

## Artifact Requirements

### `./workflow/PROJECT.md`

`PROJECT.md` is the technical reference the next agents read instead of
re-deriving the stack. Write it so a later agent can start without chat history:
where the project is, how to run it, how to verify a change, which tools are
already chosen, and which decisions are still open.

Use these sections; fill each only with observable facts. Leave a section short
or mark it `Требует уточнения` when data is missing — do not pad it.

- **Project** — name and type, when reliably identified.
- **Stack** — languages, frameworks, and their versions.
- **Package manager and commands** — the package manager and the main commands.
- **Run locally** — how to start the project locally.
- **Tests** — how to run tests.
- **Build** — how to build the project.
- **Lint / format / typecheck** — how to check changes.
- **Environment and external services** — env variables and external services.
- **Docker / CI / deploy** — only when such files are present.
- **Open questions** — facts not determined from files; each becomes a
  clarification tail in `PLAN.md`.

Keep it tight: every line is a fact a later agent can act on. No universal best
practices, no narrative project history, no time estimates, no team or
onboarding content.

### `./workflow/PLAN.md`

`PLAN.md` is a light status index: a feature list plus service tails for the
next project stages. On a fresh bootstrap there are no features yet, so create
it with an empty feature list and the routing tails. Use this base shape when
the file has no stronger local pattern:

```md
# PLAN

## Features

<empty until the feature skill adds entries>

## Tails

- [ ] roadmap — ...
- [ ] architecture — ...
```

Feature status markers used across the workflow: `[ ]` new, `[-]` planned,
`[+]` split into tasks, `[x]` done, `[*]` tested, `[/]` archived. `initialize`
does not add feature entries — it only writes the structure and the tails.

## Updating PLAN.md

At the end, ensure `./workflow/PLAN.md` carries a service tail for each next
project stage whose artifact is missing:

- a `roadmap` tail if `./workflow/VISION.md` or `./workflow/ROADMAP.md` is
  absent;
- an `architecture` tail if `./workflow/ARCHITECTURE.md` is absent;
- a `design-guideline` tail if `./workflow/DESIGN.md` is absent;
- a technical-clarification tail if `PROJECT.md` has entries under
  **Open questions**.

Preserve existing feature entries, statuses, and tails. Do not add a duplicate
tail for an artifact that already exists or for a tail already present. Do not
change any feature status — `initialize` is not a feature stage.

## Notes

- Missing input files do not stop this skill. Proceed on whatever project files
  are available and record the rest as open questions.
- When `./workflow/` already exists, treat every file as input and merge
  conservatively: complete the missing parts, never discard user content.
- The project's own `README.md` is a supporting hint only; never treat it as the
  authority on the stack.
- The skill is ready when the project has a working `./workflow/` structure, a
  fact-based technical reference in `PROJECT.md`, and a `PLAN.md` listing the
  next open project stages.
