# REFERENCES.md

## Назначение

Этот материал помогает усилить `design-guideline` без расширения его ответственности. Скил остается проектным этапом: он извлекает и фиксирует визуальный канон проекта в `./workflow/DESIGN.md`, но не реализует UI, не пишет фичевый `design.md` и не создает компонентную библиотеку.

## Близкие скилы и полезные идеи

### `vercel-labs/agent-skills@web-design-guidelines`

Источник: https://skills.sh/vercel-labs/agent-skills/web-design-guidelines

Суть: аудит UI-кода по набору правил дизайна, UX и accessibility.

Что взять:

- Формат проверки через конкретные критерии, а не общую вкусовщину.
- Короткие findings, привязанные к файлам или областям интерфейса, когда скил анализирует существующий UI.
- Отдельный блок accessibility и interaction states как часть визуального канона.

Что не брать:

- Режим terse `file:line` как основной результат. `design-guideline` должен выпускать `./workflow/DESIGN.md`, а не отчет аудита.
- Зависимость от удаленного актуального гайда перед каждым запуском.

### `arvindrk/extract-design-system@extract-design-system`

Источник: https://skills.sh/arvindrk/extract-design-system/extract-design-system

Суть: reverse-engineering дизайн-примитивов из публичного сайта в starter token files.

Что взять:

- Явный discovery-порядок: сначала извлеки наблюдаемые цвета, шрифты, spacing, radius, shadows; затем отдели подтвержденное от предположений.
- Предупреждение: одна страница или один экран не доказывает всю дизайн-систему проекта.
- Не считать извлеченные данные авторитетными без ревью и согласования.

Что не брать:

- Генерацию `tokens.json`, `tokens.css` и других файлов как обязательный результат.
- Требование публичного URL. В целевом проекте главным источником является локальный frontend-код.

### `wshobson/agents@design-system-patterns`

Источник: https://skills.sh/wshobson/agents/design-system-patterns

Суть: архитектура дизайн-систем через token hierarchy, theming и component architecture.

Что взять:

- Легкую трехслойную терминологию токенов:
  - primitive: сырые значения (`#111827`, `16px`, `Inter`);
  - semantic: смысловые роли (`text-primary`, `surface-muted`, `accent`);
  - component: применение в компонентах (`button-bg`, `card-border`).
- Разделение темизации, компонентов и токенов в `DESIGN.md`.
- Учет reduced motion, high contrast и dark mode как проектных решений, если они релевантны.

Что не брать:

- Реализацию theme provider, Style Dictionary, Figma sync и multi-platform pipeline.
- Детальные component API patterns. Это зона implementation или отдельного design-system скила.

### `wshobson/agents@tailwind-design-system`

Источник: https://skills.sh/wshobson/agents/tailwind-design-system

Суть: CSS-first дизайн-система для Tailwind v4.

Что взять:

- Если проект использует Tailwind, фиксировать канон в терминах CSS variables, `@theme`, dark mode, focus states и responsive utilities.
- Для `DESIGN.md` полезна структура `brand -> semantic -> component`, но без обязательной миграции.

Что не брать:

- Tailwind v4 как универсальное предположение.
- Миграционные инструкции и production-ready component snippets.

### `anthropics/skills@frontend-design`

Источник: https://skills.sh/anthropics/skills/frontend-design

Суть: создание выразительных production-grade интерфейсов с намеренным эстетическим направлением.

Что взять:

- Перед фиксацией канона определить: purpose, audience, tone, constraints, differentiation.
- Требовать ясную эстетическую позицию, а не набор случайных визуальных приемов.
- Добавить анти-генерик критерии: избегать бездумных SaaS-палитр, стандартных hero-композиций, случайных градиентов и декоративных эффектов без функции.

Что не брать:

- Императив "build working code". `design-guideline` не реализует UI.
- Перечень экстремальных стилей как меню, если проект уже имеет сложившийся визуальный язык.

### `anthropics/skills@brand-guidelines`

Источник: https://skills.sh/anthropics/skills/brand-guidelines

Суть: компактное применение брендовых цветов и типографики.

Что взять:

- `DESIGN.md` должен фиксировать брендовые цвета, роли цветов, типографику, fallback-шрифты и правила читаемости.
- Цвета нужно описывать не только hex-значениями, но и назначением: primary text, muted text, surface, border, accent, status.

Что не брать:

- Жесткий пример конкретного бренда.
- Автоматическое применение стилей к внешним артефактам.

