---
name: audit
description: >
  Audits a project, codebase, documents, or workflow and returns prioritized read-only recommendations in chat. Use for "audit", "review project", "check workflow", or "find risks".
---

# Audit

## Purpose

Audit local project ctx. Return prioritized diagnostic report in chat.

Read only files needed for user scope: project instructions, `./workflow/`,
docs, source, tests, explicit targets. Find contradictions, unclear ownership,
risks, gaps, stale wording, weak next actions.

Read-only by default. Write no files unless user explicitly asks for report file
and gives path. Done = user has clear map of problem, evidence, impact, next
step.

## Parameters

Use `args` as audit scope:

```txt
[audit target or question]
```

If `args` empty, infer from user msg + current project. Broad scope -> inspect
surface first, report only issues with most effect on user goal. Scope arbitrary
without clarification -> ask one concise question before audit.

## Strict Rules

- No git ops: no status, diff, log, branch, commit, push, checkout.
- No edit/create/move/delete/format/regenerate files. Chat report only, except
  explicit report path request.
- No mutating cmds: no deps install/update, cache writes, generated output,
  external service changes, unless user explicitly asks.
- Use read-only local evidence. Prefer `rg`, `rg --files`, `find`, `sed`, `nl`,
  direct reads.
- After ctx gathering, before report, call
  `mcp__sequential-thinking__sequentialthinking` for synthesis: evidence,
  severity, uncertainty, systemic patterns, positives, next actions.
- Separate fact / inference / rec. Mark uncertainty when evidence incomplete.
- Do not invent project pattern from one hint. Trust repeated evidence, canon
  docs, observable behavior.
- Current-state advice only. No biography/delta wording.
- Test recs protect live invariant. No incident/deleted-behavior tests.

## Language Notice

Write user-facing chat output and generated or rewritten artifacts in target
project working language. Detect from `./workflow/`, docs, user msg. If unclear,
use user language.

When editing existing artifact, preserve its language unless user asks
translation.

Apply artifact language to prose, headings, table headers, labels,
placeholders, examples. Keep paths, cmds, tool names, code identifiers,
framework/package names, status markers, established product terms unchanged.

Do not mix languages in one artifact unless canon already does so or source term
requires it.

## Flow

1. Scope.
   - Restate req.
   - Name explicit files/dirs/workflow artifacts/project areas.
   - Broad audit -> use task plan items: ctx, synthesis, report.

2. Gather ctx.
   - Read project instructions first: `AGENTS.md`, `CLAUDE.md`, workflow
     guidance.
   - Read relevant `./workflow/`: `PLAN.md`, `PROJECT.md`, `ARCHITECTURE.md`,
     `DESIGN.md`, `VISION.md`, `ROADMAP.md`, target `features/{slug}/`.
   - Read docs/source/tests needed for scope.
   - Search for duplicated concepts, stale refs, unclear ownership, missing
     docs, overlapping responsibilities, related tests.
   - Broad audit -> keep short ctx map: modules/docs, sources of truth,
     assumptions, deps, trust boundaries.
   - Stop when evidence sufficient.

3. Classify.
   - For each possible issue, record evidence: path, line/section, cmd output,
     observed structure.
   - Mark as fact, inference, question, or out of scope.
   - Drop low-value issues: no effect on decision, workflow, correctness,
     maintainability, docs quality, delivery risk.

4. Synthesize via `mcp__sequential-thinking__sequentialthinking`.
   - Main risks + contradictions.
   - Severity:
     - `P0`: blocks task/release/data safety/security/required invariant.
     - `P1`: high risk of wrong behavior, architecture drift, workflow failure,
       misleading user-facing result.
     - `P2`: weak maintainability, clarity, testability, docs quality,
       repeatability.
     - `P3`: polish, local ambiguity, non-urgent improvement.
   - Prefer systemic causes over symptom lists.
   - Name positive practices worth preserving.
   - Pick smallest useful next steps. Name downstream skill/manual action when
     apt.

5. Report.
   - Lead with scope + key conclusion.
   - Findings by severity.
   - Each finding: fact/evidence, inference, impact, rec.
   - Include systemic patterns when they explain findings.
   - Include positives when useful.
   - End with prioritized next steps + readiness check.

## Report Shape

Use unless user request needs narrower answer. Translate all visible headings,
field labels, table headers, placeholders, examples into output language:

```md
## Audit Scope

- Request:
- Context read:
- Out of scope:

## Key Conclusion

1-3 sentences naming main risk or current project state.

## Findings

### P1: Short Problem Name

- Fact:
- Inference:
- Impact:
- Recommendation:

## Systemic Patterns

- Pattern:
- Where it appears:
- What to normalize:

## What To Preserve

- Practice:
- Why it helps:

## Next Steps

1. Highest-value action.
2. Follow-up action.
3. Readiness check.
```

Every significant finding needs concrete impact. Avoid generic recs like
"improve documentation" unless exact artifact, missing decision, and sufficient
end state are named.

For claim/fix verification, use statuses:

- `verified`: local evidence supports claim/fix.
- `partial`: part supported; important gaps remain.
- `not addressed`: evidence absent or contradicts claim.
- `cannot determine`: ctx insufficient.

## Notes

- `audit` works at any workflow stage. Output can feed `feature`, `planning`,
  `improve`, `docs`, `markov`, or manual edits.
- Report file request without path -> ask for path before writing.
- Conflicting evidence -> name likely source of truth + why.
- No material issues -> say so, name residual uncertainty/test gap.
