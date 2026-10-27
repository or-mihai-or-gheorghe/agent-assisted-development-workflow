# AGENTS.md

Instructions for the coding agent working in this repository, for example Antigravity CLI (`agy`).

## Project

Task API: a REST backend in Node.js and Express 5 with user registration, JWT authentication, authorization and CRUD over tasks. Data lives in a local JSON file first and in Cloud Firestore later. Read `README.md` and `docs/ARCHITECTURE.md` first.

In the API, a task is a user's to-do (`/tasks`); in the workflow below, a task is a change to this repository.

All code, comments, documents and commit messages are in English.

## Commands

The workflow skills use these commands by name. Run install first in a new checkout and whenever the dependencies change. The first roadmap item creates `package.json` with these scripts.

| Name | Command | Notes |
| --- | --- | --- |
| install | `npm ci` | installs the versions in `package-lock.json`; `npm install <package>@<version>` adds a dependency and creates the lock file if it is missing |
| build | none | plain JavaScript, no build step |
| test | `npm test` | runs all tests (`node:test`) and exits |
| lint | `npm run lint` | ESLint; must report nothing |
| start | `npm start` | starts the API; the port comes from `PORT` |
| dev | `npm run dev` | like start, with `.env` loaded and a restart on every change |

Configuration comes from environment variables, in development from `.env` (`node --env-file=.env`). Never commit `.env`, keys or `node_modules`.

## Git

- Base branch: `main`. The skills call it `<base>`.
- Code commits follow `docs/standards/`. Task files are committed as `docs(<slug>): …`.

## Project documents

| Document | Content | Written by |
| --- | --- | --- |
| `README.md` | what the software does, how to install, run and test it | task-doc-updates |
| `docs/ARCHITECTURE.md` | components, data model, decisions | task-doc-updates |
| `docs/ROADMAP.md` | planned and finished increments | task-doc-updates |
| `docs/TECH_DEBT.md` | open shortcuts and placeholders | task-doc-updates |
| `docs/KNOWN_ISSUES.md` | known bugs and limitations | task-doc-updates |
| `docs/standards/*.md` | coding standards, with rule IDs | humans only |

## Workflow

Every change is a task. task-scoping chooses its slug (kebab-case, for example `user-registration`) and creates a branch with the same name and the directory `work/<slug>/`. When the task is finished, task-doc-updates moves the directory to `work/done/<slug>/`.

A task goes through eight phases. Each phase is a skill in `.agents/skills/` and runs in a new agent session.

| # | Skill | Reads | Writes | Continue when |
| --- | --- | --- | --- | --- |
| 1 | task-scoping | the user, project documents, the web | `scope.md` | the user confirms the scope |
| 2 | task-planning | `scope.md`, `critique.md`, documents, code | `plan.md` | always |
| 3 | task-plan-critique | `plan.md`, `scope.md`, documents, code | `critique.md` | verdict `approved` |
| 4 | task-development | approved `plan.md`, findings | code, tests, `dev-log.md`, `tech-debt.md` | tests pass |
| 5 | task-code-review | diff, `plan.md`, `dev-log.md`, `tech-debt.md` | `review.md` | verdict `approved` |
| 6 | task-testing | `scope.md`, `plan.md`, the running software | `test-report.md` | verdict `pass` |
| 7 | task-gitops | all task files, git | `pr.md`, pushed branch, pull request | branch pushed |
| 8 | task-doc-updates | all task files, project documents | project documents, `docs-update.md`; moves the task to `work/done/` | the user merges |

Loops: critique `changes-requested` goes back to task-planning; review `changes-requested` or test `fail` goes back to task-development, then to task-code-review again.

## Rules for every phase

1. One phase per session. Read this file, then the skill, then only the inputs the skill lists.
2. After task-scoping, every phase works on the task of the current branch: the slug is the branch name and the task files are in `work/<slug>/`. On `<base>`, or when that directory does not exist, stop and ask the user to switch to the task's branch. If only `work/done/<slug>/` exists, the task is closed: to change it, move the directory back (`git mv work/done/<slug> work/<slug>`), commit, and run the phase the change needs, usually task-development.
3. Write task files from the skill's template, in `assets/`. Keep the frontmatter; never edit a file that belongs to another phase.
4. Check the preconditions first. If one fails, stop and say what is missing.
5. When information is missing, ask the user; do not invent requirements.
6. Commit your phase's output on the task branch, adding each file explicitly (`git add <file>`). Only task-gitops and task-doc-updates push.
7. Keep task files short and precise: tables instead of prose, no text copied from other files (except in `pr.md`, which must stand alone). `scope.md` fits on about one page; `plan.md` stays under about 200 lines.
8. Check in proportion to the change: do not rerun a passing check without a change in between; no mutation testing and no prototypes of the planned code.
9. End with the next step, in the form `Next: <skill>`.

## Increment budget

A task is one logical increment: testable on its own, and every existing test still passes.

- At most about 300 changed lines of production code and 10 files; tests, documents and generated files such as lock files do not count.
- At most one new runtime dependency; development tools, such as a linter, do not count.
- Tests are essential, not exhaustive: one test for the success path of each requirement, plus one for each error case that protects security or data. A requirement usually needs one to three tests.

Larger work is split. A part that is not implemented yet is replaced by an explicit placeholder (a stub, a fake adapter, seeded dummy data, a monkey patch in tests), marked in code with `TODO(TD-<slug>-<n>)` and recorded in `work/<slug>/tech-debt.md`.
