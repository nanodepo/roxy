---
name: forge
description: >
  Meta-skill that assembles `skills/{name}/SKILL.md` from the intent canon in
  `skills/{name}/README.md`. Use when the user says "forge a skill", "build a
  skill", "assemble a skill", or asks to regenerate a skill from its README.
---

# Forge — сборка скила по канону намерений

## Назначение

Forge — мета-скил: он не пишет код продукта, он собирает скилы. Вход — единственный источник истины `skills/{name}/README.md`: назначение скила, его место в конвейере, входы, выходы, границы («не делает»), условия применения. Результат — один файл `skills/{name}/SKILL.md`, написанный по конвенциям из этого файла; при реальной необходимости — справочные файлы `skills/{name}/references/`.

Forge направляет порождаемый скил, а не диктует ему каждое действие: фиксирует цель этапа, границы, формат артефакта, нетривиальные ограничения и принципы — и не фиксирует микроменеджмент.

## Параметры

Строка `args`:

```
<skill-name>
```

- `skill-name` — имя целевого скила; совпадает с именем существующей папки `skills/{skill-name}/`, в которой лежит `README.md`.

Нет ровно одного токена, имя не совпадает с `^[a-z0-9]+(-[a-z0-9]+)*$`, папки `skills/{skill-name}/` нет или в ней нет `README.md` → остановись, объясни формат и перечисли доступные папки из `skills/`.

## Жёсткие правила

- Никаких git-операций в любой форме: ни чтения статуса и диффов, ни веток, коммитов и push. Грязное рабочее дерево — норма, git принадлежит пользователю.
- Forge правит только `skills/{skill-name}/SKILL.md` и при необходимости `skills/{skill-name}/references/`. Файл `skills/{skill-name}/README.md` не трогает: по нему скил можно пересобрать заново.
- Ответственность, границы и артефакты целевого скила определяет `README.md`. Forge не вносит в `SKILL.md` ничего, что противоречит этому канону; пробелы канона закрываются вопросами пользователю, а не догадками.
- Тело порождаемого `SKILL.md` — английский; имена скилов, путей, команд, ключей frontmatter — в оригинальном написании.
- Порождаемый скил не ссылается на внешние «инструменты размышления» (MCP и подобные): аналитическая работа описывается как обычные шаги рассуждения.

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

Для forge порождаемый `SKILL.md` — служебный артефакт экосистемы скилов и пишется на английском; правило выше он несёт внутри себя, чтобы артефакты целевого проекта следовали рабочему языку проекта.

## Этапы

Процедура многошаговая — заведи список пунктов в режиме планирования задач (todo-список / план задач, что доступно) и закрывай по одному.

1. **Разобрать `args`.** Один токен `skill-name`, проверка имени и папки по правилам раздела «Параметры». Не сошлось → остановись и объясни.

2. **Прочитать канон.** `Read skills/{skill-name}/README.md`. Посмотри содержимое папки (`find skills/{skill-name} -maxdepth 2 -type f | sort`). Если `skills/{skill-name}/SKILL.md` уже существует — спроси пользователя: перезаписать или прервать; без автоматической перезаписи.

3. **Проанализировать и спланировать.** Обычными шагами рассуждения выведи из `README.md`: ответственность и границы скила, входной контекст, выходные артефакты и их формат, место в конвейере и правило рекомендации следующего шага, вопросы, которые скил задаёт своему пользователю, многошаговая ли у него процедура. Подними минимальный дополнительный контекст только когда он нужен для решения: соседние `SKILL.md` той же ветки конвейера, артефакты `./workflow/` целевого проекта. Зафиксируй план сборки и список неясностей.

4. **Уточнить у пользователя (условно).** Сначала закрой каждую неясность материалом: README, соседние скилы, файлы проекта. Оставшиеся вопросы задай одним батчем; в каждом вопросе первый вариант — рекомендованный, с пометкой «(Recommended)» и кратким обоснованием. Вопросы — про работу целевого скила, не про forge. Неясностей нет → этап пропускается. В записанный `SKILL.md` открытые вопросы не попадают.

5. **Написать `SKILL.md`.** По плану и ответам составь файл по разделам «Каркас порождаемого SKILL.md» и «Конвенции порождаемых скилов». `Write skills/{skill-name}/SKILL.md`. Файлы `skills/{skill-name}/references/` создавай только когда объёмный справочный материал реально нужен по требованию и перегружает основной файл; каждый такой файл `SKILL.md` называет по относительному пути и говорит, на каком шаге и зачем его читать. Глубже одного уровня ссылки не делай.

