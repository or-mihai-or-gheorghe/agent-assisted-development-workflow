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

## Development workflow

Every change goes through the phases described in `AGENTS.md`, each one a skill in `.agents/skills/`. A task starts with `/task-scoping`, which proposes the next roadmap item when it gets no request. The files of a task are in `work/<slug>/` while it is open and in `work/done/<slug>/` after it is finished.
