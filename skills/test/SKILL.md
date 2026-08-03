---
name: test
description: >
  Covers an implemented feature with behavior-focused tests, reconciles
  neighboring tests, records tests.md, runs verification, and marks PLAN.md
  [*] only after a fresh pass. Use when the user says "test feature", "write
  tests", "cover feature", or "verify feature".
---

# Test — Cover an Implemented Feature with Behavior Tests

## Purpose

This skill covers an implemented feature with tests that protect live behavior
invariants. It owns `./workflow/features/{slug}/tests.md`, the project test
code for that feature, and the tested status in `./workflow/PLAN.md`.

It runs after `implement`. It reads the feature artifacts, the implemented
code, and the project test setup, then writes tests that defend current
behavior and reconciles the neighboring tests it touches so the test suite
stays a true statement about the system. It does not plan implementation, does
not change product code, and does not write documentation — those belong to
`planning`, `implement`, and `docs`.

## Parameters

`args`:

```txt
<feature-slug> [scope]
```

- `<feature-slug>` — the feature directory under `./workflow/features/{slug}/`.
- `scope` — optional narrowing: a task number, plan item, module, or test area.

When `<feature-slug>` is missing, infer it from the user's message and the
active entries in `./workflow/PLAN.md`. If several features match, ask one
short question and stop. Without `scope`, test the implemented behavior
broadly enough to cover its relevant invariants.

## Strict Rules

- No git operations of any kind; the user owns git, a dirty tree is expected.
- Do not change product code. When a test fails because the behavior is wrong,
  report the failing invariant and leave the fix to `implement` or the user.
- Every test protects a live behavior invariant. Before writing or keeping a
  test, name the invariant, the risk it guards, and the observable proof; if
  that cannot be stated plainly, the test is not needed.
- Do not test removed behavior, bug history, implementation churn, or
  mechanics already guaranteed by types, the language, the framework, or an
  existing cheaper test.
- Follow the existing test framework, layout, naming, fixtures, and assertion
  style unless no usable pattern exists.
- Choose the cheapest test level that proves the invariant; do not reach for
  E2E when a unit, integration, or component test proves the behavior
  reliably.
- Mark the feature `[*]` in `./workflow/PLAN.md` only after a fresh
  verification command succeeds and its result is recorded in `tests.md`.
  Never set `[*]` for partial, failed, skipped, or written-but-not-run tests.

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

## Steps

This is a multi-step flow; use task planning mode with one item per step.

1. **Resolve the feature and scope.** Parse `args`, confirm
   `./workflow/features/{slug}/feature.md` exists, and read
   `./workflow/PLAN.md` when present to confirm the feature status. If the
   feature cannot be identified, ask for the slug and stop.

2. **Read the context.** Read the feature artifacts that exist — `feature.md`,
   `plan.md`, `design.md`, `NN-task.md` files, `tests.md` — and the project
   canon that affects testing: `./workflow/PROJECT.md` for stack and test
   commands, `./workflow/ARCHITECTURE.md` when module boundaries affect test
   placement, `./workflow/DESIGN.md` when UI behavior is in scope.

3. **Discover the test setup.** Find the framework and verification commands
   from `PROJECT.md`, package scripts, configs, and nearby tests. Inspect
   neighboring tests before writing to capture naming, placement, fixtures,
   mocking style, and data setup. If no test command is documented, infer one
   from the actual stack and record the choice in `tests.md`.

4. **Inspect the implemented behavior.** Read the in-scope code: entry points,
   public interfaces, UI components, routes, state transitions, persistence
   paths. Code is the source of truth for what exists; feature artifacts carry
   intent and risk. If they contradict each other about what should be tested,
   stop and report the contradiction.

5. **Plan the tests.** Decide which behavior invariants are worth protecting,
   why each is not already guaranteed elsewhere, the cheapest sufficient level
   for each (unit, integration, component, E2E), which helpers and fixtures to
   reuse, which dependencies stay real versus mocked, the verification
   commands, and the residual gaps. If open questions remain that the user can
   resolve — ambiguous invariants, a choice of verification command that
   affects the tested status — ask them all in one batch before writing,
   with the recommended option first and marked "(Recommended)".

6. **Write and reconcile tests.** Write the planned tests in the project
   style: one test per invariant, named after current behavior, independent of
   execution order, with controlled time, randomness, network, and shared
   state. Then reconcile the related existing tests in scope: rename or
   rewrite tests whose names or assertions describe incidents, deleted UI, or
   old implementation details; delete in-scope tests that protect no live
   invariant or whose guarantee a clearer test already holds. The suite after
   this step describes only the current system.

7. **Run verification.** Run the smallest useful command first for fast
   feedback, then the readiness command that proves the tested status. Read
   exit codes and output; never claim success by assumption. On failure: stop
   adding tests, identify the failing invariant and observed output, fix
   mistakes in test code only, leave product code and the PLAN.md status
   untouched, and record the failure in `tests.md`.

8. **Write `tests.md`.** Create or update
   `./workflow/features/{slug}/tests.md` as the current test map of the
   feature, per Artifact Requirements. Replace stale entries with the current
   test surface; it is a map, not a run diary.

9. **Closing sweep.** Walk the touched area — `tests.md`, test names, helpers,
   fixtures — and normalize it with the `markov` skill: run it on the affected
   artifacts via the skill mechanism when available, otherwise apply the same
   discipline manually. The result describes the present: no incident-shaped
   names, no biography, no orphaned helpers or fixtures left behind.

10. **Update PLAN.md and report.** If the readiness command succeeded, set
    only this feature to `[*]` in `./workflow/PLAN.md`. Report briefly: the
    feature tested, tests added, updated, renamed, or removed, the
    verification command and result, whether `[*]` was set, and residual gaps.
    End with the next-step recommendation: on a fresh pass, run `docs` to
    document the feature; on failure, return to `implement` with the failing
    invariant.

## Artifact Requirements

`./workflow/features/{slug}/tests.md` is the current map of invariants and
verification for the feature. Core sections:

- **Scope** — the feature behavior covered, in a sentence or two.
- **Invariants** — each protected invariant with its risk, test level, and
  test location.
- **Verification** — the command that proves readiness and its latest result.
- **Gaps** — active uncovered behavior or risk, with the reason; omit or state
  "none known" when the scope is fully covered.

Add a section beyond this core only when it carries a decision. The size of
`tests.md` is proportional to the feature: a couple of sentences plus one
command is a valid complete map for a simple feature. Write in the present
tense — no history, deltas, or bug biography; verification shows the latest
relevant result, not every attempt.

## Updating PLAN.md

Status markers: `[ ]` new, `[-]` planned, `[+]` split into tasks, `[x]`
implemented, `[*]` tested, `[/]` archived. This skill changes only the tested
feature's line, and only to `[*]`, and only after a fresh successful
verification run recorded in `tests.md`.

## Notes

- Coverage percentage is not the goal; protecting meaningful behavior and risk
  is. Add accessibility, security, or performance checks only when the
  feature's risk profile calls for them.
- A regression test is justified when it encodes a product invariant that must
  keep holding; its name and `tests.md` entry describe the normal behavior,
  not the incident that motivated it.
- For dynamic web UI, prefer role, label, visible text, or test-id selectors
  and verify a user action with its visible result; add Playwright only when
  the existing stack cannot prove the behavior cheaper.
