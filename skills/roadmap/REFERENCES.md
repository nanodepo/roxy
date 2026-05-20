# REFERENCES.md

## Назначение

Этот материал помогает усилить `roadmap` без расширения его ответственности. Скил остается владельцем проектного смысла и целевого канона: он создает или обновляет `./workflow/VISION.md` и `./workflow/ROADMAP.md`, а в `./workflow/PLAN.md` добавляет только служебные roadmap-хвосты. Он не создает фичи, PRD, sprint backlog, архитектуру, дизайн, план реализации, код, документацию или git-операции.

## Близкие скилы и полезные идеи

### `deanpeters/product-manager-skills@roadmap-planning`

Источник: https://claudemarketplaces.com/skills/deanpeters/product-manager-skills/roadmap-planning

Суть: превращение стратегии и конкурирующих feature requests в outcome-driven roadmap со стратегической связностью.

Что взять:

- Roadmap должен быть стратегическим рассказом, а не списком фич.
- Полезно связывать направления с целями, эпиками, зависимостями и ограничениями.
- Нужен явный механизм sequencing: что делаем сейчас, что позже, что отложено.
- Roadmap должен показывать rationale: почему это важно и почему именно в таком порядке.
- Для сложного проекта полезны разные представления: Now/Next/Later, горизонты, темы.

Что не брать:

- Квартальный или полугодовой corporate PM workflow как обязательный формат.
- Epic definition, release planning и stakeholder deck.
- Создание backlog, user stories или sprint/release plan.

### `phuryn/pm-skills@outcome-roadmap`

Источник: https://skillsauth.com/skills/phuryn-pm-skills-pm-execution-skills-outcome-roadmap

Суть: перевод feature/output roadmap в outcome-focused roadmap.

Что взять:

- Формулируй направления через outcomes: какой пользовательский или проектный результат должен измениться.
- Для каждого направления фиксируй success signals или метрики, если они известны.
- Вопрос "so what?" помогает не оставлять цель на уровне "добавить X".
- Roadmap должен быть гибким: одна outcome-цель может быть достигнута разными фичами.
- Хороший формат outcome statement: `Enable [segment/user] to [desired outcome] so that [project/business impact]`.

Что не брать:

- Переписывание существующего roadmap как отдельный transformation report.
- Обязательную привязку к кварталам и годам.
- Отдельный `Outcome-Roadmap-[year].md`.

### `refoundai/lenny-skills@prioritizing-roadmap`

Источник: https://skills.sh/refoundai/lenny-skills/prioritizing-roadmap

Суть: фреймворк приоритизации roadmap с учетом убежденности, гипотез, больших ставок и инкрементальных улучшений.

Что взять:

- Разделяй уверенные решения и гипотезы.
- Roadmap полезен как feasibility check, а не как обещание.
- Балансируй big bets и incremental work, если проекту нужны оба типа движения.
- Не допускай roadmap без narrative: порядок должен объяснять направление проекта.
- Полезны вопросы:
  - что самое важное, если можно сделать только одно;
  - какие пункты не двигают проект;
  - где высокая уверенность, а где предположение;
  - что удалить, отложить или явно не делать.

Что не брать:

- Большую библиотеку инсайтов от продуктовых лидеров.
- Жесткую норму распределения ресурсов.
- Сложную фасилитацию stakeholder prioritization.

### `melodic-software/claude-code-plugins@prioritization`

Источник: https://skills.sh/melodic-software/claude-code-plugins/prioritization

Суть: системное ранжирование инициатив через value, effort, risk, dependencies и разные frameworks.

Что взять:

- Легкую prioritization lens:
  - value;
  - strategic fit;
  - effort;
  - risk;
  - dependencies;
  - confidence.
- Для быстрого roadmap достаточно Value/Effort или MoSCoW.
- В `ROADMAP.md` полезно явно фиксировать deferred/not-now items и причины.
- Проверяй, что приоритеты согласованы с видением и ограничениями проекта.

Что не брать:

- Полный набор MoSCoW, Kano, RICE, WSJF, weighted scoring и Mermaid-визуализаций.
- Сохранение `docs/analysis/prioritization.*`.
- Формальные scoring tables как обязательный результат.

### `deanpeters/product-manager-skills@product-strategy-session`

