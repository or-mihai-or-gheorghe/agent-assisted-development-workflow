---
task: <slug>
artifact: pr
branch: <slug>
base: <base branch>
updated_commit: none     # HEAD after updating the branch from the base branch, if it was updated
pr: not created          # the pull request's address, once created
---

# <title, in the project's commit format>

## Summary

<What the increment adds, from scope.md, in two or three lines.>

## Changes

- <change, with the main files>

## How to test

```bash
<install command>
<test command>
```

<Manual checks, from test-report.md.>

## Quality gates

| Gate | Task file | Result |
| --- | --- | --- |
| Plan critique | `critique.md`, round <n> | approved |
| Code review | `review.md`, round <n> | approved |
| Tests | `test-report.md`, round <n> | pass |

## Tech debt

- <TD id>: <description>, or None

## Follow-ups

- <slug>: <one line>, or None
