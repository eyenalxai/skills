# Mission: Restore wait-what and the setup skill

Status: active
Scope: change
Stage: PR
Goal: Restore `wait-what` and the upstream setup skill, adapted to this fork with no mentions of its original author.
Exit: PR for owner review and merge

## Route

- [x] grill: restore both named skills only; setup skill named `setup-agent-skills`; `wait-what` unchanged; adapt all author and removed-skill mentions; no references doc
- [x] implement: restore `wait-what` and `setup-agent-skills`, update README
- [x] code-review: Standards and Spec review, findings fixed in `cb06d5a`
- [ ] owner merge: PR

## Gates

| Gate | Method | Artifact | Status | Approved by | Date |
| --- | --- | --- | --- | --- | --- |
| Scope and understanding | process grill (grilling only; no GLOSSARY.md or docs/adr) | this artifact plus grill answers | approved | owner | 2026-10-29 |
| Spec | owner review | not needed for a change | skipped | owner (scope gate) | 2026-10-29 |
| Tickets | owner review | not needed for a change | skipped | owner (scope gate) | 2026-10-29 |
| PR | code-review | https://github.com/eyenalxai/skills/pull/1 | pending | | |

## Session plan

| Session | Work | Exit |
| --- | --- | --- |
| 1 | classify, grill, implement, review, PR | PR URL, head SHA, checks |

## Decisions

- No repo-level references doc. References are per thread, so setup writes only the tracker, labels, and domain docs.
- No full upstream re-import. Restore `wait-what` and the setup skill only.
- `wait-what` body stays unchanged. It already reads `GLOSSARY.md` and `GLOSSARY-MAP.md`.
- The setup skill is named `setup-agent-skills` and lives at `skills/engineering/setup-agent-skills/`.
- Setup skill replaces every upstream-author mention with this fork's repo, and names only skills that exist here.
- Setup skill does not seed `docs/agents/references.md`.
- Triage labels cover the two category roles and five state roles that the `triage` skill uses.
- `triage` points at `/setup-agent-skills` for its label mapping.
- The mission reads repo agent docs, not a repo-level references file.
- `AGENTS.md` scope sentence now names the small utilities set, because `wait-what` is a utility and not a mission dependency.
- The `LICENSE` keeps its MIT copyright notice by owner decision.

## Artifacts

- spec: not needed (change)
- tickets: not needed (change)
- PRs: https://github.com/eyenalxai/skills/pull/1

## Next step

- Skill: none
- Gate: PR
- Method: owner review in GitHub
- Command: review and merge https://github.com/eyenalxai/skills/pull/1
- Produce: merged PR

## Handoff log

- 2026-10-29: session 1, grill, scope and understanding gate, process grill, approved by owner
- 2026-10-29: session 1, implement, implementation done, next is code-review
- 2026-10-29: session 1, PR, code-review done, PR opened, owner review pending
