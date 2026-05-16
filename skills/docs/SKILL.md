---
name: docs
description: >
  Creates or updates the project's user and technical documentation — the root
  README.md and pages under ./docs/concept, ./docs/spec, ./docs/tech — recording
  the current state of the product. Use when the user says "напиши документацию",
  "обнови документацию", "задокументируй проект", "задокументируй фичу", or asks
  to document implemented behavior.
---

# Docs

## Purpose

`docs` writes and maintains the project's user and technical documentation. It
records the current state of the product — what it is for, how it behaves, how
it is built and run — so a reader understands the project without reading the
history of how it was developed.

It runs late in the pipeline, normally after `test`, when behavior is already
implemented and verified. It reads the project canon and feature artifacts but
does not own them.

It owns the root `README.md` and the pages under `./docs/concept/`,
`./docs/spec/`, and `./docs/tech/`. It does not write an audit report, does not
change product code, does not write tests, does not rewrite a feature's
`./workflow/` artifacts, and performs no git operations. Recording mismatches as
recommendations belongs to `audit`; `docs` fixes documentation and only notes
missing documents as docs tails.

## Parameters

The `args` string is optional:

```
[<feature-slug>]
```

- `<feature-slug>` — the slug of a feature under `./workflow/features/{slug}/`.
  When given, the skill runs in feature-update mode: it documents the behavior
  of that implemented feature.
- No argument → the skill determines the mode from the project state (see
  Step 1).

## Strict Rules

- **No git operations.** Do not check status, diff, branch, commit, or push. The
  working tree is expected to be dirty; the user manages git.
- **`mcp__sequential-thinking__sequentialthinking` is required** at the analysis
  step (Step 5). Deciding which documents to create or update, for which reader,
  and with what content is analytical work and must not be improvised.
- **Current state, not history.** Every document describes the product as it is
  now, in the present tense. Never write changelogs, release notes, version
  history, "previously X, now Y" comparisons, or breaking-change markers. A
  superseded statement is simply replaced.
- **Stay inside the boundary.** Do not edit product code, do not write tests, do
  not produce an audit report, and do not rewrite a feature's `feature.md`,
  `plan.md`, `design.md`, or `tests.md`. The only allowed write to
  `./workflow/PLAN.md` is appending docs tails.
- Write this skill's text in English; write the documentation itself in the
  project's working language. Keep tool names, paths, commands, and code
  identifiers in their original spelling.
- Use one term for one concept across all documents.

## Steps

This is a multi-step procedure with a nested analytical call. At the start, open
the agent's task planning mode (todo list / task plan, whichever is available)
with the steps below and close them one by one.

### 1. Parse args and determine the mode

Split the `args` string by spaces.

- One token → treat it as `<feature-slug>`. Confirm
  `./workflow/features/{slug}/feature.md` exists. If it does not, stop and ask
  the user for the correct slug. Run in **feature-update mode**.
- No token → inspect the project: does a root `README.md` exist, does `./docs/`
  exist with content?
  - No documentation at all → run in **from-scratch mode**: document the whole
    project.
  - Documentation exists → the intent is ambiguous. Ask the user one question:
    document the whole project, or update documentation for one feature (and
    which). Run the chosen mode.

### 2. Read project context

Read whichever of these exist; skip silently if missing:

- `./workflow/PROJECT.md` — tech stack, run, deploy.
- `./workflow/VISION.md` — ideological vision.
- `./workflow/ROADMAP.md` — development goals.

In feature-update mode, also read from `./workflow/features/{slug}/`:

- `feature.md` — the feature description (required for this mode).
- `plan.md`, `design.md`, `tests.md` — read whichever exist.

### 3. Scan the code and the current documentation

Read the implemented code that the documentation must describe — the modules,
entry points, commands, and behavior in scope. Then read the current
documentation: the root `README.md` and the pages under `./docs/concept/`,
`./docs/spec/`, and `./docs/tech/`.

The code is the source of truth. Feature artifacts state intent; the code states
what the product actually does.

### 4. Reconcile existing documentation with the live code