Источник: https://agentskills.so/skills/deanpeters-product-manager-skills-product-strategy-session

Суть: end-to-end продуктовая стратегия: positioning, customer discovery, problem validation, solution exploration, prioritization и roadmap.

Что взять:

- `VISION.md` должен отвечать: для кого проект, какую проблему решает, чем отличается, какие принципы направляют решения.
- Перед roadmap полезно проверить, что понятны target users, problem space и differentiation.
- Решения стоит привязывать к validated/assumed knowledge: где есть факты, где гипотезы.
- Decision points помогают не запускать тяжелую discovery-машину без необходимости.

Что не брать:

- 2-4 недельный workflow.
- Workshops, proto-personas, JTBD, opportunity solution trees, press releases.
- Stakeholder presentation и execution planning.

### `deanpeters/product-manager-skills@prioritization-advisor`

Источник: https://mcpmarket.com/tools/skills/prioritization-advisor

Суть: выбор подходящего фреймворка приоритизации по контексту команды, стадии продукта и типу решения.

Что взять:

- Не применять тяжелый scoring framework автоматически.
- Выбирай самый простой метод, который объясняет решение.
- Учитывай стадию проекта: прототип, MVP, рост, зрелая система, поддержка.
- Если данных мало, лучше фиксировать confidence и assumptions, чем имитировать точность.

Что не брать:

- Adaptive dialogue для выбора фреймворка как отдельную процедуру.
- Детальные scoring templates и alternative framework recommendations.

### `ctsstc/get-shit-done-skills@gsd`

Источник: https://skills.sh/ctsstc/get-shit-done-skills/gsd

Суть: project management system с требованиями, roadmap, phase planning, execution и verification.

Что взять:

- Goal-backward thinking: начинай с того, что должно стать true для проекта.
- Roadmap должен разделять цели, фазы и критерии прогресса.
- Контекст проекта должен быть записан, чтобы следующие этапы не восстанавливали его заново.

Что не брать:

- `.planning/` структуру, 19+ команд, agents, atomic commits, execution waves.
- Phase planning, debugging, verification и shipping workflows.

### `tech-leads-club/agent-skills@decomposition-planning-roadmap`

Источник: https://skills.sh/tech-leads-club/agent-skills/decomposition-planning-roadmap

Суть: техническая roadmap-структура для миграций с текущим состоянием, фазами, рисками, value и dependencies.

Что взять:

- Оценка текущего состояния полезна после фиксации намерения разработчика:
  она проверяет факты, ограничения и риски перед записью дорожной карты.
- Prioritization может учитывать value, risk и dependencies.
- Фазовый roadmap должен иметь milestones и success criteria.
- Roadmap остается живым документом, который обновляется по мере новых знаний.

Что не брать:

- Архитектурные stories и decomposition patterns.
- Техническую migration roadmap как основную модель для любых проектов.
- Детальное отслеживание прогресса по паттернам.

### `anthropics/knowledge-work-plugins@roadmap-update`

Источник: https://skills.sh/anthropics/knowledge-work-plugins/roadmap-update

Суть: обновление roadmap как knowledge artifact.

Что взять:

- Roadmap должен обновлять текущее состояние, а не копить историю изменений.
- При обновлении важно сохранять канон: текущие цели, текущие приоритеты, текущие хвосты.

Что не брать:

- Knowledge-work integration, если она требует внешних workspace-инструментов.
- Change log внутри `ROADMAP.md`.

## Фильтр применимости

Добавляй в `roadmap` только то, что помогает получить самодостаточные `VISION.md` и `ROADMAP.md`.

Подходит:

- формулировка смысла проекта;
- target users / audience;
- problem statement;
- positioning / differentiation;
- project principles;
- goals and outcomes;
- success signals;
- prioritization lens;
- horizons: now / next / later;
- assumptions and confidence;
- dependencies and constraints;
- risks and trade-offs;
- explicit not-now / out-of-scope;
- служебные roadmap-хвосты в `./workflow/PLAN.md`.

Не подходит:

- PRD;
- user stories;
- feature.md;
- sprint/release plan;
- architecture decisions;
- design guideline;
- implementation plan;
- backlog management;
- stakeholder workshop;
- customer interview workflow;
- scoring spreadsheet;
- external issue tracker;
- git workflow.

