# Skills

My agent skills for one guided flow: from a request, through grilling, docs, research, specs, tickets, implementation, and review, to pull requests you can review and merge.

This fork keeps the mission flow, its direct dependencies, and a small set of utilities. No plugin, no release tooling, no upstream contribution scaffolding.

## Install

Universal agent only:

```sh
bunx --bun skills add eyenalxai/skills -g -s '*' -a universal -y
```

## Entry point

Run `/setup-agent-skills` once per repo before the first mission. It records the issue tracker, triage labels, and domain docs under `docs/agents/`, and adds an `## Agent skills` pointer block to `AGENTS.md` or `CLAUDE.md`.

```text
/mission <request>
```

The mission:

- classifies the scope with `router`;
- runs the grill with `grilling` and `domain-modeling`;
- gates scope and understanding, spec, tickets, and the PR on your approval;
- plans the sessions and hands off with a prompt you can copy;
- keeps the mission document and its reference index across sessions;
- ends at one PR per ticket for you to review and merge.

Commands: `/mission continue`, `/mission status`, `/mission handoff`, `/mission done`.

Handoff prompts are delivered in chat. A delivered prompt is never pasted into an issue, a PR, a commit, the repo, or the mission document.

## Skills

### Setup

- **setup-agent-skills**: Configure a repo for the agent skills: issue tracker, triage labels, and domain docs. Run once before the first mission.

### Mission

- **mission**: Drive one piece of work from a request to reviewable PRs, with the grill, approval gates, session plan, references, and automatic handoffs.
- **router**: Name the skill or flow that fits, check the state when asked what's next, and return the next step. The mission calls it to classify.

### Flow

- **grilling**: The interview primitive. Relentless rounds until the design tree is resolved and shared understanding is confirmed.
- **grill-me**: The stateless grill for a plan or design with no repo.
- **grill-with-docs**: The standalone grill that also writes `GLOSSARY.md` and ADRs.
- **domain-modeling**: Sharpen the project's language and write the glossary and hard decisions.
- **wayfinder**: Chart a huge, foggy effort as decision tickets and resolve the way to the destination.
- **research**: Investigate a question against primary sources and leave a cited markdown file.
- **prototype**: Answer a design question with throwaway code.
- **to-spec**: Record the agreed spec without re-interviewing.
- **to-tickets**: Cut a spec into tickets with blocking edges.
- **implement**: Build one ticket and drive `tdd` at the agreed seams.
- **implement-spec**: Work the whole ticket graph on one integration branch.
- **tdd**: The red-green-refactor loop used by `implement`.
- **code-review**: Two-axis review of a diff: Standards and Spec.
- **pr**: Shape the pull request body, evidence, and merge danger.
- **diagnosing-bugs**: Build a tight feedback loop before fixing a hard bug.
- **triage**: Verify an incoming request and write an agent-ready issue.
- **codebase-design**: The deep-module vocabulary for specs, tickets, and seams.

### Handoff

- **handoff**: Write the handoff document and give you the next-session prompt in chat.

### Utilities

- **wait-what**: Stop and re-pitch the last message: simpler, with the context you were missing.
