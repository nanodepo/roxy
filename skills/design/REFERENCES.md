# REFERENCES

## Назначение

Этот файл фиксирует идеи из близких design/UX/UI-скилов, которые могут усилить `design` без превращения его в генератор кода, дизайн-системный аудит или полноценный product-discovery процесс.

`design` остаётся фичевым владельцем `./workflow/features/{slug}/design.md`: применяет проектный `./workflow/DESIGN.md` к конкретному пользовательскому сценарию, описывает UI-решения и готовит понятную передачу для `implement`.

## Исследованные источники

| Источник | Близость к `design` | Что взять | Что не брать |
| --- | --- | --- | --- |
| `davila7/claude-code-templates@feature-design-assistant` | Фичевое проектирование и handoff | Поэтапное уточнение контекста, фиксацию развилок, 2-3 варианта решения для спорных мест, проверку зависимостей и ограничений | Универсальный technical design, task checklist, git/commit-шаги, интерактивную анкету как обязательный ритуал |
| `refoundai/lenny-skills@writing-specs-designs` | Спеки и дизайн-документы | Выбор fidelity перед записью, фокус на ключевых движущихся частях, предупреждение против лишней детализации, эффективность каждого действия пользователя | Цитатный материал, static mock/prototype push как обязательную часть скила |
| `borghei/claude-skills@product-designer` | Широкий product design | Happy path + error state как минимум, user journey, IA, accessibility foundations, компонентные состояния, dev handoff | Discovery-исследования, дизайн-спринты, usability tests, SUS-метрики, генераторы токенов |
| `owl-listener/designer-skills@wireframe-spec` | Wireframe и layout-спека | Иерархию контента, размещение компонентов, responsive notes, dynamic content, состояния empty/loading/populated/error | Отдельные wireframe-артефакты, графические конвенции, цветовой запрет как жёсткое правило |
| `owl-listener/designer-skills@component-spec` | Компонентная спецификация | Anatomy, variants, states, behavior, accessibility, usage rules; явные required/optional элементы | Полную API-спеку компонентов, exhaustive variant matrix для простых фич |
| `owl-listener/designer-skills@micro-interaction-spec` | Поведение и микровзаимодействия | Trigger, response, feedback, duration/easing, reduced-motion, interruptibility | Анимационные пресеты, audio/haptic детали, если проект их не использует |
| `owl-listener/designer-skills@handoff-spec` | Передача дизайна разработке | Edge cases, responsive behavior, content rules, localization, technical notes, component reuse | Pixel-perfect значения, если проектный дизайн-канон задаёт более высокий уровень |
| `wshobson/agents@interaction-design` | Motion и feedback | Motion communicates, not decorates; loading/skeleton/progress; hover/focus/toggle/page transition states; `prefers-reduced-motion` | Framer/CSS-код, декоративные эффекты без пользовательского смысла |
| `anthropics/skills@frontend-design` | Качество frontend-интерфейса | Намеренный aesthetic direction, соответствие цели и аудитории, избегание generic AI aesthetics | Реализацию кода, наборы ярких стилей, превращение feature design в визуальный эксперимент |
| `vercel-labs/agent-skills@web-design-guidelines` | Design/accessibility review | Проверку spacing, typography, interaction, accessibility и UX как ограничений для design.md | Fetch remote rules, terse `file:line` audit format, code-review режим |
| `figma/mcp-server-guide@implement-design` | Design-to-code handoff | Reuse existing components, map design tokens to project tokens, inspect large designs incrementally | Figma MCP workflow, pixel-perfect implementation, asset download, кодовую реализацию |

## Полезные усиления для `design`

### 1. Fidelity Before Detail

Перед записью `design.md` определи нужную точность:

- conceptual — нужна развилка, поток и пользовательский смысл;
- low-fi — нужна структура экрана, приоритет контента и состояния;
- implementation-ready — нужно зафиксировать компоненты, layout, responsive, состояния и ограничения для `implement`.

Не описывай пиксельные детали, если достаточно структуры и поведения. Не оставляй только общие намерения, если `implement` должен строить UI без повторного дизайна.

### 2. Feature Context First

Собери короткую карту фичи:

- пользователь и его цель;
- основной сценарий;
- альтернативные сценарии;
- зависимости из `feature.md` и `plan.md`;
- применимые правила из `./workflow/DESIGN.md`;
- существующие frontend-паттерны, которые надо переиспользовать.

Если проектный дизайн-канон отсутствует или недостаточен для решения, не заменяй его локальной фантазией. Запиши в `./workflow/PLAN.md` служебный хвост о недостающем каноне.

### 3. Scenario And State Matrix

Минимальный фичевый дизайн покрывает не только happy path:

- default;
- loading;
- empty;
- error;
- disabled или unavailable;
- success/confirmation;
- permission/role differences, если применимо;
- mobile/desktop differences, если применимо.

Для каждого состояния укажи, что видит пользователь, что может сделать и какая обратная связь нужна.

### 4. Layout As Content Priority

Описывай layout через приоритеты, а не через декоративную композицию:

- что пользователь должен увидеть первым;
- какие действия первичные и вторичные;
- где живут фильтры, формы, результаты, ошибки и подсказки;
- какие элементы обязательны, а какие появляются условно;
- как меняется структура на узком экране.

