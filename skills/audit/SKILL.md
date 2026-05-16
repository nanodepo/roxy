---
name: audit
description: >
  Audits a project, codebase, documents, or workflow and returns prioritized read-only recommendations in chat. Use for "audit", "review project", "check workflow", "find risks".
---

# Audit

## Purpose

Audit local project context and return a structured diagnostic report in chat. Read the codebase, `./workflow/`, project documentation, and any files the user explicitly names. Find contradictions, unclear ownership, risks, gaps, stale wording, and weak next actions.

Keep audit separate from execution. Do not change code, documentation, workflow artifacts, configuration, or generated files during the default chat-report workflow. The audit is complete when the user has a clear map of problems, evidence, impact, and next steps.

## Parameters

Use `args` as the audit scope when present:

```txt
[audit target or question]
```

If `args` is empty, infer the scope from the user's message and the current project. If the scope is broad, inspect the project surface first and narrow the report to the issues that most affect the user's goal. If the scope is ambiguous enough that evidence selection would be arbitrary, ask one concise clarification question before auditing.

## Strict Rules

- Do not perform git operations in any form: no status checks, diffs, logs, branches, commits, pushes, or checkout commands.
- Do not edit, create, move, delete, format, or regenerate files during the default audit. Return the audit in chat only unless the user explicitly asks for a standalone report file.
- Do not run commands that are likely to mutate the worktree, dependency state, caches, generated output, or external services unless the user explicitly asks for that execution as part of the audit.
- Use read-only local evidence. Prefer `rg`, `rg --files`, `find`, `sed`, `nl`, and direct file reads for context.
- Call `mcp__sequential-thinking__sequentialthinking` after context gathering and before writing the final report. Use it to synthesize evidence, severity, uncertainty, systemic patterns, positive findings, and next actions.
- Separate facts from inferences and recommendations. Mark uncertainty explicitly when evidence is incomplete.
- Do not invent project patterns from a single hint. Treat repeated local evidence, canonical project files, and observable behavior as stronger than guesses.
- Do not preserve biography or delta wording as a recommendation. Normalize advice around the current desired state.
- When recommending tests, describe the live invariant the test should protect. Do not recommend tests that only memorialize an incident or deleted behavior.

## Steps

1. Define the audit scope.
   - Restate what the user asked to check.
   - Identify explicit files, directories, workflow artifacts, or project areas named by the user.
   - For broad audits, use task planning mode (todo list / task plan, whichever is available) to track context gathering, synthesis, and reporting.

2. Gather local context.
   - Read relevant project instructions first when present, such as `AGENTS.md`, `CLAUDE.md`, or local workflow guidance.
   - Read relevant files under `./workflow/`, especially `./workflow/PLAN.md`, `./workflow/PROJECT.md`, `./workflow/ARCHITECTURE.md`, `./workflow/DESIGN.md`, `./workflow/VISION.md`, `./workflow/ROADMAP.md`, and the target `./workflow/features/{slug}/` files when they relate to the request.
   - Read project documentation and source files needed to verify the user's scope.
   - Use search to locate duplicated concepts, stale references, unclear ownership boundaries, missing docs, overlapping responsibilities, and related tests.
   - Keep a short context map for broad audits: main modules or documents, apparent sources of truth, important assumptions, dependencies between artifacts, and boundaries of trust.

3. Classify evidence.
   - For each possible issue, record the local evidence: file path, line or section when available, command output when relevant, or observed project structure.
   - Decide whether the evidence is a fact, an inference, a question, or out of scope.
   - Drop findings that do not affect the user's decision, current workflow, correctness, maintainability, documentation quality, or delivery risk.

4. Synthesize with `mcp__sequential-thinking__sequentialthinking`.
   - Identify the main risks and contradictions.
   - Assign severity by impact:
     - `P0` blocks the task, release, data safety, security, or a required project invariant.
     - `P1` creates substantial risk of incorrect behavior, architectural drift, workflow failure, or misleading user-facing results.
     - `P2` weakens maintainability, clarity, testability, documentation quality, or repeatability.
     - `P3` is polish, local ambiguity, or an improvement without urgent risk.
   - Prefer systemic causes over long lists of small symptoms.
   - Identify positive practices worth preserving.
   - Decide the smallest useful next steps and which downstream skill or manual action fits each step.

5. Write the chat report.
   - Lead with the audit scope and key conclusion.
   - List findings ordered by severity.
   - For each finding, include fact/evidence, inference, impact, and recommendation.
   - Include systemic patterns when they explain multiple findings.
   - Include positive findings when they matter for future edits.
   - End with prioritized next steps and a readiness check.

## Report Requirements

Use this report shape unless the user's request calls for a narrower answer:

```md
## Audit Scope

- Request:
- Context read:
- Out of scope:

## Key Conclusion

1-3 sentences naming the main risk or current project state.

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

Every significant finding must have a concrete impact. Avoid generic recommendations such as "improve documentation" unless you name the exact artifact, missing decision, and sufficient end state.

When the user asks to verify an existing claim or fix, use explicit statuses:

- `verified` means local evidence supports the claim or fix.
- `partial` means part of the claim or fix is supported, but important gaps remain.
- `not addressed` means the relevant evidence is absent or contradicts the claim.
- `cannot determine` means the available context is insufficient.

## Notes

- `audit` can run at any stage of the workflow. Its output can feed `feature`, `planning`, `improve`, `docs`, `markov`, or manual edits, but `audit` itself remains read-only.
- If the user asks for a report file, explain that the default artifact is a chat report and ask for an explicit path before writing only that report.
- If evidence conflicts, name the likely source of truth and explain why. Do not merge contradictory rules into a compromise.
- If the audit finds no material issues, say so directly and mention any residual uncertainty or test gap.
