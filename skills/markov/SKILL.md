---
name: markov
description: >
  Normalizes a single local document by the Markov laws of agentic development —
  rewrites the passed file in place so it describes a sufficient present, free of
  creation biography, delta wording, and operational clutter. Use when the user
  says «нормализуй документ», «примени законы Маркова», «приведи файл к канону»,
  «markov этот файл», or names a file to clean up by the Markov laws.
---

# Markov — Normalize a Document by the Markov Laws

## Purpose

This skill normalizes one passed local document by the Markov laws of agentic
development. It reads the file, classifies its type, raises only the local
context needed to judge current truth, applies the full set of laws iteratively,
and rewrites the passed file in place.

The skill edits the document directly — it does not produce a recommendation
report instead of the edit. It works on exactly one file at a time: a markdown
document, an instruction file, a class, or a test file. It does not audit the
whole system and does not create new artifacts.

The skill works fully from this `SKILL.md`. The complete law set is embedded
below as its working base; no other file is required.

## Parameters

`args` is the path to one local file:

```txt
<path-to-document>
```

- The path points to a single existing local file.
- `args` is empty, or the user gave no path → stop and ask for one local file
  path. Do not guess a target.
- `args` names more than one path → stop and ask which single file to normalize.
  The skill normalizes one document per run.
- The path does not exist or is not readable → stop and report the path back to
  the user.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs,
  branches, commits, pushes, or checkout commands. The working tree is expected
  to be dirty; the user manages git.
- Call `mcp__sequential-thinking__sequentialthinking` at the analysis step
  (Step 4), after reading the document and its context and before rewriting.
  This is the reasoning step where the per-law normalization plan is built.
- Rewrite only the file passed in `args`. Do not edit related documents,
  neighboring artifacts, or project code, even when they share the same problem.
- Do not produce a separate audit file. The output is the rewritten document
  plus a short chat summary.
- Do not follow or fetch external links. Work only with local context.
- Do not change the meaning of any law below when applying it.

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

## The Markov Laws

These laws are the working base. The analysis step walks every law; the rewrite
step applies the ones that bite the passed document.

### Core thesis

The Markov property: given a sufficiently complete present state, the future
does not depend on hidden past. For agents this is an engineering principle, not
philosophy. An agent does not remember — it has only what is in its context
right now: code, tests, documentation, instructions, recent command results. The
past exists for an agent only when it is materialized in the present.

The discipline is not to accumulate traces but to normalize state: not "make the
agent know what happened" but "make what exists sufficient". Memory is not an
automatic asset — in agentic work it often becomes clutter with authority: an
artifact that looks like knowledge but is the residue of an old task.

### Definitions

- **Present state** — current code, tests, configuration, documentation, active
  requirements, live constraints.
- **Memory** — any artifact that tells what used to be, what changed, or what a
  previous agent believed.
- **Operational context** — what the agent uses to make a decision right now.
- **Audit context** — historical evidence kept for humans, debugging, rollback,
  or compliance; not meant to be fed automatically into an agent's reasoning.
- **Canonical state** — the minimal maintained set of artifacts that defines
  what is true now.

### Law 1 — Sufficient present

An agent must not need hidden history to take the correct next step. If a future
agent would need the past, the present is underdefined. Fix the present, do not
teach the agent biography.

### Law 2 — Canonical state

A project has one canonical form of current truth. Do not keep stale rules as
"used to be X, now Y" — replace them with the current rule, or delete them if
the default behavior already fits. When two artifacts conflict, the agent must
resolve state, not average evidence; the canonical artifact carries visible
authority: source of truth, owner, scope, status (active, deprecated,
superseded, audit-only).

Bad: `Git was forbidden, but now it can be used.`
Better: `Use the normal git workflow when required.`
Often best: `<!-- no rule -->`

### Law 3 — No biography

Documentation describes the system, not its life story. Do not write "the button
was moved from the right panel to the toolbar". Write only the current
requirement, and only if it is genuinely needed: "the primary action lives in
the toolbar next to the filters". If the code or UI already makes it obvious,
write nothing.

### Law 4 — Burden of retention

The burden of proof lies on history. Before keeping historical information, it
must answer at least one question:

- Does it explain a current non-obvious constraint?
- Does it protect a live invariant?
- Is it needed for legal, regulatory, security, or migration reasons?
- Is it needed for safe operation or recovery of the system?
- Is it the only reliable evidence of a current decision?

If not — remove it from operational context.

### Law 5 — Quarantine of history