## Рекомендуемый алгоритм `roadmap`

1. Прочитай минимальный существующий контекст: `./workflow/PROJECT.md`, `./workflow/VISION.md`, `./workflow/ROADMAP.md` и `./workflow/PLAN.md`, если они есть. Используй его для языка, текущего канона и формы проекта, а не для преждевременного вывода направления.
2. Если намерения пользователя неясны, сначала задай открытые вопросы о стадии проекта, целях, аудитории, горизонте, приоритетах, ограничениях, признаках успеха и `Not Now`.
3. Прими заявленное разработчиком направление как первичный стратегический вход.
4. Подними минимальный контекст из кода и документации только после пользовательского ввода: для проверки фактического состояния, ограничений, подтверждений и противоречий.
5. Выдели отдельно заявленное направление, локальные подтверждения, локальные противоречия, ограничения, assumptions и low-confidence guesses.
6. Если факты проекта конфликтуют с заявленным направлением и это влияет на roadmap, задай 1-3 уточняющих вопроса с вариантами ответа.
7. Сформулируй `VISION.md`: смысл, пользователи, проблема, принципы, non-goals, признаки успеха.
8. Сформулируй `ROADMAP.md`: цели, outcomes, горизонты, приоритеты, зависимости, риски, assumptions, not-now.
9. Используй легкую prioritization lens: value, strategic fit, effort, risk, dependencies, confidence.
10. Отдели уверенные направления от гипотез.
11. Не превращай roadmap в список фич: у каждого направления должен быть outcome или rationale.
12. В `./workflow/PLAN.md` добавь только служебные roadmap-хвосты, если нужны дополнительные решения.
13. Запиши документы как текущее состояние проекта, без истории изменений и сравнений с прежними версиями.

## Рекомендуемая структура `VISION.md`

```md
# VISION.md

## Смысл

Кратко опиши, зачем существует проект.

## Пользователи

- Основная аудитория
- Вторичная аудитория, если есть
- Кто не является целевой аудиторией

## Проблема

- Главная проблема
- Почему она важна
- Что меняется для пользователя

## Позиционирование

- Чем проект является
- Чем проект не является
- Чем отличается от альтернатив

## Принципы

- Принцип 1
- Принцип 2
- Принцип 3

## Признаки успеха

- Пользовательский результат
- Проектный результат
- Качественный или количественный сигнал

## Ограничения

- Технические
- Продуктовые
- Ресурсные

## Non-goals

- Что проект сознательно не делает
```

## Рекомендуемая структура `ROADMAP.md`

```md
# ROADMAP.md

## Направление

Кратко опиши текущий стратегический фокус.

## Цели

### Goal 1: Название

Outcome: Enable [user/segment] to [desired outcome] so that [project impact].

Success signals:

- Сигнал 1
- Сигнал 2

Confidence:

- High / Medium / Low

Assumptions:

- Гипотеза или условие

## Горизонты

### Now

- [ ] Направление или крупная цель
  - Outcome:
  - Rationale:
  - Dependencies:
  - Risk:

### Next

- [ ] Направление или крупная цель
  - Outcome:
  - Rationale:
  - Dependencies:
  - Risk:

### Later

- Направление или цель без обещания срока

## Not Now

- Что явно отложено
- Почему отложено

## Риски и компромиссы

- Risk -> mitigation / decision needed

## Roadmap-хвосты

- Вопросы, которые нужно решить на уровне видения, цели или приоритета
```

## Правила качества roadmap-артефактов

- `VISION.md` и `ROADMAP.md` можно читать без пересказа чата.
- Документы описывают текущее состояние, без биографии решений и временных дельт.
- Roadmap объясняет outcomes, а не только outputs.
- Приоритеты имеют rationale.
- Есть явный `Not Now`, чтобы отложенное не выглядело забытым.
- Assumptions и confidence не маскируются под факты.
- Дорожная карта не создает фичевые документы и не планирует реализацию.
- `PLAN.md` получает только служебные roadmap-хвосты.

## Команды исследования

```bash
npx skills find "roadmap"
npx skills find "product roadmap"
npx skills find "product strategy"
npx skills find "vision roadmap"
npx skills find "product prioritization"
npx skills find "outcome roadmap"
npx skills find "product discovery"
npx skills find "product requirements"
```
