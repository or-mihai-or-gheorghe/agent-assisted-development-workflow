---
task: <slug>
artifact: dev-log
plan_version: 1
round: 1                 # +1 for each fix round after a review or a test report
status: in-progress      # in-progress | done
---

# Development log: <slug>

One entry per increment, in commit order: a plan step, or a fix in a fix round. The code review reads the increments one by one, so each entry says what the reviewer needs to know about its commit.

## Increment 1: <plan step "1. <name>", or "round <n>: <finding ID>">

- Commit: `<sha>` <commit message>
- Changes: `<file>`: <what changed>; one line per file
- Decisions: <choices made while implementing that the plan did not fix, with the reason; or none>
- Tests: <test IDs added or changed, and what each one checks>
- Review focus: <where to look closely: security, error handling, edge cases, non-obvious logic>
- Deviation from the plan: <what, why, impact; or none>

## Commands

| Command | Result |
| --- | --- |
| test: `<command>` | <n> passed, <n> failed |
| lint: `<command>` | <no problems / n problems> |

## Notes for testing

- <what the tester should know: setup, data, how to start and use the software, limitations>