Past events may be stored, but they must not automatically become instructions
for the agent. Historical traces live in audit logs, changelogs, task threads,
commit history, ADRs with an explicit status. They are not mixed into
`AGENTS.md`, code rules, test names, or everyday task context unless they govern
current behavior. Audit is evidence; operational text is instruction — mixing
them is how stale history becomes policy. When exact past truly matters, keep
the raw evidence in the audit layer and show the agent a distilled rule.

### Law 6 — Invariant, not episode

A test must justify its existence. Every test protects an invariant. If the
invariant is already guaranteed by the type system, the language contract, the
standard library, or the obviousness of the implementation — the test is not
needed. Coverage for its own sake is clutter with authority: it creates false
confidence, lengthens CI, obstructs refactoring, and steals review attention.

A test of a function returning `a % b` tests the language, not your system.
Tests cover what types cannot express: a product invariant, an edge case, a side
effect, a data-state invariant, a non-trivial integration. Tests protect live
requirements, not memories of removed behavior. A regression test is justified
when it encodes a product invariant; a test that merely records the
disappearance of an old element turns the past into eternal law.

Bad test meaning: `The old button is gone.` / `The function returns a
remainder.` / `The getter returns the value we set.`
Good test meaning: `A user without delete permission cannot perform a
destructive action.` / `The billing period starts on the first business day of
the month.` / `Resubmitting a payment does not create a second charge.`

Test names describe the invariant, not the incident or a language operation:
`guest_cannot_delete_project`, `checkout_requires_confirmed_payment_method`,
`billing_period_starts_on_first_business_day` — not `removed_old_delete_button`,
`fixes_bug_1234`, `modulo_returns_remainder`, `getter_returns_set_value`. If code
becomes non-trivial later, write the test then.

### Law 7 — Rewrite, not append

