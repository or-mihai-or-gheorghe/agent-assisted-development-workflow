---
name: task-gitops
description: Publishes the verified task on the current branch. Checks the quality gates recorded in the task files, brings the branch up to date with the base branch, pushes it and opens a pull request whose description is written in work/<slug>/pr.md. Use after task-testing reported pass.
---

# Task GitOps

You are the release engineer.

## Inputs

- Every file in `work/<slug>/`, `AGENTS.md` (Git), `docs/standards/`, the state of the repository.

## Preconditions (stop and report if one fails)

- You are on the task branch (`AGENTS.md`), with a clean working tree.
- `critique.md` approved for the current plan version, `review.md` approved, `test-report.md` pass.
- No code changed after the tested commit: `git diff <tested_commit> HEAD -- . ':!work'` prints nothing. If `pr.md` records an `updated_commit`, compare with that commit instead.

## Steps

1. `git fetch origin`. If `origin/<base>` has commits the branch does not have, update the branch: `git rebase origin/<base>` before the first push, `git merge origin/<base>` after it. On a conflict in code, stop and ask the user. Then run the test command again and record the result and the new `HEAD` as `updated_commit` in `pr.md`.
2. Check that the commit messages follow `AGENTS.md` (Git). Never rewrite commits that were already pushed.
3. Write `work/<slug>/pr.md` from [the template](assets/pr.md): the title, the summary from `scope.md`, the changes from `plan.md` and `dev-log.md`, how to test, the three gates with their rounds, the tech debt and the follow-ups. Commit it: `git add work/<slug>/pr.md`, then `git commit -m "docs(<slug>): pull request description"`.
4. `git push -u origin <slug>`.
5. If `origin` is on GitHub and `gh auth status` succeeds, open the pull request with `pr.md` as its description, without the frontmatter: `sed '1,/^---$/d' work/<slug>/pr.md | gh pr create --base <base> --head <slug> --title "<title>" --body-file -`. Write the pull request's address in the `pr` field of `pr.md`, commit and push. Without GitHub CLI, leave `pr: not created` and tell the user how to open the pull request from the pushed branch.
6. Do not merge. Merging is the user's decision, after task-doc-updates.

## Handoff

`Next: task-doc-updates` (in a new session).
