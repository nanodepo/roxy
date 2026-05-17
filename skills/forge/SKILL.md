---
name: forge
description: >
  Кузница кастомных скилов: собирает `skills/{name}/SKILL.md` по `README.md`
  и проверенному `REFERENCES.md`. Триггеры: «собери скил», «сделай скил»,
  «кузница скилов».
---

# Forge — сборка кастомного скила по локальной спецификации

## Назначение

Forge собирает кастомный `SKILL.md` под рабочий процесс пользователя. Forge — мета-скил: не пишет код продукта, собирает скилы.

Рабочая папка целевого скила:

- **`skills/{name}/README.md`** — канон намерений: назначение, ответственность, связь с конвейером, входной контекст, выходные артефакты, границы. При конфликте с любым другим материалом `README.md` главнее.
- **`skills/{name}/REFERENCES.md`** — проверяемые источники и заметки для усиления скила. Используй их как вдохновение, инсайты для улучшения, стандартизации и укрепления инструкций. То, что усиливает `README.md` и не противоречит ему, бери. То, что противоречит, не подходит по духу или не подтверждается источниками, отсеивай.

Результат — один файл `skills/{name}/SKILL.md` (и при необходимости `skills/{name}/references/`).

## Параметры

Строка `args`:

```
<skill-name>
```

- `skill-name` — имя целевого скила. Совпадает с именем уже существующей папки `skills/{skill-name}/`, в которой лежат `README.md` и `REFERENCES.md`.

Нет ровно одного токена, имя не совпадает с `^[a-z0-9]+(-[a-z0-9]+)*$`, папки `skills/{skill-name}/` нет, отсутствует `README.md` или отсутствует `REFERENCES.md` → остановись, объясни формат и перечисли доступные папки из `skills/`.

## Жёсткие правила

- **`mcp__sequential-thinking__sequentialthinking`** обязателен на этапе 4. Это аналитический этап — без него план сборки не строится.
- **Никаких git-операций.** Не `git status`, не `git diff`, не ветки, не коммиты. Рабочее дерево всегда грязное — это норма, пользователь сам управляет git.
- **Forge правит только** `skills/{skill-name}/SKILL.md` и при необходимости `skills/{skill-name}/references/`. Файлы `skills/{skill-name}/README.md` и `skills/{skill-name}/REFERENCES.md` не трогает: по ним скил можно пересобрать заново.
- Создаваемый `SKILL.md` пиши на английском. Имена скилов, MCP-инструментов, путей, ключей frontmatter, команд — в оригинале.
- Не переноси в `SKILL.md` биографию создания, дельта-формулировки и временные заметки из `REFERENCES.md`. Итоговый скил описывает текущее правило так, чтобы исполнитель мог работать без истории его появления.

## Language Notice

Write this `SKILL.md` in English.

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

## Этапы

Процедура многошаговая, с вложенными `mcp__sequential-thinking__sequentialthinking` и вопросами пользователю. Заведи в начале список из 7 пунктов в режиме планирования агента (todo-список / план задач — что доступно) и закрывай по одному. После каждого вложенного вызова следующее действие — закрыть текущий пункт и перейти к следующему.

### 1. Разобрать `args`

Разбей строку по пробелам. Ожидается 1 токен: `skill-name`. Не так → сообщи формат, перечисли доступные папки из `skills/`, остановись.

Проверь имя по шаблону `^[a-z0-9]+(-[a-z0-9]+)*$`. Имя не прошло проверку → остановись и попроси имя папки скила.

### 2. Прочитать локальную спецификацию

- `Read skills/{skill-name}/README.md` — канон намерений целевого скила. Папки нет или README отсутствует → остановись, объясни, что forge собирает скил по уже существующей спецификации.
- `Read skills/{skill-name}/REFERENCES.md` — источники, примеры, заметки и идеи для усиления. Файл отсутствует → остановись, объясни, что forge требует локальный `REFERENCES.md`.
- `Bash find skills/{skill-name} -maxdepth 2 -type f | sort` — посмотри, какие дополнительные материалы уже есть в папке.
- `Bash test -f skills/{skill-name}/SKILL.md`. Файл уже есть → спроси: обновить/перезаписать или прервать. Без автоматической перезаписи.

### 3. Проверить источники из REFERENCES.md

Разбери `skills/{skill-name}/REFERENCES.md` как список проверяемых источников и рабочих заметок.

- Локальные пути читай напрямую. Если путь относительный, сначала считай его относительным к `skills/{skill-name}/`, затем к корню репозитория.
- Внешние URL проверяй доступным web-инструментом. Если web-инструмента нет или источник недоступен, пометь источник как неподтверждённый и не превращай его утверждения в обязательные правила.
- Если `REFERENCES.md` ссылается на установленный скил, прочитай его `SKILL.md` и только явно релевантные `references/`, `scripts/`, `assets/`.
- Для каждого источника отдели подтверждённый материал от пересказов, пожеланий и операционного мусора.
- Сравни каждую полезную идею с `README.md`: поддерживает / уточняет / нейтральна / противоречит / не подходит по духу.
- При расхождении сохраняй намерение из `README.md`; материал из `REFERENCES.md` используй только как повод точнее сформулировать канон.

