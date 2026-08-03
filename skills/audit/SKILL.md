---
name: audit
description: >
  Audits a project, module, mechanic, algorithm, architecture decision, or
  page — read-only — and returns a compact, evidence-bound diagnosis in chat.
  Each finding survives a false-positive gate or is dropped. Use for "audit",
  "review project", "investigate this bug", "how does X work and how to improve
  it", "check architecture", "find risks".
---

# Audit — Skeptical, Evidence-Bound Diagnosis

## Purpose

Audit a scope and return a short diagnosis that a reader can scan in under a
minute. The product is a small set of findings that are real, each tied to
code, each with a concrete consequence. Read-only: write no files unless the
user names a report path.

The bar is not "list everything that could be improved." The bar is "name what
actually matters, prove it, and stop." A clean scope is a valid result — say so
in one line rather than manufacturing findings to look thorough.

## Scope and Intent

Detect the **scope** and adapt the lens:

- **Whole project** — survey the surface first; report only what most affects
  the user's goal. Prefer systemic causes over symptom lists.
- **Module / subsystem** — boundaries, responsibilities, coupling, leaky
  abstractions, dead seams.
- **Mechanic / algorithm** — trace the real control and data flow end to end;
  check correctness, edge cases, complexity, invariants.
- **Architecture decision** — fit to actual constraints, trade-offs paid vs.
  claimed, cheaper alternatives, drift from stated canon.
- **Page / UI flow** — states (loading, empty, error), data contract, a11y,
  interaction edges; not pixel taste unless asked.

Detect the **intent**:

- **Bug investigation** — find the defect: reproduce the path in code, locate
  the root cause, name the trigger and the fix. One proven root cause beats
  five suspects.
- **Understand + improve** — describe how it works *now* tersely, then give
  targeted recommendations with the consequence of each.

When scope or intent stays arbitrary after reading the request and the project
surface, ask one concise question before auditing. Otherwise infer and proceed.

## The False-Positive Gate (core)

Every candidate finding must pass this gate. If it fails, **drop it** — do not
downgrade it to filler.

1. **Restate it in one precise sentence.** If it stops making sense when stated
   plainly, it was pattern-matching. Drop it. (Half of bad findings die here.)
2. **Evidence exists.** Point to `path:line` (or a named section). No concrete
   location → not a finding.
3. **It is real in *this* code**, not assumed. Did you verify the validation /
   call site / type actually behaves as you claim, or did you invent it? Check
   the source before asserting.
4. **Name a concrete scenario where it bites.** Specific input, state, or
   change that produces wrong behavior, data risk, or real maintenance cost.
   No nameable consequence → drop it.
5. **Not style dressed as risk.** Defense-in-depth, preference, or a "smell"
   with no consequence here is not a finding. Say it in one line under a
   "minor / preserve" note at most, or omit.
6. **LLM-bias check.** Am I manufacturing this to appear rigorous? Models
   over-detect problems. If in doubt, cut.

Severity reflects real blast radius, not how easy the issue was to spot or how
clever it sounds.

## Strict Rules (read-only)

- No git operations of any kind; the user owns git, a dirty tree is expected.
- Do not edit, create, move, delete, format, or regenerate files. The chat
  report is the only output, except an explicitly requested report file (ask
  for the path if not given).
- No mutating commands: no installs, updates, cache writes, generated output,
  or external service changes.
- Use read-only local evidence: `rg`, `rg --files`, `find`, direct reads.
  Read → verify each claim against source → drop the unverified → report.
- Separate fact, inference, and recommendation; mark uncertainty explicitly.
- Current-state advice only: no biography, no "was changed from" wording. Test
  recommendations protect live invariants, not past incidents.

## Severity

- **P0** — blocks work, loses data, opens a security hole, or is wrong in
  normal use.
- **P1** — likely wrong in a real scenario, genuine architecture risk, or a
  misleading result.
- **P2** — real correctness-edge, maintainability, or clarity cost with a named
  scenario.
- **P3** — minor; include only when noting it is cheap and genuinely useful.

If a candidate cannot reach P2 with a concrete scenario, it is not a finding.

## Output Format

Lead with **one or two sentences**: the scope and the headline verdict (main
risk, or "no material issues"). Then findings, highest severity first.

Each finding is **at most three lines**, in this shape:

```
**P1 — short title (≤ 8 words)**
`path/file.ext:42` · the fact, one sentence. [inference: ... if not certain]
→ Impact: concrete consequence. Fix: smallest concrete action.
```

For several small same-area findings, use a compact table instead:

```
| Sev | Finding | Where | Fix |
|-----|---------|-------|-----|
| P2  | ... | `file:line` | ... |
```

Close with:

- **Root cause** — one short block *only if* one cause explains several
  findings. Skip it otherwise.
- **Next steps** — 1–5 items, smallest useful first, each naming a concrete
  action and (if apt) a downstream skill (`feature`, `planning`, `improve`,
  `docs`, `markov`) or manual edit. Group by effort only if it helps.

When verifying a claim or a proposed fix, use statuses: `verified`, `partial`,
`not addressed`, `cannot determine`.

## Banned (these are the failure modes to avoid)

- Inflating a non-issue into a paragraph. If the gate didn't pass it, it is not
  here at all.
- Generic advice — "add tests", "improve documentation", "consider
  refactoring" — without naming the exact gap, the missing decision, and the
  sufficient end state.
- Multi-paragraph prose per finding. Heavy, hedging language. Restating the
  obvious.
- Padding the count. Three real findings beat fifteen with twelve fillers.
- Asserting behavior you did not read in the source.

## Language

Write the report in the working language of the target project (detect from
`./workflow/`, docs, and the request; fall back to the user's language). Keep
paths, commands, identifiers, framework and package names, and status markers
in their original spelling. Do not mix languages within the report unless a
quoted term requires it.
