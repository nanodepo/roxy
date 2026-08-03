---
name: feature
description: >
  Creates a self-contained feature brief from a raw request in
  ./workflow/features/{slug}/feature.md and registers it in ./workflow/PLAN.md.
  Use for "new feature", "capture feature", "write feature brief", or "start
  feature".
---

# Feature — Capture a Feature Brief

## Purpose

This skill turns a raw feature request into a self-contained brief at
`./workflow/features/{slug}/feature.md` and registers the feature in
`./workflow/PLAN.md` with status `[ ]`. It opens the feature branch of the
pipeline: it records what the user wants and why, the expected result, and the
boundaries — richer than a one-line idea, smaller than a PRD or a plan.

The brief explains the feature to a future planner or implementer who has no
access to this conversation. It guides rather than dictates: its size is
proportional to the feature's complexity, and a few well-written sentences are
a complete brief for a simple feature.

The skill does not decide implementation, architecture, UI design, tests,
documentation, or task breakdown — those belong to later stages.

## Parameters

`args` is the raw feature request:

```txt
<feature request>
```

- `args` present → the full string is the request.
- `args` empty → infer the request from the current user message.
- No identifiable feature in either → ask the user for one before doing
  anything else.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty tree is expected.
- Own only `./workflow/features/{slug}/` with its `feature.md` and the matching
  entry in `./workflow/PLAN.md`. Do not write `plan.md`, `design.md`,
  `tests.md`, `NN-task.md`, product code, or documentation.
- Do not invoke downstream skills. Finish with the brief, the `PLAN.md` entry,
  and a next-step recommendation in chat.
- Ask the user only questions whose answers change the brief — the essence,
  the problem, the user or context, the expected result, or a scope boundary.
  Do not ask implementation, architecture, design, test, or docs questions.
- Close every open question before writing the brief. The written `feature.md`
  contains no unresolved questions.
- Describe current intent only: no biography, no deltas, no migration notes,
  no references to removed behavior.

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

Use task planning mode (todo list) with one item per step.

1. **Capture the request.** Take it from `args` or the current message.
   Extract the asked capability or change, the problem behind it when stated,
   the affected user or context, the expected result in the user's words, and
   any explicit limits or exclusions. If the request mixes several unrelated
   features, ask which one to capture — one `feature.md` is one coherent
   feature.

2. **Research.** Read only the workflow context that helps understand the
   feature's meaning: `./workflow/PROJECT.md`, `./workflow/VISION.md`,
   `./workflow/ROADMAP.md`, `./workflow/PLAN.md`, and `./workflow/DESIGN.md`
   when the request touches UI. Check `./workflow/PLAN.md` entries and
   `./workflow/features/` for an existing feature covering the request: a
   duplicate → stop and report the existing slug; a distinct extension →
   continue and note the relationship. Do not scan product code unless the
   request names a concrete code area that the workflow files cannot explain.
   While researching, collect every question that the request and the context
   cannot answer — do not ask them one by one.

3. **Ask the accumulated questions in one batch.** First resolve what you can
   from the request and the files read. Then ask the remaining questions —
   typically up to three — in a single message. For each question put the
   recommended answer first, marked `(Recommended)` with a short reason, and
   offer only materially different alternatives. If nothing blocks the brief,
   skip this step.

4. **Synthesize.** With the answers in hand, settle the brief's content: the
   essence and motivation, the expected result, what is in and out of scope,
   and any context or terms a future reader genuinely needs. Pick a stable
   `kebab-case` slug (2–5 words, lowercase Latin letters, digits, hyphens)
   named after the domain intent, not an implementation detail; if the slug is
   taken, add a meaningful qualifier rather than a number.

5. **Write the brief.** Create `./workflow/features/{slug}/` and write
   `feature.md` per Artifact Requirements.

6. **Register in PLAN.md.** Add the feature line per Updating PLAN.md.

7. **Report.** Summarize the brief in chat and recommend the next step: a
   simple feature goes straight to `implement` (no plan needed); a UI feature
   with non-trivial UX goes to `design` (even when no plan is needed);
   everything else goes to `planning`. One or two lines, naming the skill.

## Artifact Requirements

`./workflow/features/{slug}/feature.md` is a human-readable narrative brief
written in prose, not a questionnaire. Core sections:

- **What and why** — the essence of the feature and the motivation behind it:
  the problem or need, the affected user or context.
- **Expected result** — what becomes true when the feature is done, in product
  terms, without acceptance criteria or test plans.
- **Boundaries** — what is in scope and what adjacent work is deliberately
  out, with the reason when it is not obvious.

Add another section — context, domain terms, relation to an existing feature —
only when it carries a decision a future reader needs. Do not pad a simple
feature: a brief of a few sentences is complete when the essence, result, and
boundaries are clear.

The brief is ready when `planning` or `implement` can proceed from it without
this conversation's history.

## Updating PLAN.md

Add one entry with status `[ ]`. If `./workflow/PLAN.md` exists, preserve its
structure and statuses, place the entry near other active features, and do not
rewrite other entries or duplicate the slug. If it is missing, create a minimal
`PLAN.md` with only the new entry — no other canon files.

Default entry shape, unless the local pattern differs; localize visible labels:

```md
- [ ] `{slug}` - <short feature title>
  - brief: `./workflow/features/{slug}/feature.md`
```

## Notes

- Missing `PROJECT.md`, `VISION.md`, `ROADMAP.md`, or `DESIGN.md` is fine —
  work with the context that exists. Create `./workflow/features/` if absent.
- A request that is really planning, design, testing, docs, or implementation
  for an existing feature → report the matching downstream skill instead of
  creating a new brief.
