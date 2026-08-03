---
name: design-guideline
description: >
  Extracts and records the project's visual canon — brand, color, typography,
  layout, components, states, accessibility, and UI conventions — into
  ./workflow/DESIGN.md. Use when the user says "record the design guideline",
  "describe the project design system", "extract the visual language", or asks
  where the project's design rules live.
---

# Design Guideline — Record the Visual Canon

## Purpose

This skill establishes the project's visual canon in `./workflow/DESIGN.md`.
It extracts the visual language from the existing UI when one exists, and asks
the user only when the UI is absent or too thin to answer. It is the sole
owner of `./workflow/DESIGN.md`.

It is a project-level stage after `initialize`, usually after `roadmap`. The
canon feeds `design`, `feature`, `planning`, `implement`, and `docs` for any
UI work. The skill does not write feature-level `design.md`, implement UI,
plan features, write tests, or generate token files or component libraries —
components are described as project conventions, not implementation.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty tree is expected.
- Create or rewrite only `./workflow/DESIGN.md`; in `./workflow/PLAN.md`
  append design tails only.
- `DESIGN.md` is the current canon only: present tense, no history, deltas,
  temporary notes, or old-version comparisons.
- Record observable, enforceable rules. Separate confirmed canon from open
  decisions; one screen is a sample, not whole-system proof — a project-wide
  rule needs repeated evidence.
- Reuse the existing design system, tokens, and components. Do not invent a
  parallel canon, and do not generate UI code or token files.

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

1. **Read the project context.** Read `./workflow/PROJECT.md`, `VISION.md`,
   and `ROADMAP.md` if they exist, for audience, purpose, and platform
   constraints. Missing files do not stop the skill.

2. **Discover visual sources.** Before asking the user, search the frontend
   code and components, CSS and Tailwind config, theme and design tokens, UI
   libraries, Storybook, and any UI kit or references.

3. **Extract observable signals.** Color, typography, spacing, radius,
   shadow; layout, grid, breakpoints, density; components with their
   variants and empty/loading/error states; interaction states and motion;
   icons, imagery, forms, navigation.

4. **Synthesize the canon.** Separate confirmed canon from assumptions and
   open decisions. Describe color and typography by role and usage limits,
   not raw values alone; use token layers (primitive, semantic, component)
   where they clarify. Consider accessibility: contrast, focus visibility,
   reduced motion, dark mode where relevant. Guard against generic output:
   no thoughtless SaaS palettes, default hero layouts, or decorative effects
   without function. Note the gaps existing material cannot answer.

5. **Ask the user only if needed.** When the UI is absent or thin, ask short
   focused questions about purpose, audience, tone, platform, and brand
   direction — grouped in one batch before writing, with the recommended
   option first marked `(Recommended)` and a brief reason. When the existing
   UI answers the canon, skip this step.

6. **Write `./workflow/DESIGN.md`** per Artifact Requirements.

7. **Append design tails.** Add to `./workflow/PLAN.md` only design tails for
   unresolved visual decisions that block a unified canon. No status changes
   or other writes. If `PLAN.md` is missing, keep unresolved decisions in the
   open-decisions part of `DESIGN.md`.

8. **Report.** State the path written, the source of the canon (existing UI
   versus user answers), the main visual decisions, and any design tails. End
   with the next-step recommendation: the next missing canon artifact —
   `roadmap` when `VISION.md`/`ROADMAP.md` is absent, `architecture` when
   `ARCHITECTURE.md` is absent — or `feature` when the project canon is
   complete.

## Artifact Requirements

`./workflow/DESIGN.md` is a self-contained document that reads without
creation context. Core sections:

- **Visual direction** — the core aesthetic, what the interface must convey,
  what to avoid, grounded in audience and platform.
- **Colors and typography** — values with their semantic roles, usage limits,
  and contrast notes; type scale and readability rules with fallbacks.
- **Layout** — grid or container, spacing scale, breakpoints, density.
- **Components and interactions** — core project components as conventions,
  their variants and states (including empty/loading/error), interaction
  states and motion.
- **Accessibility** — contrast, focus visibility, touch targets; reduced
  motion, high contrast, and dark mode where applicable.

Add a further section (brand, icons, data visualization, platform-specific
conventions) only when it carries a rule the project actually needs. For
Tailwind projects the canon may rely on CSS variables, `@theme`, and
responsive utilities — never assume a Tailwind version or require migration.
For mobile projects record safe areas, touch targets, and platform navigation
conventions.

## Updating PLAN.md

Append only design service tails: unresolved visual decisions blocking a
unified canon, marked `[ ]`. Do not add feature entries, change feature
statuses, or modify non-design tails.

## Notes

- With no `./workflow/` and no frontend code, the canon rests on user
  answers; say so in the report.