Итог этапа — рабочая таблица: источник, что проверено, какие инсайты пригодны, что отсеивается и почему.

### 4. Анализ и план (sequential-thinking)

Через `mcp__sequential-thinking__sequentialthinking` ответь:

- Что `README.md` задаёт как ответственность, границы («Не делает»), выходные артефакты, входной контекст и критерий готовности целевого скила.
- Какие идеи из `REFERENCES.md` и проверенных источников усиливают `README.md` без противоречий.
- Что из `REFERENCES.md` отсеивается: противоречит `README.md`, не подтверждается источником, не подходит по духу скила, является биографией, дельтой или временной заметкой.
- Нужны ли файлы `skills/{skill-name}/references/`: какие материалы вынести туда, какие объединить, какие не создавать.
- Аналитический ли это скил (создание фичи, планирование, разбивка плана на задачи, улучшение плана, документирование, анализ кода или документации) → тогда в его `SKILL.md` нужен обязательный вызов `mcp__sequential-thinking__sequentialthinking` на этапах размышления.
- Какие вопросы целевой скил задаёт **своему** пользователю при запуске (а не вопросы forge).
- Длинная ли у целевого скила процедура с вложенными вызовами → стоит ли рекомендовать ему режим планирования задач.
- Как итоговый `SKILL.md` останется самодостаточным: какие правила должны быть в теле, а какие справочные детали читать по требованию.
- Что осталось неясным — список для этапа 5.

Зафиксируй итог как структурированный план сборки.

### 5. Уточняющие вопросы (условно)

Сначала попробуй закрыть каждый неясный вопрос из материалов: `skills/{skill-name}/README.md`, `skills/{skill-name}/REFERENCES.md`, проверенные источники, файлы проекта (`./workflow/VISION.md`, `./workflow/ROADMAP.md`, `./workflow/PROJECT.md`, `./workflow/ARCHITECTURE.md` — `Read` по необходимости), релевантный код. Ответ выводится из материала → вопрос не задаётся.

Оставшиеся вопросы задай пользователю: группируй независимые в один блок, для каждого вопроса первый вариант — рекомендованный, с пометкой «(Recommended)» и кратким обоснованием. Каждый вопрос — про работу целевого скила, не про forge.

Этап 4 не оставил вопросов → этап 5 пропускается.

### 6. Написать SKILL.md

По плану этапа 4 и ответам этапа 5 составь `SKILL.md` по разделу «Как писать SKILL.md целевого скила» ниже. `Write skills/{skill-name}/SKILL.md`.

Файлы `skills/{skill-name}/references/` создавай только если справочный материал нужен по требованию и перегружает основной `SKILL.md`.

- **Переписать** — создай новый файл `skills/{skill-name}/references/{file}` с текущим каноном без биографии и дельт.
- **Объединить** — сведи несколько источников в один файл `skills/{skill-name}/references/{file}`, оставь только подтверждённое и применимое.
- **Не создавать** — если материал короткий, очевидный, неподтверждённый, противоречит `README.md` или не нужен исполнителю.

`SKILL.md` ссылается на каждый файл `references/` по имени, относительным путём, с явным указанием, **на каком этапе** и **зачем** его читать. Глубже одного уровня ссылки не делай.

### 7. Отчёт

5–8 строк:

- путь к созданному `skills/{skill-name}/SKILL.md`;
- как `README.md` определил ответственность, границы и артефакты;
- какие источники из `REFERENCES.md` проверены и какие идеи использованы;
- что из `REFERENCES.md` отсеяно и почему;
- судьба `skills/{skill-name}/references/`;
- вызывает ли целевой скил `mcp__sequential-thinking__sequentialthinking` и почему;
- если был этап 5 — что уточнено у пользователя;
- что делать дальше: протестировать скил на пилотной задаче.

## Как писать SKILL.md целевого скила

### Frontmatter

```yaml
---
name: {skill-name}
description: >
  {what the skill does, 1-2 sentences} + {when to use it} +
  {explicit English trigger phrases}.
---
```

- `name` — только строчные латинские буквы, цифры и дефисы; без дефиса в начале и конце; без двойных дефисов; точно совпадает с именем папки `skills/{skill-name}/`.
- `description` — write it in English, aim for ~200 characters, hard ceiling 512. Use third person ("Creates ...", not "I create ..."). This is the main trigger surface and the only text the agent sees before activation. Include the core action, when to use the skill, and explicit English trigger phrases. Keep it precise and avoid vague phrases like "helps with documents".

### SKILL.md Body

