# Triage Labels

The skills speak in terms of two category roles and five state roles. This file
maps those roles to the label strings used in team SKILL.

## Category roles

| Canonical role | Label in our tracker | Meaning                    |
| -------------- | -------------------- | -------------------------- |
| `bug`          | `Bug`                | Something is broken        |
| `enhancement`  | `Improvement`        | New feature or improvement |

`Bug` is the workspace-level label (capitalized); do not create a second `bug`.
`Feature` exists in the team but is not one of the triage category roles.

## State roles

| Canonical role    | Label in our tracker | Meaning                                  |
| ----------------- | -------------------- | ---------------------------------------- |
| `needs-triage`    | `needs-triage`       | Maintainer needs to evaluate this issue  |
| `needs-info`      | `needs-info`         | Waiting on reporter for more information |
| `ready-for-agent` | `ready-for-agent`    | Fully specified, ready for an AFK agent  |
| `ready-for-human` | `ready-for-human`    | Requires human implementation            |
| `wontfix`         | `wontfix`            | Will not be actioned                     |

A live, triaged issue carries exactly one category role and one state role.
