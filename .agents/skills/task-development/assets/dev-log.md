---
task: <slug>
artifact: dev-log
plan_version: 1
round: 1                 # +1 for each fix round after a review or a test report
status: in-progress      # in-progress | done
---

# Development log: <slug>

## Steps

| Plan step | Status | Commit | Notes |
| --- | --- | --- | --- |
| 1. <name> | done | `<sha>` | |

## Deviations from the plan

| What | Why | Impact |
| --- | --- | --- |
| None | | |

## Commands

| Command | Result |
| --- | --- |
| test: `<command>` | <n> passed, <n> failed |
| lint: `<command>` | <no problems / n problems> |

## Fix rounds

<From round 2: one table per round.>

### Round <n>: <review round n / test report round n>

| Finding | Fix | Commit |
| --- | --- | --- |
| F1 | | |

## Notes for review and testing

- <what the reviewer and the tester should know: setup, data, limitations>
