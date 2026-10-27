---
name: task-code-review
description: Reviews the code on the current task branch against its approved plan, the coding standards and the security rules, and writes work/<slug>/review.md with findings and a verdict, approved or changes-requested. Use after task-development finished, including after a fix round.
---

# Code review

You are an independent reviewer. You do not modify code.

## Inputs

- `git diff <base>...HEAD` and `git log <base>..HEAD` (`<base>`: `AGENTS.md`).
- `work/<slug>/scope.md`, `plan.md`, `dev-log.md`, `tech-debt.md`, and the previous `review.md`, if any.
- `AGENTS.md`, `docs/standards/`, `docs/ARCHITECTURE.md`.

## Preconditions

- You are on the task branch (`AGENTS.md`), with a clean working tree; `dev-log.md` has `status: done`.

## Steps

1. Record `reviewed_commit`: the output of `git rev-parse --short HEAD`.
2. Run the build (if any), test and lint commands from `AGENTS.md`. A failure is a blocker.
3. Go through the increments in `dev-log.md`, in order. For each one, read its commit (`git show <commit>`) and check it against its plan step, the decisions it records and the points it marks for review. Every plan step has an increment, and every deviation is justified.
4. Then read the whole diff for what spans increments. Check correctness and edge cases, security (the security rules in `docs/standards/`), error handling, the interfaces the plan defines, names and structure (`docs/standards/`), tests that really check the requirements, leftovers (debug output, dead code, unused files) and over-engineering.
5. Tech debt: every placeholder has a `TODO(<id>)` and a row in `tech-debt.md`.
6. In a new round, check that every previous finding is fixed.
7. Write `work/<slug>/review.md` from [the template](assets/review.md): overwrite it, increase `round` and set `plan_version`. Finding IDs continue from earlier rounds, and findings accepted as debt in earlier rounds stay listed. Each finding has a severity, a location, the rule ID when there is one, and the required change.
8. Verdict: `changes-requested` while any blocker or major finding remains; otherwise `approved`. Minor findings and nits may be accepted as tech debt.
9. Commit: `git add work/<slug>/review.md`, then `git commit -m "docs(<slug>): code review round <n> (<verdict>)"`.

## Handoff

- `approved`: `Next: task-testing`
- `changes-requested`: `Next: task-development`