6. **Отчёт.** Коротко в чат: путь к созданному `skills/{skill-name}/SKILL.md`; как README определил ответственность, границы и артефакты; что уточнялось у пользователя (если уточнялось); судьба `references/`. Финальная строка — рекомендация следующего шага: прогнать скил на пилотной задаче.

## Каркас порождаемого SKILL.md

### Frontmatter

```yaml
---
name: {skill-name}
description: >
  {what the skill does, 1-2 sentences} + {when to use it} +
  {explicit English trigger phrases}.
---
```

- `name` — строчные латинские буквы, цифры и дефисы; без дефиса по краям и двойных дефисов; точно совпадает с именем папки.
- `description` — English, third person, aim for ~200 characters (hard ceiling 512). This is the only text the agent sees before activation: state the core action, when to use the skill, and explicit trigger phrases. Avoid vague phrases like "helps with documents".

### Структура тела

Используй эту английскую структуру секций; секции, которые не применимы, опускай:

```markdown
# {Skill Title}

## Purpose
What the skill does, what it relies on, and what it does not do — including
which neighboring pipeline stages own the work it must not take over.

## Parameters
The `args` format and behavior when arguments are missing or invalid.

## Strict Rules
Invariants for every step. Always includes: no git operations of any kind;
the user owns git, a dirty tree is expected.

## Language Notice
{standard Language Notice block copied from this forge skill}

## Steps
Numbered imperative steps: what to read, what to ask, what to write. Questions
to the user are batched before the artifact is written, with the recommended
option first and marked "(Recommended)".

## Artifact Requirements
Which files the skill creates or updates, their core sections, and their format.

## Updating PLAN.md (if applicable)
What to record in `./workflow/PLAN.md` at the end and where.

## Notes (optional)
Special cases and behavior when expected files are missing.
```

Целевой объём — примерно 100–180 строк; для простых скилов меньше.

## Конвенции порождаемых скилов

Каждый собранный forge скил соответствует пунктам ниже; на этапе 3 отметь применимые, на этапе 5 встрой в текст.

- **Стиль** — лаконичный английский полными предложениями (не телеграфный «ctx/impl/cmd»-стиль), императив («Read X», «Write Y», «Ask Z»), конкретные полные пути и имена файлов (`./workflow/PLAN.md`, `./workflow/features/{slug}/feature.md`), объяснение «почему» только там, где причина влияет на исполнение. Одна рекомендованная стратегия с короткой альтернативой вместо меню равных опций; точные инструкции — только для хрупких или необратимых шагов.
- **Направлять, а не диктовать.** Скил фиксирует цель этапа, границы, формат артефакта, нетривиальные ограничения и принципы. Без исчерпывающих таксономий, чек-листов из десятков пунктов и перечисления очевидного. То же — для порождаемых им артефактов: размер пропорционален сложности, «пары предложений достаточно» — валидный полный артефакт для простой задачи.
- **Шаблоны артефактов** — минимальное ядро секций (3–5) плюс правило «добавляй секцию, только если она несёт решение». Без гигантских заготовок таблиц и длинных списков опциональных секций.
- **Граница этапа.** Скил явно и кратко фиксирует, что в его компетенции, а что принадлежит соседним этапам конвейера, и не расширяет свою задачу.
- **Pipeline awareness.** Скил знает своё место в конвейере; финальный отчёт в чат заканчивается рекомендацией следующего шага (одна-две строки, с именем скила), выведенной из фактического состояния. Скил не вызывает следующий скил сам.
- **Вопросы пользователю** задаются до записи артефакта, одним батчем, рекомендованный вариант первым с пометкой «(Recommended)». В записанных артефактах не остаётся открытых вопросов, закрываемых диалогом.
- **Без MCP-инструментов размышления** и зависимостей от других скилов окружения: аналитические шаги — обычные шаги рассуждения, скил самодостаточен и работает из своего `SKILL.md` и `references/`.
- **Достаточное настоящее.** Артефакты описывают текущее состояние в настоящем времени, без биографии создания и формулировок «было X, стало Y»; одна каноническая формулировка на правило; устаревшее удаляется, а не дописывается рядом — забывание это обслуживание. Исполнитель артефакта работает без истории его появления.
- **Тесты защищают инварианты** (если скил касается тестов): живое поведение, не инциденты и не факты удаления.
- **Режим планирования задач** упоминается одной строкой и только в многошаговых скилах, в нейтральной формулировке: «task planning mode (todo list / task plan, whichever is available)».
- **Скил не знает о forge**: никаких упоминаний сборки, прежних версий и сравнений с прошлым состоянием.
