# Mission: Restore wait-what and the setup skill

Status: active
Scope: change
Stage: review
Goal: Restore `wait-what` and the upstream setup skill, adapted to this fork with no mentions of its original author.
Exit: PR for owner review and merge

## Route

- [x] grill: restore both named skills only; setup skill named `setup-agent-skills`; `wait-what` unchanged; adapt all author and removed-skill mentions; no references doc
- [x] implement: restore `wait-what` and `setup-agent-skills`, update README
- [ ] code-review: review the diff
- [ ] owner merge: PR

## Gates

| Gate | Method | Artifact | Status | Approved by | Date |
| --- | --- | --- | --- | --- | --- |
| Scope and understanding | process grill (grilling only; no GLOSSARY.md or docs/adr) | this artifact plus grill answers | approved | owner | 2026-10-29 |
| Spec | owner review | not needed for a change | skipped | owner (scope gate) | 2026-10-29 |
| Tickets | owner review | not needed for a change | skipped | owner (scope gate) | 2026-10-29 |
| PR | code-review | PR | pending | | |

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
- PRs: pending

## Next step

- Skill: `code-review`
- Gate: PR
- Method: Standards and Spec review of the diff
- Command: `/code-review` against `0b1110f`
- Produce: review findings, then one PR

## Handoff log

- 2026-10-29: session 1, grill, scope and understanding gate, process grill, approved by owner
- 2026-10-29: session 1, implement, implementation done, next is code-review
