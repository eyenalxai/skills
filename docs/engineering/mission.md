## What it does

`mission` is the single entry for one piece of work that should end in pull requests you can review and merge. You describe the request, and it classifies the scope, runs a process grill, keeps a mission artifact, and drives the route stage by stage.

The defining constraint: the mission artifact is the state. The conversation can end, the [harness](https://www.aihero.dev/ai-coding-dictionary/harness) can change, and a fresh [session](https://www.aihero.dev/ai-coding-dictionary/session) resumes from the artifact without re-deriving the route or the shared understanding. The stage skills keep their own procedures; `mission` owns the route, the gates, the references, and the handoffs.

## When to reach for it

You invoke this by typing `/mission`, and the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) won't reach for it on its own. Reach for it when you want one entry that carries a request through grilling, research, [spec](https://www.aihero.dev/ai-coding-dictionary/spec), [tickets](https://www.aihero.dev/ai-coding-dictionary/ticket), implementation, review, and PRs.

For a single routing question, use [ask-matt](https://aihero.dev/skills-ask-matt). For a standalone grill, use [grill-with-docs](https://aihero.dev/skills-grill-with-docs). For a handoff outside a mission, use [handoff](https://aihero.dev/skills-handoff).

## Prerequisites

A working directory. A configured issue tracker if you want the mission artifact in Linear or GitHub; [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) configures it. Without a tracker, the mission lives in `docs/agents/missions/`.

## The process grill

The first gate is a grill, and it runs as part of the route. `grilling` and `domain-modeling` are model-invoked, so the mission calls both when the repo has a glossary or ADRs. Without docs, it calls `grilling` alone. The user-invoked [grill-with-docs](https://aihero.dev/skills-grill-with-docs) shortcut wraps the same pair for a grill outside a mission.

The depth comes from the frontier, not from the scope. A clear bug may be one question about the repro and the expected behavior. A foggy idea may take many rounds. `domain-modeling` writes only resolved terms and ADR-worthy decisions, so a short grill can leave the docs untouched.

## Gates

The gates are mandatory; the method scales with the situation.

| Gate | Method | The owner approves | Next |
| --- | --- | --- | --- |
| Scope and understanding | process grill, research, diagnosis, an existing spec or ticket, or a direct confirmation | problem, constraints, approach, route | spec or issue |
| Spec | the owner reads the spec or issue | requirements, non-goals, acceptance | tickets |
| Tickets | the owner reads the ticket graph or issue | ticket graph, blocking edges, acceptance | implement |
| PR | `code-review` | diff, evidence, merge danger | merge |

At a pending gate the mission presents the artifact, the questions, and the decision it needs. It records the approval, then advances. A pending gate is the next step; silence is not approval.

## The mission artifact

The mission is one durable artifact: a Linear issue or project, a GitHub issue, or a repo file. It carries the scope, route, session plan, gates, references, decisions, artifacts, next step, and handoff log.

A fresh session reads it first, verifies the references, and resumes at the current gate. The chat is disposable.

## Handoff is part of the flow

The mission plans its sessions. After the tickets, before each implementation, and after each PR, the next session starts with a handoff prompt. The prompt names the mission, the current gate and method, the next ticket, the references, the checks, and the exit.

The decision is automatic. The prompt is automatic. The transport depends on the harness: a thread launcher can open the next session, or the owner pastes the prompt into a fresh one. `/mission handoff` is the manual override.

## References

The mission artifact carries the reference index: source paths, vendor source, API docs, examples, ADRs, and research. At each stage the skill suggests the top references for that stage. At session start it checks that each path and URL still resolves, so stale references are visible instead of lost.

## Common questions

**Can I skip the grill for a small bug?**
The depth scales, the gate stays. A clear bug is one confirmation of the repro and the expected behavior. The mission records that confirmation as the understanding gate.

**Do I type `/grill-with-docs` inside a mission?**
Inside a mission the grill runs as part of the route, so you answer the questions. Use [grill-with-docs](https://aihero.dev/skills-grill-with-docs) when you want a grill on its own.

**Who approves the gates?**
The owner. The mission records the approval and the date in the artifact. A pending gate stops the route until the owner answers.

**Why verify specs and tickets separately?**
The spec gate checks the problem, the requirements, and the non-goals. The ticket gate checks the slicing, the blocking edges, and the acceptance. Implementation starts after both.

**Does it merge the PRs?**
It opens one PR per ticket, runs `code-review`, fixes the findings, and hands the PR back. The owner reviews and merges.

**Can it open the next session?**
When the harness exposes a thread launcher, yes. Otherwise it delivers the prompt in a fenced block for the owner to paste. The prompt points at the mission artifact first.

**How do references survive between sessions?**
They live in the mission artifact's reference index. The handoff prompt carries the mission path and the next stage's top references. A new session verifies the links before using them.

## It's working if

- Every reply names the mission, stage, gate, method, next skill, and next command.
- Every gate is approved before the next stage.
- The process grill runs at every scope, and its depth follows the frontier.
- A fresh session resumes from the artifact without asking you to re-explain.
- A handoff appears at the planned boundary with the prompt already written.
- Every ticket ends at a PR with `code-review`, and you review and merge it.
- Stale references are flagged at the start of a session.

## Where it fits

`mission` is the single entry that wraps the chain: [ask-matt](https://aihero.dev/skills-ask-matt) classifies, `grilling` and `domain-modeling` build the shared understanding, the stage skills do the work, [code-review](https://aihero.dev/skills-code-review) checks the diff, [pr](https://aihero.dev/skills-pr) shapes the body, and [handoff](https://aihero.dev/skills-handoff) carries the next session. It sits beside the router rather than replacing it.