Если интерфейс плотный и рабочий, предпочитай сканируемость, предсказуемую навигацию и экономию действий.

### 5. Component Contract

Для новых или изменяемых UI-частей фиксируй:

- название и назначение компонента;
- anatomy: обязательные и опциональные элементы;
- variants, только если они реально нужны фиче;
- states: default, hover, focus, active, disabled, loading, error;
- behavior: что происходит при действиях пользователя;
- accessibility: роль, имя, keyboard/focus, screen-reader поведение;
- reuse: существующий компонент или паттерн, который надо использовать.

Не описывай полный API компонента, если фича требует только одно конкретное применение.

### 6. Interaction And Feedback

Каждое действие пользователя должно иметь понятный ответ системы:

- immediate feedback для кликов, отправки форм и переключателей;
- progress или skeleton для ожидания;
- inline error для исправимых ошибок;
- toast/modal/confirmation только когда это помогает задаче;
- motion только для ориентации, фокуса, continuity или подтверждения.

Укажи, какие переходы должны поддерживать reduced motion. Не добавляй анимацию как украшение.

### 7. Content Rules

Фичевый дизайн должен описывать контентные ограничения, если они влияют на UI:

- заголовки и hierarchy;
- примерная длина текста;
- пустые тексты и error copy;
- правила обрезки, переноса и overflow;
- локализация или длинные строки;
- источник данных: static, CMS, API, user-generated.

Это помогает `implement` не угадывать поведение интерфейса при реальных данных.

### 8. Accessibility And Input Methods

Минимальная проверка design.md:

- всё интерактивное доступно с клавиатуры;
- focus state описан для нестандартных элементов;
- цвет не является единственным носителем смысла;
- формы имеют labels, errors и helper text;
- motion не блокирует действие и уважает reduced motion;
- touch targets достаточны для мобильного сценария;
- loading/error/empty states доступны screen reader, если они динамические.

### 9. Handoff Readiness

`design.md` готов, когда следующий агент может реализовать UI без нового дизайн-раунда:

- цель и пользовательский сценарий понятны;
- layout и responsive behavior зафиксированы;
- состояния перечислены;
- компоненты и reuse-паттерны указаны;
- контентные ограничения есть;
- accessibility constraints есть;
- открытые вопросы вынесены явно;
- граница с проектным `DESIGN.md` не нарушена.

## Минимальная форма `design.md`

```md
# Design: <feature name>

## Контекст

- Пользователь:
- Цель:
- Входные документы:
- Применимые правила из `./workflow/DESIGN.md`:
- Переиспользуемые frontend-паттерны:

## Fidelity

- Уровень:
- Почему достаточно именно этого уровня:

## Сценарии

### Основной сценарий

1. Шаг пользователя.
2. Ответ интерфейса.
3. Следующее состояние.

### Альтернативные сценарии

- Сценарий:
- Поведение:

## Layout

- Приоритет контента:
- Основные зоны:
- Primary action:
- Secondary actions:
- Mobile behavior:
- Desktop behavior:

## Компоненты

### <Component>

- Назначение:
- Anatomy:
- Variants:
- States:
- Behavior:
- Reuse:
- Accessibility:

## Состояния

| Состояние | Что видно | Действия пользователя | Feedback |
| --- | --- | --- | --- |
| Default |  |  |  |
| Loading |  |  |  |
| Empty |  |  |  |
| Error |  |  |  |
| Success |  |  |  |

## Контент

- Заголовки:
- Empty copy:
- Error copy:
- Длина и overflow:
- Источник данных:

## Ограничения для реализации

- Accessibility:
- Motion:
- Responsive:
- Performance:
- Не делать:

## Открытые вопросы

- Вопрос:
- Почему блокирует или не блокирует реализацию:
```

## Правила отсечения

- Не описывай всю дизайн-систему: используй `./workflow/DESIGN.md` как канон.
- Не создавай новые visual principles для одной фичи.
- Не пиши код и не выбирай библиотеку без необходимости.
- Не добавляй research/testing этапы, если пользователь просит фичевый дизайн для реализации.
- Не расширяй `plan.md`: дизайн уточняет UI-решение, а не переписывает план.
- Не перегружай простую фичу exhaustive state/API matrix.
- Не оставляй маркетинговый текст вместо конкретных интерфейсных решений.

## Источники

- https://skills.sh/davila7/claude-code-templates/feature-design-assistant
- https://skills.sh/refoundai/lenny-skills/writing-specs-designs
- https://skills.sh/borghei/claude-skills/product-designer
- https://skills.sh/owl-listener/designer-skills/wireframe-spec
- https://skills.sh/owl-listener/designer-skills/component-spec
- https://skills.sh/owl-listener/designer-skills/micro-interaction-spec
- https://skills.sh/owl-listener/designer-skills/handoff-spec
- https://skills.sh/wshobson/agents/interaction-design
- https://skills.sh/anthropics/skills/frontend-design
- https://skills.sh/vercel-labs/agent-skills/web-design-guidelines
- https://skills.sh/figma/mcp-server-guide/implement-design
