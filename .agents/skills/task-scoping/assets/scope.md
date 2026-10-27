---
task: <slug>
artifact: scope
version: 1
status: draft            # draft | confirmed (only after the user confirms)
branch: <slug>
roadmap_item: <slug or "new">
---

# Scope: <title>

## Goal

<One or two sentences: what the user can do after this increment that they could not do before.>

## Requirements

| ID | Requirement | Acceptance criterion |
| --- | --- | --- |
| R1 | <what the system does> | <Given …, when …, then …> |

## Out of scope

- <what this task deliberately does not do>

## Options considered

| Option | Pros | Cons | Decision |
| --- | --- | --- | --- |
| <technology or approach> | | | chosen / rejected |

Sources:

- <link used in the research>

## Constraints and assumptions

- <versions, environment, data, conventions the scope relies on>

## Impact

- Code areas touched: <files or components>
- Blast radius: <low / medium / high, and why>
- Increment budget: <fits / does not fit; see Split>

## Split and placeholders

<Only when the request is larger than one increment: what this task covers, what becomes a follow-up, which placeholders keep the software working.>

## Follow-ups

- <slug>: <one line>

## Open questions

- None
