---
name: docs
description: >
  Creates or updates the project's user and technical documentation — the root
  README.md and pages under ./docs/concept, ./docs/spec, ./docs/tech —
  recording the current state of the product. Use when the user says "write
  documentation", "update documentation", "document the project", or "document
  a feature".
---

# Docs — Record the Current State of the Product

## Purpose

This skill writes and maintains the project's user and technical
documentation: what the product is for, how it behaves, and how to build and
run it. A reader understands the project from the docs alone, without knowing
its development history.

It runs late in the feature pipeline, usually after `test`. It owns only the
root `README.md` and the pages under `./docs/concept/`, `./docs/spec/`, and
`./docs/tech/`. When it documents one feature, it also reconciles the
neighboring owned pages that the new state contradicts, so the documentation
stays a true statement about the product. In feature-update mode it marks the
documented feature `[/]` archived in `./workflow/PLAN.md`.

It does not write product code, tests, audit reports, or feature `./workflow/`
artifacts — those belong to `implement`, `test`, and `audit`.

## Parameters

Optional `args`:

```txt
[<feature-slug>]
```

- `<feature-slug>` under `./workflow/features/{slug}/` — feature-update mode:
  document the implemented behavior of that feature.
- No args — infer the mode: with no docs present, document the whole project
  from scratch; with docs present, ask one short question whether the user
  wants a whole-project pass or a feature update with a slug.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty tree is expected.
- Document the current state only, in the present tense. No changelogs,
  release notes, version history, "previously/now" phrasing, or
  breaking-change markers — superseded text is replaced, not annotated.
- Document only behavior that the code, config, or runnable commands support.
  Existing docs are claims to verify against the code, not truth by default.
- Stay inside the ownership boundary: the root `README.md` and `./docs/`. Do
  not rewrite `feature.md`, `plan.md`, `design.md`, or `tests.md`; do not
  write code or tests.
- One canonical home per fact and one term per concept across all docs; other
  pages link instead of duplicating.
- `./workflow/PLAN.md` writes are limited to appending docs tails and, in
  feature-update mode, setting the documented feature to `[/]`.

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

This is a multi-step flow; use task planning mode with one item per step.

1. **Resolve the mode.** Parse `args` per Parameters. In feature-update mode,
   confirm `./workflow/features/{slug}/feature.md` exists; if it is missing,
   stop and ask for the correct slug.

2. **Read the context.** Read what exists of the project canon:
   `./workflow/PROJECT.md` for stack and run commands, `./workflow/VISION.md`
   and `./workflow/ROADMAP.md` for purpose and direction. In feature-update
   mode also read the feature's `feature.md`, plus `plan.md`, `design.md`, and
   `tests.md` when present.

3. **Scan code and current docs.** Read the implemented code in scope —
   modules, entry points, commands, behavior — and the current owned docs.
   Code, config, routes, schemas, and runnable commands are the source of
   truth for what exists; feature artifacts carry intent and acceptance
   context.

4. **Reconcile.** Check existing owned docs against the live code: names,
   behavior, commands, config. In feature-update mode, also sweep the
   neighboring owned pages for statements the new feature state contradicts —
   they are part of the scope. Plan in-place fixes that replace stale text
   rather than adding corrections beside it.

5. **Plan the docs.** Decide which pages to create or update and where each
   belongs: `./docs/concept/` answers why (purpose, ideas, value),
   `./docs/spec/` answers what (behavior, contracts, reference),
   `./docs/tech/` answers how (structure, setup, run, extend). Name the reader
   and the task each page serves, the canonical home for each fact, the text
   to delete as stale or duplicate, and the gaps that become docs tails. If
   open questions remain that the user can resolve — audience, depth,
   conflicting sources — ask them all in one batch before writing, with the
   recommended option first and marked "(Recommended)".

6. **Write the docs.** Create or update `README.md` and the planned `./docs/`
   pages per Artifact Requirements. Keep the structure of existing pages and
   fix mismatches in place. Remove empty sections, stale TODOs, and
   placeholders that no longer help the reader. Unsupported behavior stays out
   of the docs; record a docs tail only when a real documentation need
   remains.

7. **Self-check.** Verify the result: names and behavior match the
   implementation, terminology is consistent, each fact lives in one place,
   every kept paragraph answers a current reader's need, and no page requires
   history or outside explanation to be understood.

8. **Closing sweep.** Walk the touched pages and normalize them with the
   `markov` skill: run it on the affected docs via the skill mechanism when
   available, otherwise apply the same discipline manually. The result reads
   as if the product had always been in its current form — no biography, no
   deltas, no orphaned sections.

9. **Update PLAN.md and report.** Append docs tails for needed documentation
   the current material cannot support; tail wording states the missing doc,
   not how the gap was found. In feature-update mode, set only the documented
   feature to `[/]`. Report briefly: the mode, pages created or updated,
   mismatches fixed, tails appended, and the status change. End with the
   next-step recommendation: the feature branch is complete here; when
   `./workflow/PLAN.md` shows other features awaiting work, name the next one
   and the skill its status calls for, otherwise say the pipeline is clear.

## Artifact Requirements

### Root `README.md`

The front door: project name, short purpose, quick start or usage example,
configuration and run. Concise, current, accurate.

### `./docs/` pages

- `./docs/concept/*.md` — purpose, core ideas, value.
- `./docs/spec/*.md` — behavior and contracts, a precise lookup reference.
- `./docs/tech/*.md` — structure, setup, run, extend.

Each page serves one reader and one task; do not split a single topic across
explanation, reference, and instruction pages. Add a page or section only when
it carries information the reader needs — a few accurate paragraphs are a
valid complete page. Present tense, current state only; one canonical home per
fact, with links instead of duplicated detail; a diagram only when it
clarifies.

## Updating PLAN.md

Status markers: `[ ]` new, `[-]` planned, `[+]` split into tasks, `[x]`
implemented, `[*]` tested, `[/]` archived. This skill appends docs tails and,
in feature-update mode, changes only the documented feature's line to `[/]`.
From-scratch mode changes no feature status. If `PLAN.md` is missing, list the
gaps in the report instead.

## Notes

- Partial input is fine: with `PROJECT.md`, `VISION.md`, or `ROADMAP.md`
  missing, work from the available context and the code.
- When documenting tested behavior, describe the current guarantees and active
  gaps, not the incidents that motivated the tests.
