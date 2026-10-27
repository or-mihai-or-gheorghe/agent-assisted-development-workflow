---
name: task-doc-updates
description: Closes the task on the current branch. Updates the project documents (README, architecture, roadmap, tech debt, known issues) from the task files, writes work/<slug>/docs-update.md, moves the task to work/done/<slug>/ and pushes to the open pull request. Use after task-gitops pushed the branch.
---

# Documentation updates

You are the technical writer. You change documentation only.

## Inputs

- Every file in `work/<slug>/`, and `git diff origin/<base>...HEAD --stat` (`<base>`: `AGENTS.md`).
- `README.md`, `docs/ARCHITECTURE.md`, `docs/ROADMAP.md`, `docs/TECH_DEBT.md`, `docs/KNOWN_ISSUES.md`.

## Preconditions

- You are on the task branch (`AGENTS.md`), already pushed, with `pr.md` committed and a clean working tree.

## Steps

1. `README.md`: what users and developers need now (features, interfaces, configuration, how to install, run and test). Remove statements that are no longer true.
2. `docs/ARCHITECTURE.md`: new components, data model changes and decisions, one line each, with the reason and the task slug.
3. `docs/ROADMAP.md`: mark the item done, with the pull request (an item with `roadmap_item: new` is added as done); add the follow-ups named in `scope.md` and in the placeholders of `plan.md`.
4. `docs/TECH_DEBT.md`: add the open items from `work/<slug>/tech-debt.md` and the findings that `review.md` accepted as debt (ID `TD-<slug>-<finding ID>`, for example `TD-user-login-F2`); mark the items this task resolved.
5. `docs/KNOWN_ISSUES.md`: add the "Known issues to record" from `test-report.md`, with IDs `KI-<slug>-<n>`.
6. `docs/standards/` stays unchanged. If the task revealed a missing convention, propose it to the user.
7. A project document that does not exist yet starts from its template in [assets/project/](assets/project/).
8. Write `work/<slug>/docs-update.md` from [the template](assets/docs-update.md).
9. Move the finished task: `mkdir -p work/done`, then `git mv work/<slug> work/done/<slug>`.
10. `git add README.md docs work`, then `git commit -m "docs(<slug>): update project documents and close the task"` and `git push`.

## Handoff

Tell the user that the pull request is ready to merge, and that after the merge come `git switch <base>` and `git pull`. Then: `Next: task-scoping` (for the next task).
