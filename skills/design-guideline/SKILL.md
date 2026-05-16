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

`design-guideline` establishes the project's visual canon. It extracts the visual language from the existing UI when one exists, asks the user about visual direction when the UI is absent or thin, and records everything as `./workflow/DESIGN.md`. This skill is the sole owner of `./workflow/DESIGN.md`.

It operates at the project level, after `initialize` and usually after `roadmap`. Its output is read by the feature-level `design` step and by `feature`, `planning`, `implement`, and `docs` whenever a task touches UI.

It does not write a feature-level `design.md`, does not implement UI, does not create a feature plan, does not write tests, does not generate token files or a component library, and performs no git operations.

## Strict Rules

- No git operations: do not check status, create branches, commit, or push. The working tree is expected to be dirty.
- Call `mcp__sequential-thinking__sequentialthinking` during the analysis step (step 4): synthesizing extracted UI signals into a canon is analytical work and must not be skipped.
- `./workflow/DESIGN.md` is the only file this skill creates or rewrites. The only allowed write to `./workflow/PLAN.md` is appending design tails.
- Write `./workflow/DESIGN.md` as the current canon: no creation history, no "previously X, now Y" deltas, no temporary notes, no comparisons to earlier versions.
- Record observable, enforceable rules — not taste slogans. Separate what is confirmed from what still needs a decision.
- Do not generate UI code, `tokens.json`/`tokens.css` or other token files, or a component library. Describe components at the level of project conventions, not implementation.

## Steps

This is a multi-step procedure with user questions and a nested analytical call. At the start, open a task planning mode list (todo list / task plan, whichever is available) with the steps below and close them one by one.

### 1. Read project context

Read these files if they exist; skip silently if missing:

- `./workflow/PROJECT.md` — tech stack, frameworks, run/deploy.
- `./workflow/VISION.md` — ideological direction.
- `./workflow/ROADMAP.md` — development goals.

These set the audience, purpose, and platform constraints the visual canon must serve.

### 2. Discover visual sources

Search the codebase for existing visual material before asking the user anything:

- frontend code and components;
- CSS, Tailwind config, `@theme` blocks, shadcn/theme config, design tokens;
- a design system or UI library used by the project;
- Storybook, UI kit, screenshots, or other visual references.

A project may already have a design system, component set, or tokens — find and reuse them rather than inventing a parallel canon.

### 3. Extract observable signals

From the discovered sources, extract only what is actually observable:

- colors, typography, spacing, radius, shadows;
- layout, grid/container, breakpoints, density;
- components, variants, states (including empty/loading/error);
- interaction states (hover/focus/active/disabled), motion;
- iconography, imagery, data visualization, forms, navigation.

Treat one page or one screen as a sample, not proof of the whole design system. Do not promote a single observed value into a project-wide rule without evidence across the UI.

### 4. Analyze and synthesize (sequential-thinking)

Call `mcp__sequential-thinking__sequentialthinking` to:

- separate confirmed canon from assumptions and from items that still need a decision;
- group colors, typography, components, and visual assets into distinct concerns rather than one undifferentiated list;
- define the product context — purpose, audience, tone/character, platform constraints, differentiation — that the visual direction must express;
- frame color and typography by role and usage limits, not by raw values alone;
- apply token layering where it clarifies the canon: primitive (raw values like `#111827`, `16px`, `Inter`), semantic (roles like `text-primary`, `surface-muted`, `accent`), component (use in components like `button-bg`, `card-border`);
- decide whether reduced motion, high contrast, and dark mode are relevant project decisions;
- list anti-generic criteria: avoid thoughtless SaaS palettes, default hero compositions, random gradients, and decorative effects with no function;
- collect every gap that the existing material cannot answer.

### 5. Ask the user (only if needed)

If the UI is absent or the data is too thin to fix a canon, ask the user short, focused questions about purpose, audience, tone and character, platform constraints, brand direction, and differentiation. Group independent questions into one block; for each, offer a recommended option first with a brief rationale. If the existing UI fully answers the canon, skip this step.

### 6. Write `./workflow/DESIGN.md`

Create or update `./workflow/DESIGN.md` following the structure in **Artifact Requirements**. Record the current canon only — no history, no deltas.

### 7. Run the quality checklist

Before finishing, verify the document against this checklist:

- hierarchy, consistency, accessibility, and responsive behavior are all addressed;
- critical requirements are distinguishable from taste-level recommendations;
- colors and typography are described by role and usage limits, not hex values alone;
- components are described as project conventions, without implementation detail;
- accessibility and responsive behavior are concrete, not hidden in general phrasing;
- the document reads on its own, without project history or external explanation.

### 8. Append design tails to `./workflow/PLAN.md`

Add to `./workflow/PLAN.md` only the design tails that block a unified project canon — unresolved visual decisions that need follow-up. Do not record feature statuses or anything else. If `./workflow/PLAN.md` is missing, note the unresolved decisions in the "Открытые решения" section of `DESIGN.md` instead.

### 9. Report

Give a short report: the path to `./workflow/DESIGN.md`, whether the canon was extracted from existing UI or based on user answers, the main visual decisions fixed, and any design tails added to `./workflow/PLAN.md`.

## Artifact Requirements

`./workflow/DESIGN.md` uses this structure. Omit a section only when it genuinely does not apply to the project.

```md
# DESIGN.md

## Назначение
Единый визуальный канон проекта.

## Контекст продукта
- Аудитория
- Задачи интерфейса
- Тон и характер
- Ограничения платформы

## Визуальное направление
- Ключевая эстетика
- Что интерфейс должен транслировать
- Чего избегать

## Бренд
- Название/логотип, если есть
- Голос интерфейса
- Допустимые визуальные ассоциации

## Цвета
- Primitive values
- Semantic roles
- Status colors
- Contrast/accessibility notes

## Типографика
- Headings
- Body/UI text
- Mono/code, если нужно
- Fallbacks
- Scale and weight rules

## Layout
- Grid/container
- Spacing scale
- Breakpoints
- Density rules

## Компоненты
- Основные компоненты проекта
- Варианты
- Состояния
- Empty/loading/error states

## Интеракции
- Hover/focus/active/disabled
- Motion
- Feedback
- Keyboard behavior, если применимо

## Accessibility
- Contrast
- Focus visibility
- Touch targets
- Reduced motion/high contrast/dark mode, если применимо

## UI-конвенции
- Icons
- Imagery
- Data visualization
- Forms
- Navigation

## Открытые решения
- Только вопросы, которые мешают единому канону
```

Format rules:

- Describe colors both by primitive value and by role: primary text, muted text, surface, border, accent, status.
- Describe typography with fallbacks and readability rules, not font names alone.
- If the project uses Tailwind, the canon may be expressed in terms of CSS variables, `@theme`, dark mode, focus states, and responsive utilities — but never assume a specific Tailwind version and never require migration.
- For mobile projects, record safe areas, touch targets, navigation patterns, and platform conventions.
- The document contains no change history, no temporary notes, and no comparisons to previous versions.

## Updating PLAN.md

At the end, append to `./workflow/PLAN.md` only design tails — unresolved project-level visual decisions that block a single canon. Do not add feature entries or change feature statuses; that is outside this skill's scope.

## Notes

- If no `./workflow/` files exist and no frontend code is found, the canon rests entirely on user answers from step 5; make that explicit in the report.
- Existing design systems, component sets, and tokens take priority — reuse them instead of building a parallel canon.
- The skill follows these steps literally and does not shorten them. The `DESIGN.md` it produces must be readable without knowing how or when it was created.
