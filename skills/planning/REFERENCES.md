# REFERENCES.md

## Назначение

Этот материал помогает усилить `planning` без расширения его ответственности. Скил остается этапом планирования реализации: он превращает `./workflow/features/{slug}/feature.md` в самодостаточный `./workflow/features/{slug}/plan.md`, обновляет статус фичи в `./workflow/PLAN.md` на `[-]`, но не пишет `design.md`, `tests.md`, `NN-task.md`, код, документацию или git-операции.

## Близкие скилы и полезные идеи

### `addyosmani/agent-skills@planning-and-task-breakdown`

Источник: https://github.com/addyosmani/agent-skills/blob/main/skills/planning-and-task-breakdown/SKILL.md

Суть: разложение работы на упорядоченные, проверяемые задачи с acceptance criteria.

Что взять:

- Планирование выполняется в read-only режиме по отношению к продуктовому коду.
- Перед записью плана нужно прочитать feature, релевантные документы и участки кодовой базы.
- Полезно явно построить dependency graph: данные, API, UI, интеграции, миграции, shared-модули.
- Вертикальные срезы лучше горизонтальных блоков: план должен вести к работающему поведению, а не к набору изолированных слоев.
- В `plan.md` стоит фиксировать фазы, зависимости, likely touched files, риски, checkpoints и open questions.
- Большие пункты нужно дробить: если пункт требует много файлов, много подсистем или содержит "и" в названии, он слишком широкий.

Что не брать:

- Создание отдельных `NN-task.md`. Это ответственность `task`.
- Требование human approval как отдельного gate после каждого раздела.
- Полный task-breakdown формат с отдельными task-файлами.
- Обязательное включение тестовых шагов как тестового плана.

### `addyosmani/agent-skills@spec-driven-development`

Источник: https://github.com/addyosmani/agent-skills/blob/main/skills/spec-driven-development/SKILL.md

Суть: требования, success criteria и boundaries должны быть ясны до реализации.

Что взять:

- Не планировать молча поверх неясной фичи; сначала выявить assumptions и open questions.
- Переформулировать расплывчатые требования в проверяемые success criteria.
- В плане должны быть major components, implementation order, risks, mitigation, parallel/sequential work и verification checkpoints.
- План должен быть reviewable: другой агент может понять подход без пересказа чата.
- Если во время планирования появляются решения, которые меняют смысл фичи, нужно зафиксировать их в `plan.md` или вынести вопрос.

Что не брать:

- Полный spec-first workflow. `feature.md` уже является входным артефактом.
- Создание отдельного `SPEC.md` или PRD.
- Коммиты, PR-ссылки и external review rituals.

### `softaworks/agent-toolkit@requirements-clarity`

Источник: https://skills.sh/softaworks/agent-toolkit/requirements-clarity

Суть: системное уточнение размытых требований через gap analysis.

Что взять:

- Проверяй `feature.md` на функциональную ясность, технический контекст, границы, edge cases и error handling.
- Задавай короткие вопросы только по блокирующим пробелам.
- Группируй вопросы по категориям: scope, user interaction, data, integrations, constraints.
- Не перегружай пользователя: 2-3 точных вопроса лучше, чем длинная анкета.

Что не брать:

- 100-point clarity score.
- Генерацию PRD в `./docs/prds/`.
- Обязательное достижение формального порога качества перед записью плана.
- Сохранение clarification rounds как биографии.

### `openai/skills@create-plan`

Источник: https://skills.sh/openai/skills/create-plan

Суть: компактный план с scope, action items и open questions.

Что взять:

- Быстрый context scan перед планом: проектные документы, очевидные docs, вероятные файлы.
- Явное разделение `In` и `Out`, чтобы план не расползался.
- Чекбоксы должны быть concrete, ordered и verb-first.
- Open questions короткие и только по настоящим неизвестным.
- Избегать vague steps вроде "handle backend" или "do auth".

Что не брать:

- Read-only final message вместо записи файла. `planning` обязан создать или обновить `plan.md`.
- Ограничение на 6-10 пунктов как жесткое правило.
- Универсальный порядок discovery -> changes -> tests -> rollout, если он конфликтует с локальным workflow.

### `jezweb/claude-skills@project-planning`

Источник: https://skills.sh/jezweb/claude-skills/project-planning

Суть: структурированные implementation phases с sizing, files, verification criteria и exit criteria.

Что взять:

- Фазы должны быть context-safe: ограниченный scope, понятные зависимости, конкретные файлы, clear exit criteria.
- Для разных типов работы полезны разные phase labels: infrastructure, database, API, UI, integration.
- Если фаза слишком большая, план должен предложить разрезание.
- Для UI-фич полезно указать, какие компоненты или routes затрагиваются, но не проектировать `design.md`.
- Для сложных setup/integration шагов полезно выделить critical workflow notes.

Что не брать:

- Создание набора отдельных документов (`IMPLEMENTATION_PHASES.md`, `DATABASE_SCHEMA.md`, `API_ENDPOINTS.md`, `SESSION.md`, `TESTING.md`).
- Cloudflare/Vite/React/D1 как default stack.
- Git commit, session management и автоматизированные slash commands.

### `qodex-ai/ai-agent-skills@plan-implementation`

Источник: https://skills.sh/qodex-ai/ai-agent-skills

Суть: системное исполнение detailed implementation plans с progress tracking и milestones.

Что взять:

- План должен быть пригоден для пошагового исполнения: milestones, order, blockers, progress markers.
- Структура плана должна позволять `implement` отмечать фактический прогресс без догадок.
- В плане стоит отделять последовательные зависимости от независимых веток работы.

Что не брать:

- Execution engine и управление выполнением плана. Это зона `implement`.
- Milestone management во внешних трекерах.

