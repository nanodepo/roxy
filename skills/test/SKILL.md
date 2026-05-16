---
name: test
description: >
  Covers implemented features with behavior-focused tests, records tests.md,
  runs verification, and marks PLAN.md [*] only after a fresh pass. Use for
  "test feature", "write tests", "cover feature", or "verify feature".
---

# Test

## Purpose

`test` covers an implemented feature with meaningful checks. It owns the
feature's `./workflow/features/{slug}/tests.md`, the project test code written
for that feature, and the tested status in `./workflow/PLAN.md`.

It runs after `implement`. It reads feature artifacts, the implemented code, and
the project test setup, then writes tests that protect current behavior
invariants. It does not plan implementation, does not change product code, does
not write documentation, and performs no git operations.

## Parameters

Use `args` to name the feature and, optionally, a narrower scope:

```txt
<feature-slug> [scope]
```

- `<feature-slug>` - the feature directory under
  `./workflow/features/{slug}/`.
- `scope` - optional: a task number, plan item, component, module, or test area
  to focus on.

Resolve missing parameters:

- If `<feature-slug>` is absent, infer it from the user message and active
  feature entries in `./workflow/PLAN.md`.
- If multiple features match, ask one short question and stop until the user
  names the feature.
- If `scope` is absent, test the implemented feature behavior broadly enough to
  cover the relevant invariants.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs,
  branches, commits, pushes, checkout, or worktree commands. The working tree is
  expected to be dirty; the user manages git.
- Do not change product code. If a test fails because the implemented behavior
  is wrong, report the failing invariant and leave the fix to `implement` or the
  user unless the user explicitly changes scope.
- **`mcp__sequential-thinking__sequentialthinking` is required** at the test
  planning step (Step 5). Choosing invariants, risks, test levels, commands, and
  residual gaps is analytical work.
- Write tests for live behavior invariants, not for incidents, removed behavior,
  or obvious language mechanics.
- Use the existing project test framework, file layout, naming style, fixtures,
  helpers, and assertion style unless the project has no usable test pattern.
- Choose the cheapest test level that proves the invariant. Do not use E2E when
  a unit, integration, or component test proves the same behavior reliably.
- Set the feature status to `[*]` in `./workflow/PLAN.md` only after a fresh
  verification command succeeds and its result is recorded in `tests.md`.
- If tests are not run, or the verification command fails, do not set status
  `[*]`.
- Write this skill's text in English. Keep project prose, code, and artifacts
  in the project's working language; keep paths, tool names, commands, and code
  identifiers in their original spelling.

## Steps

For this multi-step procedure with a nested analytical call, use the agent's
task planning mode (todo list / task plan, whichever is available) and close
items one by one.

### 1. Identify the feature and scope

Parse `args` and identify `./workflow/features/{slug}/`.

Read `./workflow/PLAN.md` if it exists to confirm the feature status and find
the entry that must be updated later. Then confirm
`./workflow/features/{slug}/feature.md` exists. If the feature cannot be
identified, ask the user for the slug and stop.

### 2. Read feature and project context

Read the feature context:

- `./workflow/features/{slug}/feature.md` - required.
- `./workflow/features/{slug}/plan.md` - if it exists.
- `./workflow/features/{slug}/design.md` - if it exists.
- `./workflow/features/{slug}/NN-task.md` files - if they exist.
- `./workflow/features/{slug}/tests.md` - if it exists.

Read project context:

- `./workflow/PROJECT.md` - for stack, run commands, test commands, and service
  setup.
- `./workflow/ARCHITECTURE.md` - when module boundaries affect test placement.
- `./workflow/DESIGN.md` - when UI behavior or visual interaction is in scope.

### 3. Discover the test setup

Find the project's existing test framework and commands from `PROJECT.md`,
package scripts, config files, lock files, test directories, and neighboring
tests.

Inspect nearby tests before writing new ones. Capture:

- naming conventions;
- file placement;
- fixture and helper patterns;
- mocking style;
- test data setup and cleanup;
- command used for targeted and readiness verification.

If no test command is documented, infer the most likely command from the actual
stack and record the chosen command in `tests.md`. If multiple commands are
equally plausible and choosing one could create misleading status, ask the user
which command proves readiness.

### 4. Inspect the implemented behavior

Read the implemented code in scope, including entry points, adapters, public
interfaces, UI components, routes, state transitions, and persistence paths that
the feature affects.

Use the code as the source of truth for what exists. Use feature artifacts to
understand intended behavior and risks. If the artifacts and code contradict
each other in a way that changes what should be tested, stop and report the
contradiction.

