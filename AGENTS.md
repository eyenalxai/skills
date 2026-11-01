# Skills

A personal skill set for the mission flow.

## Rules

- Skills are markdown, with no YAML metadata files. Keep `SKILL.md` concise and process-driven.
- No em-dashes.
- Handoff prompts are delivered in chat. Never put a delivered prompt in an issue, a PR, a commit, the repo, or a mission document. The mission document records a one-line log entry. A prompt shape in a skill is a template, not a delivered prompt.
- Keep the README skill list in sync when a skill is added, renamed, or removed.
- Install with `bunx --bun skills add eyenalxai/skills -g -s '*' -a universal -y`.
- This fork targets the universal agent.

## Scope

This fork is the mission flow, its direct dependencies, and a small set of utilities. Keep it to the skills listed in `README.md`.

## Agent skills

### Issue tracker

Issues live in Linear (team SKILL, personal workspace), driven with the `rata` CLI; PRs carry `SKILL-<n>` and `Closes SKILL-<n>`. See `docs/agents/issue-tracker.md`.

### Triage labels

The five state roles use their default label strings; `bug` maps to `Bug` and `enhancement` maps to `Improvement`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: `GLOSSARY.md` and `docs/adr/` at the repository root. See `docs/agents/domain.md`.
