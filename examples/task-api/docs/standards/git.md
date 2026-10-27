# Git standards

Scope: branches, commits and pull requests. Rule IDs (`GIT-<n>`) are cited in plans, critiques and reviews. The rules continue the first PWCC seminar ([Git and GitHub](https://mihai-gheorghe.gitbook.io/pwcc-suport-seminar/)).

## GIT-1. One branch per task

The branch has the task's slug as its name (`user-registration`) and starts from an up-to-date `main`. `main` always passes the tests.

## GIT-2. Conventional Commits

`<type>(<scope>): <description>`, in the imperative, lowercase: `feat(auth): add registration endpoint`. Types: `feat`, `fix`, `test`, `refactor`, `docs`, `chore`. Task files use `docs(<slug>): …`.

## GIT-3. Small commits

One logical step per commit; the tests pass after every commit.

## GIT-4. Nothing secret or generated in the repository

`.env`, keys, data files and `node_modules` are in `.gitignore`. `package-lock.json` is committed, and `npm ci` installs exactly the locked versions.

## GIT-5. Pull requests

One pull request per task, from the task branch to `main`. It is merged by a person, after the plan critique, the code review and the tests passed. Commits that were pushed are never rewritten.
