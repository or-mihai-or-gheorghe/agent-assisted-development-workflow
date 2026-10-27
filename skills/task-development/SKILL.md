---
name: task-development
description: Implements the approved plan of the task on the current branch, step by step, with its tests, and keeps work/<slug>/dev-log.md and work/<slug>/tech-debt.md up to date. Also fixes the findings of a code review or a failed test report. Use only when work/<slug>/critique.md approves the current plan version.
---

# Task development

You are the developer.

## Inputs

- `work/<slug>/plan.md` and `work/<slug>/critique.md`; when present, `review.md`, `test-report.md`, `dev-log.md`, `tech-debt.md`.
- `AGENTS.md`, `docs/standards/`, the code.

## Preconditions (stop if one fails)

- You are on the task branch (`AGENTS.md`), with a clean working tree.
- `critique.md` has `verdict: approved` and a `plan_version` equal to the plan's `version`.

## Steps

1. Find where the task stands:
   - no `dev-log.md`: first run; create `dev-log.md` and `tech-debt.md` from the templates ([dev log](assets/dev-log.md), [tech debt](assets/tech-debt.md)), with the plan's `version` as `plan_version`;
   - `status: in-progress`: continue with the first plan step that is not done;
   - `status: done`, and `review.md` says `changes-requested`, `test-report.md` says `fail` or the user asks for changes: a fix round; increase `round`, set `status: in-progress` and fix only what no earlier fix round in `dev-log.md` has fixed;
   - otherwise there is nothing to do: stop and say so.
2. For each plan step, in order:
   1. implement it, following the plan and `docs/standards/`;
   2. write the tests the plan lists for this step;
   3. run the build (if any), test and lint commands from `AGENTS.md` and fix until they pass;
   4. commit the step's files, with a message in the format of `docs/standards/`;
   5. record the step, the commit and the results in `dev-log.md`.
3. When the plan is wrong or incomplete, make the smallest reasonable deviation and record it: what, why, impact. When a deviation would change the scope or exceed the budget, stop and ask the user.
4. Mark every placeholder or shortcut with `TODO(TD-<slug>-<n>)` and give it a row in `tech-debt.md`.
5. When every step is done and the tests pass, set `status: done` in `dev-log.md`, then `git add work/<slug>/dev-log.md work/<slug>/tech-debt.md` and `git commit -m "docs(<slug>): dev log"`.

## Rules

- Do not edit `scope.md`, `plan.md`, `critique.md`, `review.md` or `test-report.md`.
- Do not push.

## Handoff

`Next: task-code-review` (in a new session).
