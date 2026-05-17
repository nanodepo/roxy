---
name: test
description: >
  Covers implemented features with behavior-focused tests, records tests.md,
  runs verification, and marks PLAN.md [*] only after fresh pass. Triggers:
  test feature, write tests, cover feature, verify feature.
---

# Test

## Purpose

Cover implemented feature with meaningful checks. Owns feature
`./workflow/features/{slug}/tests.md`, project test code for that feature, and
tested status in `./workflow/PLAN.md`.

Runs after `implement`. Reads feature artifacts, implemented code, project test
setup. Writes tests protecting current behavior invariants. Does not plan impl,
change product code, write docs, or perform git ops.

## Params

Use `args`:

```txt
<feature-slug> [scope]
```

- `<feature-slug>`: feature dir under `./workflow/features/{slug}/`.
- `scope`: optional task number, plan item, component, module, or test area.

Resolve missing:

- No `<feature-slug>`: infer from user msg + active entries in
  `./workflow/PLAN.md`.
- Multiple matches: ask one short question, stop.
- No `scope`: test implemented behavior broadly enough for relevant
  invariants.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout, worktree.
  Dirty tree expected; user owns git.
- Do not change product code. If test fails because behavior wrong, report
  failing invariant; leave fix to `implement` or user unless scope changes.
- `mcp__sequential-thinking__sequentialthinking` required at Step 5. Choosing
  invariants, risks, levels, cmds, residual gaps = analytical work.
- Test live behavior invariants, not incidents, removed behavior, or obvious
  language mechanics.
- Use existing test framework, layout, naming, fixtures, helpers, assertion
  style unless no usable pattern exists.
- Choose cheapest level proving invariant. No E2E when unit/integration/component
  proves behavior reliably.
- Set feature `[*]` in `./workflow/PLAN.md` only after fresh verification cmd
  succeeds and result recorded in `tests.md`.
- If tests not run or verification fails, do not set `[*]`.

## Language Notice

Write this `SKILL.md` in English.

Write chat output and generated/rewritten project artifacts in target project
working language. Detect from `./workflow/`, docs, user request. If unclear,
use user language.

When editing existing artifact, preserve language unless user asks translate.

Apply artifact language to prose, headings, table headers, labels,
placeholders, examples. Keep paths, commands, tools, code ids, frameworks,
packages, status markers, product terms as-is.

Do not mix languages in one artifact unless project canon already does or quote
/ source term requires it.

## Steps

Use task planning mode for this multi-step flow + nested analytical call. Close
items one by one.

### 1. Identify feature + scope

Parse `args`; identify `./workflow/features/{slug}/`.

Read `./workflow/PLAN.md` if exists to confirm feature status and later update
entry. Confirm `./workflow/features/{slug}/feature.md` exists. If feature
unknown, ask slug and stop.

### 2. Read feature + project ctx

Read feature ctx:

- `./workflow/features/{slug}/feature.md`: required.
- `./workflow/features/{slug}/plan.md`: if exists.
- `./workflow/features/{slug}/design.md`: if exists.
- `./workflow/features/{slug}/NN-task.md` files: if exist.
- `./workflow/features/{slug}/tests.md`: if exists.

Read project ctx:

- `./workflow/PROJECT.md`: stack, run cmds, test cmds, service setup.
- `./workflow/ARCHITECTURE.md`: when module boundaries affect test placement.
- `./workflow/DESIGN.md`: when UI behavior / visual interaction in scope.

### 3. Discover test setup

Find existing framework + cmds from `PROJECT.md`, package scripts, configs,
lock files, test dirs, nearby tests.

Inspect nearby tests before writing. Capture:

- naming conventions;
- file placement;
- fixture/helper patterns;
- mocking style;
- test data setup/cleanup;
- targeted + readiness verification cmds.

If no test cmd documented, infer likely cmd from actual stack and record choice
in `tests.md`. If multiple plausible cmds and wrong choice could mislead
status, ask user which cmd proves readiness.

