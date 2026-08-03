---
name: roadmap
description: >
  Creates or updates the project's vision and roadmap canon in
  ./workflow/VISION.md and ./workflow/ROADMAP.md. Triggers: create roadmap,
  update roadmap, define project vision, set product direction, prioritize
  goals.
---

# Roadmap — Record the Project Direction Canon

## Purpose

This skill records the project's current meaning, goals, and priorities as
canon. It creates or updates `./workflow/VISION.md` and `./workflow/ROADMAP.md`
and maintains roadmap service tails in `./workflow/PLAN.md` for unresolved
project-level direction questions. Later stages read these files for purpose,
audience, priorities, constraints, and vocabulary instead of re-deriving them.

It runs after `initialize` and before feature-level work. The developer's
stated direction is the primary input; local files, docs, and code are
evidence that confirms, constrains, or challenges it — never a silent
replacement for it.

The skill does not write `PROJECT.md`, choose architecture, create feature
briefs, write PRDs, plan feature implementation, create backlog or release
plans, or write code and tests.

## Parameters

`args` is an optional roadmap intent: a goal, a priority change, or a product
direction. When `args` is empty and the user's message does not state the
direction, elicit it before reading the codebase in depth.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty tree is expected.
- Own only `./workflow/VISION.md`, `./workflow/ROADMAP.md`, and roadmap
  service tails in `./workflow/PLAN.md`. Do not touch feature folders,
  architecture or design files, project tech files, or source code.
- Write both documents as current canon: present tense, no change history,
  decision biography, or "previously/now" comparisons. Superseded goals and
  priorities are replaced, not preserved as context.
- Keep the roadmap outcome-focused: items explain the result and rationale,
  not just a feature name. Separate confirmed facts from assumptions.
- Treat the user-stated stage, goals, horizon, and priorities as the leading
  strategic input. When local evidence conflicts with it, ask instead of
  silently choosing the local inference.
- Ask only direction questions (users, goals, horizon, constraints, priority,
  success signals) — never implementation, architecture, UI, or test
  questions. Batch them before writing.

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

1. **Read the minimal project context.** Read `./workflow/PROJECT.md`,
   `VISION.md`, `ROADMAP.md`, and `PLAN.md` if they exist; read
   `ARCHITECTURE.md` or `DESIGN.md` only when they constrain product
   direction. Missing files do not stop the skill. Treat existing direction
   docs as candidates for the current canon, not history to preserve.

2. **Elicit the user's direction.** If the request does not already answer it,
   ask concise open-ended questions covering only the blocking inputs: current
   stage, near- and longer-term direction, goals, target users, horizon,
   constraints, success signals, explicit "not now" items. The developer knows
   project intent better than code can infer it.

3. **Validate against local evidence.** Inspect code, docs, and feature briefs
   only as far as needed to confirm, constrain, or challenge the stated
   direction. Note conflicts and gaps; do not turn implementation detail into
   roadmap output.

4. **Synthesize the direction.** Reason through: what the project exists to
   do and for whom; the most important problem; outcomes versus outputs;
   sequencing across Now / Next / Later given value, effort, risk, and
   dependencies; what current statements to keep, rewrite, or delete; which
   unresolved project-level decisions become roadmap tails.

5. **Ask reconciliation questions.** Resolve open points from the intake
   answers and local evidence first. Then ask the user at most a few concise
   questions in one batch — only those whose answer changes purpose, users,
   priority, horizon, constraints, or success signals. For option questions,
   put the recommended option first marked `(Recommended)` with a short
   reason. A non-blocking question becomes an assumption or a roadmap tail,
   not an interruption.

6. **Write `./workflow/VISION.md` and `./workflow/ROADMAP.md`** per Artifact
   Requirements. Replace stale content in place; remove answered assumptions,
   stale priorities, and inactive "not now" entries.

7. **Update roadmap tails in `./workflow/PLAN.md`** per Updating PLAN.md.

8. **Report.** State the paths written, the main direction decisions, labeled
   assumptions, and any roadmap tails added. End with the next-step
   recommendation: the next missing canon artifact — `architecture` when
   `ARCHITECTURE.md` is absent, `design-guideline` when `DESIGN.md` is absent
   — or `feature` when the project canon is complete.

## Artifact Requirements

Both documents are self-contained, need no chat transcript, and do not
duplicate each other. Size is proportional to the project: a small project
gets a short vision and a short roadmap.

`./workflow/VISION.md` — the stable "why". Core sections: purpose and users;
problem and value; principles that guide product choices; success signals;
non-goals and constraints. Add a section (for example positioning) only when
it carries a decision the project actually needs.

`./workflow/ROADMAP.md` — the current "what next". Core sections: direction
(the current strategic focus); horizons Now / Next / Later, where each active
item names its outcome and rationale; Not Now — active boundaries that prevent
wrong near-term choices, not a rejected-idea archive. Add risks, dependencies,
or confidence notes only where they affect a real prioritization decision.
Label assumptions explicitly.

## Updating PLAN.md

Touch `./workflow/PLAN.md` only for roadmap service tails: unresolved
project-level questions about vision, goals, priority, or success signals,
marked `[ ]`. Place them in the most relevant existing service section, or add
a `## Roadmap Tails` section if none fits. Remove or do not refresh tails the
new canon answers. Do not add feature entries, change feature statuses, or
modify non-roadmap tails. Create `PLAN.md` only when roadmap tails exist and
the file is missing.

## Notes

- If `initialize` has not run, proceed only when direction can still be
  established from the user and local context; otherwise recommend running
  `initialize` first.
- The roadmap is a strategic narrative with a feasibility check, not a
  delivery-date promise.
