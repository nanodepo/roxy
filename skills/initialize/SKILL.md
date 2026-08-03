---
name: initialize
description: >
  Bootstraps project workflow: creates ./workflow/, records tech context in
  ./workflow/PROJECT.md, seeds ./workflow/PLAN.md tails. Triggers: initialize
  project, bootstrap project, set up workflow, prepare project for work.
---

# Initialize — Bootstrap the Project Workflow

## Purpose

This skill gives the agent workflow its home. It creates the `./workflow/`
structure, records the project's technical context in `./workflow/PROJECT.md`,
and seeds `./workflow/PLAN.md` with routing tails for the canon stages that do
not exist yet. It is the first stage of the pipeline.

The bootstrap is cheap and safe: it creates the place for canon without making
product, architecture, or design decisions. Writing `VISION.md`, `ROADMAP.md`,
`ARCHITECTURE.md`, or `DESIGN.md`, choosing patterns, and creating features
belong to later stages — initialize only routes to them through `PLAN.md`
tails.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty tree is expected.
- Stay inside the boundary: do not write `VISION.md`, `ROADMAP.md`,
  `ARCHITECTURE.md`, or `DESIGN.md`, do not select an architecture, describe
  design, or create features. Route those stages as `PLAN.md` tails only.
- Do not touch the application: no scaffolding of app code, `.gitignore`,
  `.env.example`, `README.md`, and no edits to `AGENTS.md`, `CLAUDE.md`,
  `.claude/`, hooks, or symlinks.
- Be idempotent and conservative. When `./workflow/` already exists, never
  overwrite user text: fill only missing pieces and keep existing `PLAN.md`
  entries and tails.
- Record observable facts only. When a fact cannot be confirmed from project
  files, write `Needs clarification` instead of guessing.
- Write the present state. `PROJECT.md` and `PLAN.md` carry no bootstrap notes,
  decision logs, or "previously/now" comparisons.
- Do not call downstream skills. Stop once the structure, `PROJECT.md`, and
  `PLAN.md` are ready, and recommend the next step in chat.

## Language Notice

Write user-facing chat output and generated or rewritten project artifacts in
the working language of the target project. Detect it from existing
`./workflow/` files, project documentation, and the user's request. If the
project language is unclear, use the user's current language.

When editing an existing artifact, preserve its language unless the user
explicitly asks to translate it.

Apply the chosen artifact language to all prose, headings, table headers,
labels, placeholders, and examples. Keep file paths, commands, tool names, code
identifiers, framework names, package names, status markers, and established
product terms in their original spelling.

Do not mix languages inside one artifact unless the existing project canon
already does so or a quoted or source term requires it.

## Steps

Use task planning mode (todo list) for this multi-step flow.

1. **Detect the workflow state.** Check which of these exist: `./workflow/`,
   `PROJECT.md`, `PLAN.md`, `VISION.md`, `ROADMAP.md`, `ARCHITECTURE.md`,
   `DESIGN.md`, `archive/`, `features/`. Nothing exists → fresh bootstrap.
   `./workflow/` exists → idempotent update: read the existing files and
   complete only what is missing.

2. **Collect the technical context.** Inspect the project root. Trust current
   executable and configuration files over prose, in this order: package
   manifests and lock files; language, build, and framework config;
   Dockerfiles, Compose, devcontainer, CI; test, lint, and format config; env
   templates. Use `README.md` and `docs/` only to discover possibilities, then
   verify them against files or mark them as open questions. Ask the user only
   for facts no file reveals that block a usable tech reference — few, short,
   and in one batch before writing anything.

3. **Create the missing directories**: `./workflow/`, `./workflow/archive/`,
   `./workflow/features/`. Leave existing content untouched.

4. **Write `./workflow/PROJECT.md`** per Artifact Requirements. On update,
   keep user text and fill gaps; replace a section only when project files
   clearly contradict it.

5. **Create or update `./workflow/PLAN.md`** per Updating PLAN.md.

6. **Report.** Summarize the structure and facts now in place, list open
   questions, and end with the next-step recommendation: name the first
   missing canon stage — `roadmap` when `VISION.md`/`ROADMAP.md` is absent,
   `architecture` when `ARCHITECTURE.md` is absent, `design-guideline` when
   `DESIGN.md` is absent. When the canon is complete, recommend `feature`.

## Artifact Requirements

### `./workflow/PROJECT.md`

The current technical reference: a later agent runs, tests, and builds the
project from this file alone, without re-deriving the stack or reading chat
history. Core sections: stack; commands (run, test, build, lint/format/
typecheck); environment and external services; open questions. Add a further
section only when it carries a fact an agent will act on — for example Docker,
CI, or deploy when those files exist. Every line is an observable fact or a
`Needs clarification` mark; no best-practice filler and no padding.

### `./workflow/PLAN.md`

A light status index: the status legend, the feature list, and service tails.
A fresh bootstrap creates the legend, an empty feature list, and routing tails;
initialize adds no feature entries. Base shape when no stronger local pattern
exists (translate visible headings and placeholders):

```md
# PLAN

## Statuses

- `[ ]` new
- `[-]` planned
- `[+]` split into tasks
- `[x]` implemented
- `[*]` tested
- `[/]` archived

## Features

## Tails

- [ ] roadmap — ...
```

## Updating PLAN.md

Ensure a service tail exists for each missing canon artifact: a `roadmap` tail
when `VISION.md` or `ROADMAP.md` is absent, an `architecture` tail when
`ARCHITECTURE.md` is absent, a `design-guideline` tail when `DESIGN.md` is
absent, and a tech-clarification tail when `PROJECT.md` has open questions.
Ensure the `## Statuses` legend exists. Preserve existing entries, statuses,
and tails; never duplicate a tail or add one for an artifact that already
exists. A tail names an active missing artifact or fact, not a record of what
was created. Do not change feature statuses.

## Notes

- Missing input files do not stop the skill: use the project files that exist
  and record the rest as open questions.
- Ready state: the `./workflow/` structure exists, `PROJECT.md` is a fact-based
  tech reference, and `PLAN.md` lists the open project stages.
