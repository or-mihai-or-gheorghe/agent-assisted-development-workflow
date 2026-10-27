---
name: task-testing
description: Verifies the reviewed task on the current branch. Runs the whole test suite and the tests listed in the plan, checks every acceptance criterion of the scope, uses the running software in exploratory checks and writes work/<slug>/test-report.md with a pass or fail verdict. Use after task-code-review approved the code.
---

# Task testing

You are the tester. You report problems; you do not fix code.

## Inputs

- `work/<slug>/scope.md` (acceptance criteria), `plan.md` (tests, interfaces), `review.md`, `dev-log.md` (notes for testing), the previous `test-report.md`, if any.
- `AGENTS.md` (commands), `README.md`, the testing rules in `docs/standards/`.

## Preconditions

- `review.md` has `verdict: approved`.
- No code changed after the reviewed commit: `git diff <reviewed_commit> HEAD -- . ':!work'` prints nothing.

## Steps

1. Record `tested_commit` (`git rev-parse --short HEAD`) and the environment: runtime and version, operating system, storage or external services.
2. Clean install and the whole suite: the install, build (if any) and test commands from `AGENTS.md`. Record the counts.
3. For every test the plan lists, check that it exists and passed.
4. For every acceptance criterion, record how it was verified (test ID or exploratory check ID) and the result.
5. Exploratory checks, with temporary data. Use the software the way its users do, as the plan's interfaces, the dev log's notes and `README.md` describe: requests to an API, commands of a CLI, actions in a UI (with a browser tool, if you have one), calls from a short script for a library. Start servers in the background. Cover the success path, invalid input, missing or wrong credentials where they apply, and the edge cases in `scope.md`. Also run the manual checks kept in the repository. Record each check: action, expected and actual result.
6. Stop what you started and delete the temporary data.
7. Write `work/<slug>/test-report.md` from [the template](assets/test-report.md): overwrite it, increase `round` and keep the known issues recorded in earlier rounds. Problems outside this task's scope go under "Known issues to record".
8. Verdict: `pass` only if every test passes and every acceptance criterion is met. A criterion you cannot check is `not verified`, never `pass`; ask the user to check it before you decide.
9. Commit: `git add work/<slug>/test-report.md`, then `git commit -m "docs(<slug>): test report round <n> (<verdict>)"`.

## Handoff

- `pass`: `Next: task-gitops`
- `fail`: `Next: task-development`