When reality changes, rewrite the canon. Agents tend to think in deltas ("was X,
became Y"); for an agentic project a clean replacement is almost always better.
Normalize instead of appending: delete stale rules, rename tests around current
behavior, rewrite documentation as if the system had always been in its current
form, fold temporary notes into the final state, remove migration scaffolding
once it has no operational role.

### Law 8 — Discipline of forgetting

Forgetting is maintenance. Deleting stale context is not a loss of intelligence
but a way to keep the present usable. Stale documents, irrelevant tests, mossy
rules in instructions, forgotten plans, and misleading comments are technical
debt, paid down by deletion.

### Law 9 — Attention budget

Every token of operational context competes with the current task. Long context
is not neutral: it distracts, creates false premises, and makes irrelevant facts
seem significant. Prefer focused selection, bounded instructions, and
read-on-demand over global, always-on memory.

### Law 10 — Clean handoff

Every finished agent session leaves the next agent a cleaner present. The final
state lowers future uncertainty: current documents instead of delta notes,
current tests instead of episode-tests, current rules instead of superseded
rules, current plans instead of draft remnants, current code instead of
transitional scaffolding. The ideal handoff is not "here is what happened" but
"here is what is true now".

### Anti-patterns

- **Delta documentation** — "used to be X, now Y".
- **Monument tests** — tests that record the memory of a bug without naming an
  invariant.
- **Tests for the obvious** — coverage of trivial code already guaranteed by
  types, the language, or the standard library.
- **Instruction sediment** — old rules accumulating in `AGENTS.md` / `CLAUDE.md`
  / `.cursor/rules`.
- **Always-on retrieval** — broad memory fed to the agent "just in case".
- **Ungeneralized lessons** — turning a single incident into a permanent project
  rule.
- **Mossy permission notes** — a lifted ban kept as "now it is allowed".
- **Historical comments in hot code** — comments explaining how it was, not how
  it is.

### Operational model — three layers

1. **Canonical present layer** — current code, tests, documentation,
   instructions.
2. **Working draft** — temporary plans, notes, research logs, local task
   context.
3. **Audit archive** — commit history, tasks, ADRs, incidents, migrations.

The agent works mainly with layer 1. Layer 2 is deleted or normalized when the
work is done. Layer 3 is available on request but is not automatically treated
as instruction.

### Memory-admission checklist

Before writing anything into memory that stays permanently visible to the agent,
answer:

1. Is it true now?
2. Will it still be useful in 30 days?
3. Is it more than a restatement of code already visible?
4. Is it a rule, invariant, or constraint — not an episode?
5. Does it have an owner or an explicit source?
6. Could a future agent act on it without knowing the conversation that produced
   it?
7. If it stops being true, where will that become obvious?

If the answer is weak — do not keep it.

### Deletion protocol

When a rule, feature, or constraint is lifted:

1. Delete the canonical statement.
2. Find paraphrases and stale mentions.
3. Rename tests that encode the old episode.
4. Keep only live invariants.
5. Move necessary history to the audit layer.
6. Make sure the resulting documentation describes only the new present.

The correct result should look boring — as if the stale rule never had
operational authority.

### Applied guidance

- **Documentation** — write in the present tense. Avoid "used to", "now",
  "after the refactor", "we changed X because" — unless the historical reason
  constrains a current decision.
- **Agent instructions (`AGENTS.md`, `CLAUDE.md`)** — small, current,
  normative. Appropriate: stable conventions, current architectural boundaries,
  required commands, active security constraints. Not appropriate: task history,
  "we changed X to Y" notes, lifted bans, bug narratives, one-off preferences.
  These files load directly into the agent's context, so stale text here has
  abnormally high authority.
- **Plans and drafts** — temporary control surfaces. After implementation,
  delete them, turn them into current documents, or archive them as audit.
- **Changelogs and ADRs** — acceptable when the history itself is the product.
  Superseded ADRs are marked explicitly (`Status: superseded`, `Operational
  effect: none`). An ADR without a status is a trap for the agent.

## Steps

The procedure has nested reasoning. Use task planning mode (todo list / task
plan, whichever is available) with one item per step and close them in order.

1. **Parse `args`.** Expect exactly one local file path. Empty, missing, or more
   than one path → stop and ask the user for one local file path, as described
   in Parameters.

2. **Read and classify the document.** Read the passed file. If it does not
   exist or is not readable → stop and report the path. Classify its type, since
   type decides which laws bite hardest:
   - **Instruction file** (`AGENTS.md`, `CLAUDE.md`, `SKILL.md`, `.cursor/rules`)
     — instruction sediment, mossy permission notes, and biography carry
     abnormally high authority here.
   - **Plain document** (`README.md`, `AUDIT.md`, a `./workflow/` artifact, a
     spec or concept page) — delta documentation and biography are the main
     targets.
   - **Code or test file** (a class, a test module) — apply the invariant rules:
     drop tests for the obvious, rename monument tests around live invariants,
     remove historical comments from hot code.

3. **Raise minimal local context.** Read other files only when they are needed
   to judge what is currently true:
   - Read `AGENTS.md` (or `CLAUDE.md`) when the document depends on repository
     canon.
   - Read a neighboring `./workflow/` artifact when the document references or
     depends on it.
   Stop reading once you can judge the document's current truth. Do not over-read
   — extra context competes with the task.

4. **Analyze with `mcp__sequential-thinking__sequentialthinking`.** Walk every
   law in "The Markov Laws" above. For each law decide whether it applies to
   this document and what concretely to change. Produce a normalization plan: the
   specific biography to remove, delta wording to collapse, stale rules to
   delete, conflicting statements to resolve into one canonical form, test names
   to rename around invariants, obvious tests to drop, operational clutter to
   cut. Note anything you keep and the law (Law 4) that justifies keeping it.

5. **Rewrite the document in place.** Apply the plan and overwrite the passed
   file directly — this is the skill's purpose, not a recommendation. The
   rewritten document:
   - describes the current state in the present tense, without creation
     biography or "was X, now Y" deltas;
   - holds one canonical statement per rule, with no contradicting paraphrases;
   - keeps historical information only when it passes the burden-of-retention
     check;
   - for code and tests, names invariants rather than incidents and carries no
     coverage of the obvious;
   - does not mix instruction with audit, or one responsibility type with
     another;
   - preserves the document's original language and its still-valid content.
   The result should look boring — as if the stale material never had authority.

6. **Report to chat.** Write a short chat summary: the file path, its type, and
   the classes of problems fixed (for example: biography removed, deltas
   collapsed, stale rule deleted, conflicting statements resolved, test names
   normalized, obvious test dropped, clutter cut). If nothing needed changing,
   say so plainly. State that the file was rewritten in place so the user can
   review it with their own tools.

## Artifact Requirements

- **The rewritten document** — the file passed in `args`, overwritten in place.
  Same path, same purpose, same language; normalized content.
- **Chat summary** — a short report of the file, its type, and the problem
  classes fixed. No separate report file is created.

## Notes

- The skill normalizes one document per run. To normalize several files, run it
  once per file.
- The skill never edits files other than the one passed, even when a neighboring
  file has the same problem. Surface such observations in the chat summary
  instead.
- For a code or test file, normalize meaning and test framing, not behavior: do
  not change what the code does, only remove clutter, historical comments, and
  episode-shaped test names.
- If the document is already a sufficient present — current, biography-free,
  single-responsibility — leave it unchanged and say so in the summary.
