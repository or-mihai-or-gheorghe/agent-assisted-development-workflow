---
task: <slug>
artifact: test-report
round: 1
tested_commit: <sha>
verdict: fail            # pass | fail
---

# Test report: <slug>, round <n>

## Environment

- <runtime and version>, <operating system>, <storage or external services>

## Automated tests

- Clean install and test command: <n> passed, <n> failed, <n> skipped

| Plan test | Exists | Result |
| --- | --- | --- |
| T1 | yes | pass |

## Acceptance criteria

| Requirement | Verified by | Result |
| --- | --- | --- |
| R1 | T1, check E1 | pass / fail |

## Exploratory checks

| ID | Action | Expected | Actual | Result |
| --- | --- | --- | --- | --- |
| E1 | <request, command or user action> | <expected result> | <actual result> | pass / fail |

## Failures

| ID | Description | Steps to reproduce | Severity |
| --- | --- | --- | --- |
| None | | | |

## Known issues to record

| Description | Severity |
| --- | --- |
| None | |

## Decision

<pass: continue with task-gitops. fail: back to task-development.>
