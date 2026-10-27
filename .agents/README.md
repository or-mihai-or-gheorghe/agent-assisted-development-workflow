# Agent-assisted development workflow

Eight skills, in the open [Agent Skills](https://agentskills.io/specification) format, that take a change from scoping to documentation. Each phase runs in a new agent session and hands over Markdown files written from templates; fields in those files (verdicts, plan versions, reviewed commits) are the quality gates between phases.

| # | Skill | Role | Writes |
| --- | --- | --- | --- |
| 1 | `task-scoping` | analyst: clarifies the request with the user | `scope.md` |
| 2 | `task-planning` | tech lead: designs the change | `plan.md` |
| 3 | `task-plan-critique` | independent reviewer of the plan | `critique.md` |
| 4 | `task-development` | developer: implements the approved plan, with tests | code, tests, `dev-log.md`, `tech-debt.md` |
| 5 | `task-code-review` | independent reviewer of the code | `review.md` |
| 6 | `task-testing` | tester: checks the running software | `test-report.md` |
| 7 | `task-gitops` | publishes the branch and opens the pull request | `pr.md` |
| 8 | `task-doc-updates` | updates the project documents and closes the task | `docs-update.md` |

`task-scoping` chooses the task's slug and creates a branch and a directory `work/<slug>/` with that name. The later phases find the task from the current branch, so they start without arguments. When the task is finished, its directory moves to `work/done/<slug>/`. A person confirms the scope and merges the pull request.

## Using the skills in another project

The skills contain nothing specific to a project. The project provides:

- `AGENTS.md`, from the template in `skills/task-doc-updates/assets/project/AGENTS.md`: what the project is, its install, build, test, lint and start commands, its base branch and, if needed, a different increment budget;
- `docs/standards/`: the coding standards, one file per topic, each rule with an ID (template: `skills/task-doc-updates/assets/project/standard.md`);
- `README.md` and `docs/ARCHITECTURE.md`, `docs/ROADMAP.md`, `docs/TECH_DEBT.md`, `docs/KNOWN_ISSUES.md` (templates in `skills/task-doc-updates/assets/project/`).

Copy this `.agents/` directory into the project; running the copy again updates the skills. Agents look for skills in these directories:

| Agent | Skills directory |
| --- | --- |
| `agy` (Antigravity CLI) | `.agents/skills/` |
| Codex | `.agents/skills/` |
| Claude Code | `.claude/skills/` |

For an agent that looks in another directory, a symbolic link to `.agents/skills` avoids a second copy, for example `ln -s ../.agents/skills .claude/skills` in the project.