For every document that already exists, check its statements against the live
code: names, signatures, behavior, commands, configuration. Mark each statement
as confirmed or as a mismatch to fix. Preserve the existing structure of each
document — plan to correct mismatches in place, not to rewrite the document from
nothing. In from-scratch mode with no existing documentation, skip this step.

### 5. Analyze and plan the documents (sequential-thinking)

Call `mcp__sequential-thinking__sequentialthinking` to work through:

- **Which documents to create or update**, and where each belongs across the
  three folders by what it answers:
  - `./docs/concept/` — explanation: the purpose, ideas, and value of the
    product (why it exists);
  - `./docs/spec/` — reference: behavior and contracts a reader consults for a
    precise answer (what it does);
  - `./docs/tech/` — structure and operation: how the project is built and how
    to work with it (how it is built and how to run it).
  Do not split a single document across explanation, reference, and instruction.
  Do not introduce the full four-quadrant documentation taxonomy on top of these
  three folders.
- **The reader and task of each document** before writing it: a concept page
  serves a reader who wants the "why"; a spec page serves a reader hunting a
  precise answer about behavior; a tech page serves a reader deploying and
  extending the project. Content is chosen for that task.
- **The root `README.md`** — what it must cover (see Artifact Requirements).
- **Mismatches from Step 4** — which statements to correct.
- **Gaps** — documents the project needs but the current material cannot fully
  support; these become docs tails.

Fix the result as a structured documentation plan.

### 6. Write the documentation

Following the plan from Step 5:

- Create or update the root `README.md` against the section checklist in
  *Artifact Requirements*.
- Create or update the pages under `./docs/concept/`, `./docs/spec/`, and
  `./docs/tech/`, each addressed to the reader and task fixed in Step 5.
- When updating an existing document, keep its structure and correct mismatches
  in place; do not rewrite it from scratch.
- Write every document as the current state — no history, no deltas, no
  temporary notes.

### 7. Self-check before finishing

Verify every created or updated document:

- **Factual accuracy** — code, names, and behavior match the implementation.
- **Clarity** — the language is plain and unambiguous.
- **Consistent terminology** — one term per concept across all documents.
- **No duplication** — a fact lives in one document; others link to it rather
  than restating it.
- **Self-contained present** — the document reads on its own, with no project
  history or external explanation.

### 8. Append docs tails to `./workflow/PLAN.md`

Append to `./workflow/PLAN.md` only docs tails — documents the project still
needs that the current material could not support. Do not add feature entries
and do not change feature statuses. If `./workflow/PLAN.md` is missing, list the
gaps in the report instead.

### 9. Report

Give a short report: the mode used, the documents created or updated, the
mismatches corrected, and any docs tails appended to `./workflow/PLAN.md`.

## Artifact Requirements

### Root `README.md`

The root `README.md` covers, at minimum:

- the project name;
- a short description of what the project is for;
- a quick start or usage example;
- configuration and how to run.

Keep it readable as the project's front door — concise, current, and accurate.

### `./docs/` pages

- `./docs/concept/*.md` — explanation: purpose, core ideas, the value the
  product delivers.
- `./docs/spec/*.md` — reference: behavior and contracts, precise and lookup-
  oriented.
- `./docs/tech/*.md` — structure and operation: how the project is built, how to
  set it up, run it, and extend it.

Format rules for every document:

- Write only the current state, in the present tense.
- No change history, no release notes, no version markers, no "previously /
  now" comparisons.
- No operational chatter — every line helps the reader understand the project.
- A diagram is optional — add one only when it clarifies the text; never make it
  a required element.

## Updating PLAN.md

At the end, write to `./workflow/PLAN.md` only docs tails — missing documents the
project still needs. Do not add feature entries and do not modify feature
statuses; documentation is not a feature.

## Notes

- The skill works with partial input: if `PROJECT.md`, `VISION.md`, or
  `ROADMAP.md` is missing, proceed on the available context and the code.
- When existing documentation contradicts the live code, the code wins — correct
  the document.
- In feature-update mode, the feature artifacts describe intent; always confirm
  the documented behavior against the implemented code.
- The skill follows these steps literally and does not shorten them. The
  documentation it produces must be readable without knowing how or when it was
  written.