### 5. Plan tests with sequential thinking

Call `mcp__sequential-thinking__sequentialthinking` to decide:

- the behavior invariants that deserve tests;
- the risk each invariant protects;
- the cheapest sufficient test level for each invariant:
  - unit - pure logic, validation, transformations, branching;
  - integration - module interaction, API, database, adapters;
  - component/UI - component state, events, accessibility of important actions;
  - e2e - critical user paths or cross-system behavior that cannot be proven
    more cheaply;
- existing test helpers and fixtures to reuse;
- dependencies to keep real and dependencies to mock because they are external,
  expensive, unstable, or unavailable;
- targeted verification commands and the readiness command;
- residual gaps that should remain visible in `tests.md`.

Fix the result as a short test plan before editing files.

### 6. Write or update tests

Write tests in the project style and keep each test focused on one invariant.

Quality rules:

- Name tests by current behavior, not by bug IDs, removed UI, or implementation
  details.
- Use clear arrange-act-assert structure when the local framework does not imply
  a different convention.
- Keep tests independent of execution order.
- Set up and clean up state inside the test, fixture, or accepted project
  helper.
- Control time, randomness, network, shared state, and external services.
- Use parameterization for equivalent boundary cases.
- Avoid assertions that only prove the language, framework, or a trivial getter.
- Do not assert internal implementation details when visible behavior or public
  contracts prove the invariant.

For dynamic web UI, start or reuse the local server, wait for a stable rendered
state, inspect the rendered DOM or screenshot before choosing selectors, prefer
stable selectors such as role, label, visible text, or test id, and verify the
user action plus visible result. Add Playwright only when the existing stack
does not prove the behavior more cheaply.

### 7. Run verification

Run the smallest useful command first when it gives faster feedback, then run
the readiness command that proves the feature's tested status.

For every command, read the exit code and output. Do not claim success from an
assumption. If a command is intentionally scoped, record the scope and reason in
`tests.md`.

If verification fails:

- stop adding new tests;
- identify the failing invariant, file, command, and observed output;
- fix test-code mistakes within this skill's scope;
- do not modify product code;
- leave `./workflow/PLAN.md` unchanged unless it already contains an accurate
  non-tested status;
- record the failure or residual gap in `tests.md` and report it to the user.

### 8. Write tests.md

Create or update `./workflow/features/{slug}/tests.md` as the current test map
for the feature.

Use this structure:

```md
# Tests

## Scope

Briefly name the feature behavior covered.

## Invariants

| Invariant | Risk | Test level | Test file or command | Result |
| --- | --- | --- | --- | --- |

## Verification

- Command: `<command>`
- Result: `<passed / failed / not run>`
- Notes: `<short factual note, if needed>`

## Gaps

- `<uncovered behavior or risk, with reason>`
```

Write `tests.md` in the present tense. Do not include history, deltas, release
notes, bug biography, or temporary work logs. If there are no meaningful gaps,
write `- None known from the current scope.`

### 9. Update PLAN.md

If the readiness command succeeded, update only this feature's service status in
`./workflow/PLAN.md` to `[*]`.

Do not change unrelated entries. Do not set `[*]` for partial verification,
failed verification, skipped verification, or tests that were only written but
not run.

The status markers are: `[ ]` new, `[-]` planned, `[+]` split into tasks, `[x]`
implemented, `[*]` tested, `[/]` archived.

### 10. Report

Give a short report with:

- the feature tested;
- tests added or updated;
- verification command and result;
- whether `./workflow/PLAN.md` was updated to `[*]`;
- residual gaps or blockers, if any.

## Artifact Requirements

This skill produces:

- **`./workflow/features/{slug}/tests.md`** - the feature's current test map,
  written around behavior invariants.
- **Project test code** - tests in the existing test framework and local style.
- **Updated `./workflow/PLAN.md`** - the feature status set to `[*]` only after a
  fresh successful verification command.

## Updating PLAN.md

At the end, touch `./workflow/PLAN.md` only to set the tested feature's status
to `[*]` after successful verification. Do not update other feature statuses and
do not rewrite unrelated tails.

If verification does not pass, leave the status as it is and report what blocks
the tested status.

## Notes

- Coverage tools are useful for finding blind spots, but coverage percentages
  are not the goal. The goal is protecting meaningful behavior and risk.
- Add accessibility, security, performance, compatibility, or cross-browser
  checks only when the feature's behavior or risk profile calls for them.
- Regression tests must encode the product invariant that should keep holding.
  The test name and `tests.md` entry describe normal behavior, not the incident
  that motivated the check.