Use this literal English section structure for the generated `SKILL.md`; omit
sections that do not apply:

```markdown
# {Skill Title}

## Purpose
What the skill does, what it relies on, and what it does not do.

## Parameters
The `args` format, required and optional parameters, and behavior when they are missing.
No parameters -> omit this section.

## Strict Rules
Invariants for every step: no git operations, English skill text,
required `mcp__sequential-thinking__sequentialthinking` (if the skill is analytical),
updating `./workflow/PLAN.md` (if the skill works with features).

## Language Notice
{standard Language Notice copied from this `forge` skill}

## Steps
Numbered imperative steps: what to read, what to write, which tools to call.

## Artifact Requirements
Which files the skill creates, their structure, and their format.

## Updating PLAN.md (if applicable)
What to record in `./workflow/PLAN.md` at the end and where.

## Notes
Special cases, known constraints, and behavior when files are missing.
```

### Style

- **English.** Write the generated skill in English. Keep skill names, MCP tool names, paths, frontmatter keys, commands, and code identifiers in their original spelling.
- **Imperative voice.** Use "Read X", "Write Y", "Ask Z". Avoid passive forms like "X should be read".
- **Complete before compressed.** The base `SKILL.md` may be detailed. Prefer clear, explicit instructions over premature brevity; later processing can compress the text without changing meaning.
- **Relevant detail only.** Include what the executing agent cannot infer from default behavior: project conventions, `./workflow/` structure, non-obvious constraints, and exact artifact rules. Explain why only where the reason affects execution.
- **Concrete names.** Use full paths, MCP tool names, and file names (`mcp__sequential-thinking__sequentialthinking`, `./workflow/PLAN.md`, `./workflow/features/{slug}/feature.md`). Always use full MCP tool names so the tool can be found.
- **Default over menu.** Give one recommended approach plus a short alternative, not a list of equal options.
- **Strictness follows fragility.** Fragile or irreversible steps and critical ordering need exact instructions. When several approaches are valid, give direction and the reason.
- **Self-contained present.** Write for an executing agent without creation context: it sees only `SKILL.md`, `references/`, and project files. `SKILL.md` describes current operation without biography or comparisons to previous states.
- **Consistent terminology.** Use one term for one concept throughout the file.
- **Forward slashes** in paths.

### References for the Target Skill

- `references/*.md` is loaded on demand from a `SKILL.md` step; this is progressive disclosure. Put bulky reference material there and keep the core workflow in `SKILL.md`.
- For every `references/` file, `SKILL.md` states **when** to read it ("Read `references/{file}` if ..."), not just "see references".
- Link only one level down from `SKILL.md`. If a reference file is longer than 100 lines, start it with a table of contents.
- Create `scripts/` and `assets/` only when there is a real need; ask the user before creating them.

### What Must Not Go Into SKILL.md

- Mentions of forge: the target skill does not know forge assembled it.
- Stories about previous skill versions or delta instructions.
- Git operations in any form.
- Dependencies on other environment skills: the target skill is self-contained.

## Конвенции создаваемых скилов

Каждый собранный forge скил соответствует пунктам ниже. Этап 4 — отметь применимые, этап 6 — встрой в `SKILL.md`.

- **Frontmatter** with `name` and `description` by the rules above.
- **Generated skill text — in English**, with names and identifiers in their original spelling.
- **Language Notice** copied into every generated skill so project artifacts,
  headings, labels, placeholders, and examples follow the target project's
  working language instead of inheriting English templates.
- **No git operations** in any form: do not check status, create branches, commit, or push.
- **The skill follows its instructions literally**, without shortening steps "as needed".
- **`mcp__sequential-thinking__sequentialthinking` is required** if the skill is analytical: feature creation, planning, task breakdown, plan improvement, documentation, code analysis, or documentation analysis. `SKILL.md` explicitly instructs when to call it during reasoning steps.
- **Long procedures with nested calls** should recommend the agent's built-in task planning mode. Use agent-neutral wording: "task planning mode (todo list / task plan, whichever is available)". Atomic 1-2 step skills do not mention task planning mode.
- **Sufficient present**: the final `SKILL.md` and its `references/` give the executing agent current rules without hidden history.
- **No biography or deltas**: do not describe previous states, rule replacements, or removed constraints. Only the active rule goes into the final artifact.
- **Tests protect invariants**: if the target skill writes tests, formulate test requirements around live behavior, not around incidents or removal facts.

## Замечания

- Ответственность, границы и артефакты целевого скила определяет `skills/{skill-name}/README.md`. Forge не вносит в `SKILL.md` ничего, что противоречит этой спецификации.
- `skills/{skill-name}/REFERENCES.md` — источник идей и проверяемых материалов, а не источник обязательных правил.
- Если при работе обнаружится, что `README.md` неполон или внутренне противоречив — не угадывай, задай вопрос на этапе 5.
