# Agent-assisted development workflow

A development workflow for coding agents. Every change is a small task that goes through eight phases, from scoping to documentation, and each phase is a skill in the open [Agent Skills](https://agentskills.io/specification) format. The phases hand over Markdown files written from templates; fields in those files (verdicts, plan versions, reviewed commits) are the quality gates between phases. The same skills work with Antigravity CLI (`agy`), Claude Code, Codex and other agents that read skills, for software in any language.

## The phases

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

Each phase runs in a new agent session. `task-scoping` chooses the task's slug and creates a branch and a directory `work/<slug>/` with that name. The later phases find the task from the current branch, so they start without arguments: `/task-planning`, `/task-code-review`. When the task is finished, its directory moves to `work/done/<slug>/`. A person confirms the scope and merges the pull request.

## Contents

| Path | Content |
| --- | --- |
| `skills/` | the eight skills, each with its templates in `assets/` |
| `templates/AGENTS.md` | the instructions file a project needs: its commands, its base branch, the workflow, the rules for every phase and the increment budget |
| `examples/task-api/` | an example project to start from: a REST API in Node.js and Express, with its documents, coding standards and roadmap, and no code yet |

## Adding the workflow to a project

The skills contain nothing specific to a project. The project provides:

- `AGENTS.md`, from `templates/AGENTS.md`: what the project is, its install, build, test, lint and start commands, its base branch and, if needed, a different increment budget;
- `docs/standards/`: the coding standards, one file per topic, each rule with an ID (template: `skills/task-doc-updates/assets/project/standard.md`);
- `README.md` and `docs/ARCHITECTURE.md`, `docs/ROADMAP.md`, `docs/TECH_DEBT.md`, `docs/KNOWN_ISSUES.md` (templates in `skills/task-doc-updates/assets/project/`).

Then copy the skills into the project. `agy` and Codex read `AGENTS.md` and `.agents/skills/`; Claude Code reads `CLAUDE.md` and `.claude/skills/`, here a link to the same directory:

```bash
mkdir -p <project>/.agents
cp -R skills <project>/.agents/
cd <project>
echo "@AGENTS.md" >> CLAUDE.md
mkdir -p .claude
ln -s ../.agents/skills .claude/skills
```

Running the `cp` command again updates the skills in place. Create the `.claude/skills` link only once.

## Trying the example

```bash
git clone https://github.com/or-mihai-or-gheorghe/agent-assisted-development-workflow.git
cp -R agent-assisted-development-workflow/examples/task-api ~/task-api
mkdir -p ~/task-api/.agents
cp -R agent-assisted-development-workflow/skills ~/task-api/.agents/
cd ~/task-api
git init -b main
git add .
git commit -m "chore: add project documents, standards and workflow skills"
gh repo create task-api --private --source=. --remote=origin --push
agy
```

The example already contains `CLAUDE.md` and the `.claude/skills` link. In an `agy` session, `/skills` lists the eight skills, and `/task-scoping` starts the first roadmap item.
