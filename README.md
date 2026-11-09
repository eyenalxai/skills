# Skills

My agent skills for one guided flow: from a request, through grilling, docs, research, specs, tickets, implementation, and review, to pull requests you can review and merge.

This fork keeps the mission flow, its direct dependencies, and a small set of utilities. No plugin, no release tooling, no upstream contribution scaffolding.

## Install

Universal agent only:

```sh
bunx --bun skills add eyenalxai/skills -g -s '*' -a universal -y
```

## Entry point

Run `/setup-agent-skills` once per repo before the first mission. It records the issue tracker, triage labels, and domain docs under `docs/agents/`, and adds an `## Agent skills` pointer block to `AGENTS.md`.

```text
/mission <request>
```

The mission:

- classifies the scope with its scope table;
- runs the grill with `grilling` and `domain-modeling`;
- gates the scope with the spec on a feature or fog, the fog decisions, the tickets, and the PR on your approval;
- computes the ready frontier at every phase and runs up to ten tickets, stacking
  dependent tickets by blocking edge;
- plans the sessions, runs a frontier's build in the mission session with one
  subagent worker per ticket where the harness allows it, and hands off only
  when the work travels or the context cannot carry it;
- keeps one work item record and its reference index across sessions, updated
  only at boundaries;
- records current state in neutral language, without quotes, names, or history;
- ends at one PR per ticket for review and merge, bottom-up for a stack.

Commands: `/mission continue`, `/mission status`, `/mission handoff`, `/mission done`.

Handoff prompts are delivered in chat. A delivered prompt is never pasted into an issue, a PR, a commit, the repo, or the mission document.

## Skills

### Setup

- **setup-agent-skills**: Configure a repo for the agent skills: issue tracker, triage labels, and domain docs. Run once before the first mission.

### Mission

- **mission**: Drive one piece of work from a request to reviewable PRs, with the grill, approval gates, frontier plan, references, and handoffs when the work travels.
- **router**: Name the skill or flow that fits, check the state when asked what's next, including a stack's bottom PR, and return the next step. Use it standalone, or to name the next action when a mission is active.

### Flow

- **grilling**: The interview primitive. Relentless rounds until the design tree is resolved and shared understanding is confirmed.
- **grill-me**: The stateless grill for a plan or design with no repo.
- **grill-with-docs**: The standalone grill that also writes `GLOSSARY.md` and ADRs.
- **domain-modeling**: Sharpen the project's language and write the glossary and hard decisions.
- **wayfinder**: Chart a huge, foggy effort as decision tickets and resolve the way to the destination.
- **research**: Investigate a question against primary sources and leave a cited markdown file, or fan out independent questions in parallel.
- **prototype**: Answer a design question with throwaway code.
- **to-spec**: Record the agreed spec without re-interviewing.
- **to-tickets**: Cut a spec into tickets with surface ownership and blocking edges.
- **implement**: Build one ticket and drive `tdd` at the agreed seams.
- **implement-spec**: Run a frontier of two or more tickets as a crew chief: one worker per ticket where the harness allows it, one PR per ticket, stacked by blocking edge. A one-ticket frontier uses `implement`.
- **tdd**: The red-green-refactor loop used by `implement`.
- **code-review**: Two-axis review of a diff, or of each layer in a stack: Standards and Spec.
- **pr**: Shape the pull request body, evidence, and merge danger.
- **diagnosing-bugs**: Build a tight feedback loop before fixing a hard bug.
- **triage**: Verify an incoming request and write an agent-ready issue.
- **codebase-design**: The deep-module vocabulary for specs, tickets, and seams.

### Handoff

- **handoff**: Write the handoff document and give you the next-session prompt in chat, with the stack order and the merge order.

### Utilities

- **pushback**: The shared procedure for a challenge: name the claim and its check, stop on a failed check, and let the owner decide a preference.
- **improve-codebase-architecture**: Survey a codebase for implicit state machines, unenforced invariants, and deepening opportunities, then grill the chosen candidate or hand the candidates to the mission.
- **to-questionnaire**: Turn a decision you can't answer alone into a questionnaire for the one person who can fill it in.
- **wait-what**: Stop and re-pitch the last message: simpler, with the context you were missing.
