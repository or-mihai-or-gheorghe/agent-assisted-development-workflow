# Task API

A REST API for personal tasks (to-dos): users register, sign in with a JSON Web Token and manage their own tasks. Node.js and Express 5; data in a local JSON file first, in Cloud Firestore later.

Status: no code yet. The increments are listed in `docs/ROADMAP.md`.

## Features

| Feature | Interface | Since |
| --- | --- | --- |
| None yet | | |

## Requirements

- Node.js 22 or newer, with npm
- Git, and GitHub CLI (`gh`) for pull requests
- A coding agent that reads skills, for example Antigravity CLI (`agy`)

## Development workflow

Every change is a task that goes through eight phases, each one a skill in `.agents/skills/`; the rules are in `AGENTS.md`. A task starts with `/task-scoping`, which takes the next roadmap item when it gets no request. The later phases (`/task-planning`, `/task-plan-critique`, `/task-development`, `/task-code-review`, `/task-testing`, `/task-gitops`, `/task-doc-updates`) find the task from the current branch. The files of a task are in `work/<slug>/` while it is open and in `work/done/<slug>/` after it is finished.

The skills do not depend on this project; `.agents/README.md` explains how to use them in another one.
