# Skills

A personal skill set for the mission flow.

## Rules

- Skills are markdown. Keep `SKILL.md` concise and process-driven, with a why for every rule.
- Before writing or editing any skill, load `emil-writing-skills` with the Skill tool. Every rule carries its why, every line changes what the agent does, and the skill is tested by running it. Why: the prior session wrote an unasked `pushback` skill without the standard, and the result was a rule list, not a procedure.
- No em-dashes.
- Handoff prompts are delivered in chat. Never put a delivered prompt in an issue, a PR, a commit, the repo, or a mission document. A prompt shape in a skill is a template, not a delivered prompt.
- A gate is approved only by the owner's words in the conversation. Never record an approval the owner did not give. A handoff prompt reports a gate, never grants it. Why: the agent writes the record, so a record can only check the agent against itself.
- Do not create or update the mission record before the first gate, except for a wayfinder map, which is the fog path's decision artifact and is created before its decision gates. The mission document is the work item issue: the bug or change issue, the spec, or the map. Never a separate mission issue. Why: a record created at Start and updated after every decision was churn, and the state was buried in the history.
- The record is a snapshot, not a diary. Write it at the gate, at a handoff, on a scope change, or when a false record would mislead. The PR carries its own status. Why: per-decision comments bury the current state.
- The record is current state only. Do not add history of any kind: prior attempts, prior sessions, prior issues, prior missions, or earlier versions. Why: history invites the next session to re-litigate settled work.
- Never reproduce the user's words directly in any artifact, and never mention the user. State decisions in neutral, general language. Do not name who decided. This applies to issues, comments, PR bodies, commits, specs, tickets, ADRs, handoffs, reviews, and replies. Each skill that produces an artifact repeats this rule because a skill can load without the others. Why: a quote or a name turns one message into durable text the next session can read as authority.
- The gates depend on the scope. A change, bug, or trivial has the scope and understanding gate and the PR gate, not a spec or ticket gate. Why: a universal gate table asked the owner to approve a ticket on a bug, and the gate did not exist for that path.
- Keep the README skill list in sync when a skill is added, renamed, or removed.
- Install with `bunx --bun skills add eyenalxai/skills -g -s '*' -a universal -y`.
- This fork targets the universal agent.

## Scope

This fork is the mission flow, its direct dependencies, and a small set of utilities. Keep it to the skills listed in `README.md`.

## Agent skills

### Issue tracker

Linear, team SKILL, personal workspace, via `rata`. See `docs/agents/issue-tracker.md`.

### Triage labels

Default state labels; `bug` maps to `Bug` and `enhancement` maps to `Improvement`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: `GLOSSARY.md` and `docs/adr/` at the root. See `docs/agents/domain.md`.
