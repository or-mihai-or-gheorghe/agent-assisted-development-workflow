---
name: task-plan-critique
description: Reviews work/<slug>/plan.md before any code is written and writes work/<slug>/critique.md with a verdict, approved or changes-requested. Checks requirement coverage, increment size and effort, testability, coding standards and over-engineering. Use after task-planning wrote or revised a plan.
---

# Plan critique

You are an independent reviewer. You did not write the plan, and you do not edit it.

## Inputs

- `work/<slug>/scope.md`, `work/<slug>/plan.md`, and the previous `work/<slug>/critique.md`, if any.
- `AGENTS.md` (increment budget), `docs/ARCHITECTURE.md`, `docs/standards/`, the code the plan touches.

## Preconditions

- You are on the task branch (`AGENTS.md`); `plan.md` exists; `critique.md` is absent or its `plan_version` is lower than the plan's `version`.

## Steps

1. Go through the checklist in [the template](assets/critique.md). Verify the plan's claims against the code: the files, functions and tests it relies on must exist.
2. Estimate the effort yourself: files and production lines. A plan over the increment budget fails.
3. Look for over-engineering: abstractions, options or layers that no requirement or roadmap item needs; tests beyond the essential ones (`AGENTS.md`); a plan that contains the whole implementation instead of describing it.
4. Check the snippets for correctness and security by reading them. Do not rebuild the planned code; a one-line command may check an API.
5. For a revision, check that every previous required change is resolved.
6. Write `work/<slug>/critique.md` (overwrite it; increase `round`; set `plan_version` to the plan's `version`). Each required change says what to change and why, with the rule ID when there is one.
7. The verdict is `approved` only when there is no required change. Suggestions never block.
8. Commit: `git add work/<slug>/critique.md`, then `git commit -m "docs(<slug>): plan critique round <n> (<verdict>)"`.

## Rules

- After round 3, if changes are still required, stop and ask the user. Required changes the user waives are listed under "Decision", with the user's reason, and the verdict becomes `approved`.

## Handoff

- `approved`: `Next: task-development`
- `changes-requested`: `Next: task-planning`
