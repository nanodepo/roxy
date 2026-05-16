# REFERENCES — исследование внешних скилов для `feature`

## Метод

Через `find-skills` (`npx skills find`) собраны скилы экосистемы `skills.sh`, близкие по сути к нашему `feature` — разбор сырого запроса пользователя в самодостаточный feature brief, уточнение требований, scoping. Каждый кандидат разобран на предмет того, что усилит наш скил, не выйдя за его границы (brief богаче одной фразы, но беднее PRD и плана; скил не принимает решений о реализации, дизайне, тестах; артефакты — `feature.md` и строка в `PLAN.md`).

## Найденные скилы

| Скил | Ссылка | Суть | Вердикт |
|---|---|---|---|
| `mingyuepop/specforge@feature-requirements-clarification` | https://skills.sh/mingyuepop/specforge/feature-requirements-clarification | Сократические вопросы → структурированный FRD: ценность, user stories, acceptance criteria, краевые условия | Взять фиксацию краевых случаев, FRD и acceptance criteria отклонить |
| `steveclarke/dotfiles@feature-requirements` | https://skills.sh/steveclarke/dotfiles/feature-requirements | Три фазы discovery → scoping → consolidation, единый `requirements.md`, защита от scope creep | Взять scoping-дисциплину, фазовый TODO-трекинг отклонить |
| `jasonkneen/kiro@requirements-engineering` | https://skills.sh/jasonkneen/kiro/requirements-engineering | Методология EARS — специфичные, тестируемые, недвусмысленные требования | Отклонить целиком — формализация тестируемых требований не наш этап |
| `oimiragieo/agent-studio@interactive-requirements-gathering` | https://skills.sh/oimiragieo/agent-studio/interactive-requirements-gathering | Интерактивные опросники, принцип Question Classification — классифицировать вопрос до того, как задать | Взять классификацию вопросов, формат опросника отклонить |
| `bmad-code-org/bmad-method@bmad-product-brief` | https://skills.sh/bmad-code-org/bmad-method/bmad-product-brief | Коуч продуктового анализа: pressure-test допущений, брифы «честные, right-sized», без раздувания | Взять pressure-test допущений, bmad-машинерию отклонить |

## Принятые усиления

Четыре блока дают метод уже существующим шагам скила и усиливают уже существующие секции `feature.md`. Два из них (A, B) работают как фильтры — делают brief точнее и короче, а не толще. Все согласованы с принципами (достаточное настоящее) и удерживают границу «беднее PRD и плана».

### A. Классификация уточняющего вопроса перед тем, как его задать

Скил уже задаёт короткие уточняющие вопросы. Добавить фильтр отбора: задавать только вопрос, ответ на который меняет суть, проблему или границы brief. Вопросы о реализации, архитектуре, дизайне и тестах не задавать — это следующие этапы конвейера. Что не прошло фильтр и осталось неясным — уходит в «открытые вопросы», а не в диалог.
Источник: `interactive-requirements-gathering`.

### B. Pressure-test допущений запроса

При разборе запроса находить скрытые непроверенные допущения — что считается само собой разумеющимся о пользователе, проблеме или результате. Именно туда направлять уточняющий вопрос либо, если он не прошёл фильтр A, явный «открытый вопрос». Brief остаётся right-sized: фиксирует смысл и намерение без раздувания и домыслов.
Источник: `bmad-product-brief`.

### C. Явная фиксация краевых и исключительных случаев

При разборе функциональности отмечать очевидные краевые и исключительные сценарии. Фиксировать их строго как «границы» или «открытые вопросы» — без описания ожидаемого поведения и без формы acceptance criteria. Цель — чтобы планирование их не упустило, а не спроектировать их здесь.
Источник: `feature-requirements-clarification`.

### D. Scoping как осознанный шаг

Явно отделять то, что входит в фичу, от того, что за её границей. Усиливает секции «границы» и «не-цели»: не-цель — это не просто «чего не делаем», а сознательно отрезанный соседний объём, который иначе расползётся в plan.
Источник: `feature-requirements`.

## Сознательно отклонено

| Что | Источник | Причина |
|---|---|---|
| FRD (Functional Requirements Document) как формат вывода | `feature-requirements-clarification` | Полноценный requirements-документ — конфликт с границей «беднее PRD и плана» |
| Синтаксис EARS для тестируемых требований | `requirements-engineering` | Формализация тестируемых требований — домен `planning` и `test`, не `feature` |
| Acceptance criteria в brief | `feature-requirements-clarification` | Прямой запрет в README: brief не пишет acceptance criteria как тестовый план |
| Трёхфазный TODO-трекинг через task-инструменты | `feature-requirements` | Операционная машинерия, перегруз для короткого входного скила |
| Интерактивные опросники как формат сбора | `interactive-requirements-gathering` | Скил задаёт короткие точечные вопросы, а не ведёт опросник |
| Intent detection, party-mode, advanced-elicitation, config.yaml, headless | `bmad-product-brief` | Тяжёлая чужая инфраструктура bmad, несовместима с конвейером |
| User stories по ролям как обязательная структура | `feature-requirements-clarification`, `feature-requirements` | Brief фиксирует «пользователя или контекст» свободным текстом; навязанный формат — шаг к PRD |
| Многоитерационный сократический диалог | `feature-requirements-clarification`, `bmad-product-brief` | Конфликтует с лёгкостью brief и требованием коротких уточняющих вопросов |
