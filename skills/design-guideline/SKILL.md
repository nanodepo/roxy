---
name: design-guideline
description: >
  Extracts and records the project's visual canon — brand, color, typography,
  layout, components, states, accessibility, and UI conventions — into
  ./workflow/DESIGN.md. Use when the user says "зафиксируй дизайн",
  "опиши дизайн проекта", "извлеки визуальный язык", "создай design guideline",
  or asks where the project's design rules live.
---

# Design Guideline

## Purpose

Establish project visual canon in `./workflow/DESIGN.md`. Extract visual lang
from existing UI when present; ask user only when UI absent/thin. Sole owner of
`./workflow/DESIGN.md`.

Project-level stage after `initialize`, usually after `roadmap`. Output feeds
`design`, `feature`, `planning`, `implement`, `docs` for UI work.

Does not write feature `design.md`, implement UI, plan features, write tests,
generate token files/component lib, or run git ops.

## Strict Rules

- No git ops: no status, branch, commit, push. Worktree expected dirty.
- Step 4 must call `mcp__sequential-thinking__sequentialthinking`.
- Create/rewrite only `./workflow/DESIGN.md`. `./workflow/PLAN.md` write only
  appends design tails.
- `./workflow/DESIGN.md` = current canon only: no history, deltas, temp notes,
  old-version comparisons.
- Record observable, enforceable rules. Separate confirmed canon from open
  decisions.
- Do not generate UI code, `tokens.json`, `tokens.css`, token files, component
  lib. Describe comps as project conventions, not impl.

## Language Notice

Write this `SKILL.md` in English.

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

### 1. Read Project Context

Read if exists; skip missing:

- `./workflow/PROJECT.md` — stack, frameworks, run/deploy.
- `./workflow/VISION.md` — direction.
- `./workflow/ROADMAP.md` — goals.

Use for audience, purpose, platform constraints.

### 2. Discover Visual Sources

Search before asking user:

- frontend code/comps
- CSS, Tailwind config, `@theme`, shadcn/theme config, design tokens
- design system / UI lib
- Storybook, UI kit, screenshots, refs

Reuse existing system/tokens/comps. Do not invent parallel canon.

### 3. Extract Signals

Extract only observable:

- color, type, spacing, radius, shadow
- layout, grid/container, breakpoints, density
- comps, variants, empty/loading/error states
- hover/focus/active/disabled, motion
- icons, imagery, charts, forms, nav

One screen = sample, not whole-system proof. Need repeated evidence before
project-wide rule.

### 4. Analyze

Call `mcp__sequential-thinking__sequentialthinking`. Synthesize:

- confirmed canon vs assumptions vs open decisions
- separate concerns: color, type, comps, visual assets
- product ctx: purpose, audience, tone, platform, differentiation
- color/type by role + usage limits, not raw values only
- token layers where useful:
  - primitive: `#111827`, `16px`, `Inter`
  - semantic: `text-primary`, `surface-muted`, `accent`
  - component: `button-bg`, `card-border`
- reduced motion, high contrast, dark mode relevance
- anti-generic criteria: no thoughtless SaaS palettes, default hero layouts,
  random gradients, decorative effects without function
- gaps existing material cannot answer

### 5. Ask User Only If Needed

If UI absent/thin, ask short focused questions: purpose, audience, tone,
platform, brand direction, differentiation. Group questions. Each option:
recommended first with `(Recommended)` + brief reason.

If UI answers canon, skip.

### 6. Write `./workflow/DESIGN.md`

Create/update via Artifact Req. Current canon only; no history/deltas.

### 7. Quality Check

Before finish verify:

- hierarchy, consistency, accessibility, responsive addressed
- critical reqs distinct from taste recs
- color/type described by role + usage limits
- comps described as conventions, no impl detail
- accessibility/responsive concrete
- doc reads standalone, no project history needed

### 8. Append Design Tails

Add to `./workflow/PLAN.md` only design tails blocking unified canon:
unresolved visual decisions needing follow-up. No statuses/other writes.

If `./workflow/PLAN.md` missing, put unresolved decisions in `Open decisions`
of `DESIGN.md`.

### 9. Report

Short report: `./workflow/DESIGN.md` path, source of canon (existing UI vs user
answers), main visual decisions fixed, design tails added.

## Artifact Req

`./workflow/DESIGN.md` semantic shape. Translate all visible headings, labels,
table headers, placeholders, examples. Omit section only when not applicable.

```md
# DESIGN.md

## Purpose
The project's unified visual canon.

## Product context
- Audience
- Interface goals
- Tone and character
- Platform constraints

## Visual direction
- Core aesthetic
- What the interface must convey
- What to avoid

## Brand
- Name/logo, if any
- Interface voice
- Acceptable visual associations

## Colors
- Primitive values
- Semantic roles
- Status colors
- Contrast/accessibility notes

## Typography
- Headings
- Body/UI text
- Mono/code, if needed
- Fallbacks
- Scale and weight rules

## Layout
- Grid/container
- Spacing scale
- Breakpoints
- Density rules

## Components
- Core project components
- Variants
- States
- Empty/loading/error states

## Interactions
- Hover/focus/active/disabled
- Motion
- Feedback
- Keyboard behavior, if applicable

## Accessibility
- Contrast
- Focus visibility
- Touch targets
- Reduced motion/high contrast/dark mode, if applicable

## UI conventions
- Icons
- Imagery
- Data visualization
- Forms
- Navigation

## Open decisions
- Only questions that block a unified canon
```

Format rules:

- Colors: primitive value + role: primary text, muted text, surface, border,
  accent, status.
- Type: fallbacks + readability rules, not font names only.
- Tailwind projects: canon may use CSS vars, `@theme`, dark mode, focus states,
  responsive utilities; never assume Tailwind version or require migration.
- Mobile projects: record safe areas, touch targets, nav patterns, platform
  conventions.

## Notes

- No `./workflow/` + no frontend code -> canon rests on user answers. Say so in
  report.
- Follow steps literally. Produced `DESIGN.md` must read without creation
  context.