### `github/awesome-copilot@penpot-uiux-design`

Источник: https://skills.sh/github/awesome-copilot/penpot-uiux-design

Суть: создание UI/UX-дизайнов в Penpot с discovery existing design systems, responsive layouts и accessibility validation.

Что взять:

- Сначала искать существующие design systems, компоненты и tokens.
- В `DESIGN.md` фиксировать responsive breakpoints или целевые viewport-классы, если они видны в проекте.
- Добавить короткий review checklist: hierarchy, consistency, accessibility, responsive behavior.

Что не брать:

- Penpot MCP как обязательный инструмент.
- Создание дизайн-макетов.

### `ehmo/platform-design-skills@ios-design-guidelines`

Источник: https://skills.sh/ehmo/platform-design-skills/ios-design-guidelines

Суть: подробные платформенные правила по Apple HIG.

Что взять:

- Идею "impact-aware" правил: критичные требования отделены от вкусовых рекомендаций.
- Для мобильных проектов фиксировать safe areas, touch targets, navigation patterns и platform conventions.

Что не брать:

- 100+ правил в тело `design-guideline`.
- SwiftUI/UIKit examples, если проект не iOS.

### `nextlevelbuilder/ui-ux-pro-max-skill@ckm:design`

Источник: https://skills.sh/nextlevelbuilder/ui-ux-pro-max-skill/ckm:design

Суть: широкая маршрутизация по brand, design-system, UI styling, logo, slides, banners, icons.

Что взять:

- Идею маршрутизации: бренд, токены, UI conventions и визуальные ассеты должны быть разными разделами.
- Если требуется logo, banners или social assets, это отдельная задача, а не обязанность `design-guideline`.

Что не брать:

- Генераторы логотипов, CIP, презентаций, баннеров и иконок.
- Большую библиотеку палитр, стилей и скриптов.

## Фильтр применимости

Добавляй в `design-guideline` только то, что помогает получить самодостаточный `./workflow/DESIGN.md`.

Подходит:

- discovery существующего UI;
- извлечение наблюдаемых токенов;
- вопросы пользователю при нехватке данных;
- описание визуального направления и проектных UI-конвенций;
- чеклист качества и accessibility;
- служебные дизайн-хвосты в `./workflow/PLAN.md`.

Не подходит:

- генерация UI-кода;
- генерация токен-файлов;
- создание компонентной библиотеки;
- дизайн конкретной фичи;
- платформенные справочники целиком;
- бренд-ассеты, презентации, баннеры, логотипы;
- git-операции.

## Рекомендуемый discovery-порядок

1. Прочитай `./workflow/PROJECT.md`, `./workflow/VISION.md`, `./workflow/ROADMAP.md`, если они есть.
2. Найди frontend-код, CSS, Tailwind/shadcn/theme config, design tokens, Storybook, UI kit, screenshots или другие визуальные источники.
3. Извлеки только наблюдаемые признаки: цвета, типографику, spacing, radius, shadows, layout, breakpoints, компоненты, состояния, iconography, motion.
4. Раздели подтвержденное и требующее решения.
5. Если UI отсутствует или данных мало, задай пользователю короткие вопросы о tone, audience, constraints и brand direction.
6. Запиши `./workflow/DESIGN.md` как текущий канон, без истории создания и без сравнений с прежними решениями.
7. В `./workflow/PLAN.md` добавь только те дизайн-хвосты, которые блокируют единый проектный канон.

## Рекомендуемая структура `./workflow/DESIGN.md`

```md
# DESIGN.md

## Назначение

Кратко опиши роль документа: единый визуальный канон проекта.

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

## Критерии готовности

- `./workflow/DESIGN.md` можно читать без истории проекта и без внешних пояснений.
- В документе есть наблюдаемые правила, а не вкусовые лозунги.
- Цвета и типографика описаны через роли и ограничения применения.
- Компоненты описаны на уровне проектных конвенций, без реализации.
- Accessibility и responsive behavior не спрятаны в общих словах.
- Неясные решения вынесены в `./workflow/PLAN.md` как служебные дизайн-хвосты.
- Документ не содержит биографию изменений, временные пометки и сравнения с прежними версиями.

## Команды исследования

```bash
npx skills find "design guideline"
npx skills find "design system"
npx skills find "ui ux design"
npx skills find "brand guidelines"
npx skills find "design audit"
npx skills find "accessibility design"
npx skills find "frontend design"
```
