---
name: task-planning
description: Writes or revises work/<slug>/plan.md, the implementation plan of the task on the current branch. The plan follows the architecture, the coding standards and the current code, gives code snippets for non-trivial or critical parts and lists the tests to write and run. Use after task-scoping confirmed the scope, or when task-plan-critique requested changes.
---

# Task planning

You are the tech lead. In this phase you change no production code.

## Inputs

- `work/<slug>/scope.md`, and `work/<slug>/critique.md` if it exists.
- `AGENTS.md`, `docs/ARCHITECTURE.md`, `docs/standards/`, `docs/TECH_DEBT.md`, `docs/KNOWN_ISSUES.md`.
- The code the task touches and its existing tests.

## Preconditions

- You are on the task branch (`AGENTS.md`), and `scope.md` has `status: confirmed`. Otherwise stop.

## Steps

1. A revision is needed when `critique.md` says `changes-requested`, when the scope's `version` is higher than the plan's `scope_version`, or when the user asks for one. Then address every required change (C1, C2…) and every scope change, increase `version`, set `scope_version` and `answers_critique_round`, and fill in "Changes since the previous version".
2. Read the code paths the requirements touch. Note what exists and what can be reused.
3. Design the smallest change that meets every requirement. Follow `docs/ARCHITECTURE.md`; cite the rule ID from `docs/standards/` where a rule drives a decision.
4. Write `work/<slug>/plan.md` from [the template](assets/plan.md), in under about 200 lines:
   - every file to create or modify, with its purpose;
   - ordered steps that a developer can follow without guessing;
   - code snippets only for the critical parts (security, data access, error handling, non-obvious logic), each at most about 20 lines; the plan describes the code, it does not contain all of it;
   - the essential tests (see the increment budget in `AGENTS.md`), each mapped to a requirement, the existing tests to run again and the commands;
   - a placeholder, with a tech debt entry and the follow-up that removes it, for everything deferred;
   - the size estimate against the increment budget.
5. If the plan exceeds the budget, shrink it: move parts to follow-ups and add placeholders.
6. Commit: `git add work/<slug>/plan.md`, then `git commit -m "docs(<slug>): plan v<version>"`.

## Rules

- Do not implement the plan, not even in a copy outside the repository. Check an API in its documentation or with a one-line command.

## Handoff

`Next: task-plan-critique` (in a new session).
