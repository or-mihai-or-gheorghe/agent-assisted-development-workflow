---
task: <slug>
artifact: review
round: 1
plan_version: 1
reviewed_commit: <sha>    # HEAD when the review started
verdict: changes-requested   # approved | changes-requested
---

# Code review: <slug>, round <n>

## Reviewed

- `git diff <base>...<sha>`: <n> files, +<n> / -<n> lines
- build: <result or none>; test: <n> passed, <n> failed; lint: <no problems / n problems>

## Increments

| Increment | Commit | Plan step or finding | Notes |
| --- | --- | --- | --- |
| 1 | `<sha>` | 1. <name> | <conforms / see F1> |

## Findings

| ID | Severity | Location | Finding | Rule | Required change |
| --- | --- | --- | --- | --- | --- |
| F1 | blocker / major / minor / nit | `<file>:<line>` | | <rule ID> | |

## Previous findings

<From round 2: whether each earlier finding is fixed.>

## Tech debt

- Every placeholder has a `TODO(<id>)` and a row in `tech-debt.md`: <yes / no>
- Accepted as debt in this review: <finding IDs, or none>

## Decision

<approved: continue with task-testing. changes-requested: blockers and majors go back to task-development.>
