---
name: design
description: >
  Creates feature-level UI/UX design docs in ./workflow/features/{slug}/design.md
  for features with new or non-trivial UX. Use for "design feature", "create
  feature design", "write design.md", or "spec UI for feature".
---

# Design — Feature UI/UX Decisions

## Purpose

This skill records the UI/UX decisions of one feature in
`./workflow/features/{slug}/design.md`, so that `implement` can build the
interface without a second design pass. It applies the project's visual canon
(`./workflow/DESIGN.md`) and existing frontend patterns to a concrete feature;
it does not invent a local design system and does not change the canon.

The skill is selective: it runs only for features whose UX is genuinely new or
non-trivial. The default result is a short `design.md` — a few sentences naming
the components to use (including shadcn primitives where the project has them)
and the page structure. That short form is a complete, finished artifact, not a
draft. Expanded sections appear only when the feature introduces UX complex
enough to need them, and only the sections that carry real decisions.

## Parameters

`args` is the feature slug:

```txt
<slug>
```

- `<slug>` names a folder under `./workflow/features/{slug}/`.
- Empty or ambiguous `args` → list the folders under `./workflow/features/`
  and ask the user to pick one.
- `feature.md` missing → stop and name the missing upstream artifact.
- The feature does not affect the UI → stop and explain that `design` runs
  only for UI-affecting features.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty tree is expected.
- Own only `./workflow/features/{slug}/design.md`, plus a service tail in
  `./workflow/PLAN.md` when a missing or weak design canon blocks the feature
  design. Do not edit product code, tests, `./workflow/DESIGN.md`,
  `feature.md`, or `plan.md`.
- Treat `./workflow/DESIGN.md` as the visual and interaction canon. Reuse
  existing frontend components, layouts, and tokens when they fit.
- Size the artifact to the feature: do not expand a simple feature into
  scenario tables and component contracts. A section exists only if it carries
  a decision.
- Describe only current UI decisions, in the present tense — no biography,
  deltas, or notes about removed behavior.
- No code beyond component and identifier names that point at existing
  reusable patterns.

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

1. **Resolve the feature.** Read `./workflow/features/{slug}/feature.md`, the
   existing `design.md` if any, and `plan.md` if the feature has one (`design`
   may run straight after `feature` when no plan is needed). Read
   `./workflow/DESIGN.md` for the visual canon; if it is missing or too weak
   to answer the feature's questions, note the gap and continue — do not
   replace canon with taste.

2. **Inspect frontend patterns.** Read only the frontend files needed to see
   the project's conventions: related pages, shared layouts, the components
   the feature will touch, and the component-library or theme configuration.
   Record what can be reused. Do not run builds, codegen, or any mutating
   commands.

3. **Decide the design.** Work out the user's goal, the primary scenario, the
   page structure, and which existing components cover it. Judge honestly
   whether the UX is genuinely new: most features compose known patterns, and
   for them the short default form is the right answer. Identify only the
   states and interactions that are non-obvious for this feature — do not
   enumerate default/loading/empty/error mechanically when the project's
   patterns already define them.

4. **Ask the user only blockers.** Resolve questions from local documents and
   code first. When `design.md` would otherwise require arbitrary invention,
   ask the remaining questions in one batch, each with the recommended option
   first, marked `(Recommended)` with a short reason. The written artifact
   carries no open questions.

5. **Write `design.md`.** Create or update
   `./workflow/features/{slug}/design.md` per Artifact Requirements. When
   updating, rewrite it as the current design of the feature — remove stale
   decisions and anything that no longer holds.

6. **Report and recommend.** Summarize the design decisions in chat. End with
   a next-step recommendation: back to the main branch — `task` when the plan
   is large and multi-phase, `improve` when the feature is complex or
   architecturally important and the plan deserves a second pass, otherwise
   `implement`.

## Artifact Requirements

`./workflow/features/{slug}/design.md` is self-contained and follows the
Language Notice.

The default form — sufficient for most features:

```md
# Design: <feature name>

A few sentences: what the user does on this screen, which existing components
(including shadcn primitives) implement it, and the page structure — zones,
primary action, where the feature plugs into existing layout. Name the one or
two states or interactions that are non-obvious, if any.
```

Expand beyond this only when the feature introduces genuinely new, complex UX,
and add only the sections that carry decisions: a scenario walkthrough when
the flow is non-linear, a states breakdown when states behave unusually, a
component contract when the feature introduces or changes a reusable
component, content rules when copy is constrained. Never add a section to look
complete.

## Updating PLAN.md

If `./workflow/PLAN.md` exists, keep the feature's status marker unchanged —
the design stage has no marker of its own. Optionally add one line under the
feature:

```md
- design: `./workflow/features/{slug}/design.md`
```

If a missing or weak `./workflow/DESIGN.md` blocked or weakened the feature
design, append a service tail naming the gap and why it matters — current
state, actionable, no discovery history. If `PLAN.md` is absent, do not create
it; mention this in the final report.