### `jwynia/agent-skills@task-breakdown`

Источник: https://playbooks.com/skills/jwynia/agent-skills/task-breakdown

Суть: разбиение перегружающих задач на управляемые фрагменты с clear completion criteria.

Что взять:

- Начинай с actual requirement: минимум результата, non-negotiable, out of scope.
- Chunk by natural breakpoints: outcome, dependency, context, decision boundary.
- У каждого chunk должен быть видимый completion marker.
- Не создавай разбиение, которое само становится тяжелее задачи.

Что не брать:

- ADHD/autism-specific coaching как часть технического planning-скила.
- Energy mapping, transition rituals, support structures.
- Отдельные task-breakdown output files.

### `github/awesome-copilot@breakdown-feature-implementation`

Источник: https://claudemarketplaces.com/skills/github/awesome-copilot/breakdown-feature-implementation

Суть: превращение требований в implementation blueprint.

Что взять:

- Для сложной фичи полезно покрыть frontend, backend, data model, API contracts, infrastructure constraints и risks.
- План должен назвать ключевые interfaces/contracts, если они нужны нескольким частям системы.
- Если фича затрагивает deployment/runtime, это нужно отразить как constraint или risk.

Что не брать:

- Полный blueprint с Mermaid-диаграммами, deployment strategy и issue creation.
- Привязку к monorepo-шаблону или конкретной UI-библиотеке.

## Фильтр применимости

Добавляй в `planning` только то, что помогает получить реалистичный и самодостаточный `plan.md`.

Подходит:

- анализ `feature.md` и проектного контекста;
- codebase reconnaissance до уровня, достаточного для плана;
- уточнение блокирующих пробелов;
- dependency graph;
- фазы реализации;
- плановые чекбоксы;
- likely touched files;
- dependencies and sequencing;
- risks and mitigations;
- verification/checkpoints на уровне плана;
- open questions;
- обновление статуса фичи на `[-]`.

Не подходит:

- создание `NN-task.md`;
- написание `tests.md`;
- написание тестового кода;
- фичевое UI-проектирование в `design.md`;
- реализация кода;
- документация в `./docs/`;
- создание PRD/SPEC как отдельного артефакта;
- issue tracking, sprint planning, PR, branch или commit workflow.

## Рекомендуемый алгоритм `planning`

1. Определи slug фичи и прочитай `./workflow/features/{slug}/feature.md`.
2. Прочитай `./workflow/PROJECT.md`, `./workflow/ARCHITECTURE.md`, `./workflow/DESIGN.md`, если они есть и релевантны.
3. Найди в кодовой базе похожие модули, routes, components, services, schemas, migrations, configs и conventions.
4. Определи, что уже существует, что нужно изменить, что нужно создать и какие границы нельзя нарушать.
5. Проверь feature на блокирующие пробелы: scope, user flow, data, integrations, error cases, permissions, UI constraints.
6. Если пробел блокирует план, задай 1-3 точных вопроса или запиши open question, если можно безопасно продолжить.
7. Построй dependency graph: foundation, data, API/contracts, UI, integrations, migrations, rollout constraints.
8. Разложи работу на фазы, которые можно выполнить и проверить без удержания всего проекта в голове.
9. Для каждой фазы укажи цель, плановые чекбоксы, likely touched files, зависимости, риски и критерий выхода.
10. Отдельно зафиксируй sequential work и parallelizable work, если это помогает `task` или `implement`.
11. Запиши `./workflow/features/{slug}/plan.md` как текущее состояние плана без истории создания.
12. Обнови статус фичи в `./workflow/PLAN.md` на `[-]`.

## Рекомендуемая структура `plan.md`

```md
# plan.md

## Цель

Кратко опиши, какое поведение должна дать фича и какой подход выбран.

## Scope

### In

- Что входит в реализацию

### Out

- Что не входит в реализацию

## Контекст

- Входные документы
- Релевантные модули
- Локальные паттерны
- Ограничения архитектуры, дизайна или стека

## Решения

- Технические решения и причины
- Контракты между частями системы
- Assumptions, если без них можно продолжить

## Dependency graph

- Foundation
- Data/model
- API/contracts
- UI/interaction
- Integrations
- Rollout/runtime constraints

## Фазы

### Phase 1: Название

Цель: ...

Пункты:

- [ ] Конкретное действие
- [ ] Конкретное действие

Likely files:

- `path/to/file`

Dependencies:

- Нет / Phase N / внешний ответ

Risks:

- Риск -> mitigation

Exit criteria:

- Что должно быть true после фазы

### Phase 2: Название

...

## Checkpoints

- [ ] После foundation: ...
- [ ] После core flow: ...
- [ ] Перед передачей в `task`: ...

## Open questions

- Только вопросы, которые влияют на реализацию

## Хвосты

- Что нужно передать в `design`, `task`, `test`, `docs` или пользователю
```

## Правила качества плана

- `plan.md` можно передать следующему агенту без пересказа чата.
- План описывает настоящее состояние фичи, без истории изменений и временных пометок.
- В плане есть `In` и `Out`, чтобы scope не расползался.
- Каждая фаза имеет цель, действия, likely files, зависимости и exit criteria.
- Риски описаны через практичную mitigation, а не общим предупреждением.
- Большие фазы разрезаны до управляемого размера.
- Checkpoints описывают проверку реализации, но не заменяют `tests.md`.
- Open questions короткие и действительно влияют на реализацию.
- План не содержит тестовый канон, фичевый дизайн, код или документацию.

## Команды исследования

```bash
npx skills find "implementation planning"
npx skills find "feature planning"
npx skills find "task breakdown"
npx skills find "spec driven development"
npx skills find "project planning"
npx skills find "execution plan"
npx skills find "requirements clarity"
npx skills find "architecture planning"
```