### 4. Inspect implemented behavior

Read in-scope code: entry points, adapters, public interfaces, UI components,
routes, state transitions, persistence paths.

Code = source of truth for existence. Feature artifacts = intended behavior +
risks. If artifacts and code contradict what should be tested, stop and report
contradiction.

### 5. Plan tests with sequential thinking

Call `mcp__sequential-thinking__sequentialthinking`. Decide:

- behavior invariants worth testing;
- risk each invariant protects;
- cheapest sufficient level:
  - unit: pure logic, validation, transforms, branching;
  - integration: module interaction, API, DB, adapters;
  - component/UI: component state, events, accessibility of key actions;
  - e2e: critical user paths / cross-system behavior not cheaper to prove;
- existing helpers/fixtures to reuse;
- deps kept real vs mocked because external, costly, unstable, unavailable;
- targeted verification cmds + readiness cmd;
- residual gaps to show in `tests.md`.

Fix result as short test plan before edits.

### 6. Write/update tests

Write tests in project style. One test focuses on one invariant.

Quality rules:

- Name tests by current behavior, not bug IDs, removed UI, impl details.
- Use arrange-act-assert unless local framework implies other convention.
- Tests independent of execution order.
- Setup/cleanup state inside test, fixture, or accepted helper.
- Control time, randomness, network, shared state, external services.
- Use parameterization for equivalent boundaries.
- Avoid assertions proving language/framework/trivial getter only.
- Do not assert internals when visible behavior/public contract proves
  invariant.

Dynamic web UI: start/reuse local server, wait for stable render, inspect DOM or
screenshot before selectors, prefer role/label/visible text/test id, verify user
action + visible result. Add Playwright only when existing stack cannot prove
behavior cheaper.

### 7. Run verification

Run smallest useful cmd first for feedback, then readiness cmd proving tested
status.

For every cmd, read exit code + output. Do not claim success by assumption. If
cmd scoped intentionally, record scope + reason in `tests.md`.

If verification fails:

- stop adding tests;
- identify failing invariant, file, cmd, observed output;
- fix test-code mistakes within scope;
- do not modify product code;
- leave `./workflow/PLAN.md` unchanged unless it already has accurate
  non-tested status;
- record failure/residual gap in `tests.md` and report.

### 8. Write `tests.md`

Create/update `./workflow/features/{slug}/tests.md` as current feature test map.

Use semantic structure. Translate visible headings, field labels, table headers,
placeholders, examples:

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

Write present tense. No history, deltas, release notes, bug biography, temp
logs. If no meaningful gaps: `- None known from the current scope.`

### 9. Update PLAN.md

If readiness cmd succeeded, update only this feature service status in
`./workflow/PLAN.md` to `[*]`.

Do not change unrelated entries. Do not set `[*]` for partial, failed, skipped,
or only-written-not-run verification.

Status markers: `[ ]` new, `[-]` planned, `[+]` split into tasks, `[x]`
implemented, `[*]` tested, `[/]` archived.

### 10. Report

Report briefly:

- feature tested;
- tests added/updated;
- verification cmd + result;
- whether `./workflow/PLAN.md` updated to `[*]`;
- residual gaps/blockers, if any.

## Artifact Requirements

Produces:

- `./workflow/features/{slug}/tests.md`: current feature test map around
  behavior invariants.
- Project test code: tests in existing framework + local style.
- Updated `./workflow/PLAN.md`: feature `[*]` only after fresh successful
  verification cmd.

## Notes

- Coverage tools help find blind spots; percent is not goal. Goal = protect
  meaningful behavior + risk.
- Add accessibility/security/performance/compat/cross-browser checks only when
  feature behavior or risk profile calls for them.
- Regression tests encode product invariant that should keep holding. Test name
  and `tests.md` describe normal behavior, not incident that motivated check.
