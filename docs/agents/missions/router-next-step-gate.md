# Mission: Router next-step gate and handoff boundary

Status: active
Scope: change
Stage: implement
Goal: Make the router answer "what's next" from state, gate the next ticket on merged blocking PRs, and hand off at ticket boundaries.
Exit: PR for owner review and merge

## Route

- [x] grill: scope change, mission deferral, merge gate, handoff timing approved
- [ ] implement: update router, mission, handoff, and README
- [ ] code-review: Standards and Spec review, fix findings
- [ ] owner merge: PR

## Gates

| Gate | Method | Artifact | Status | Approved by | Date |
| --- | --- | --- | --- | --- | --- |
| Scope and understanding | process grill (grilling only; no GLOSSARY.md or docs/adr) | this artifact plus grill answers | approved | owner | 2026-10-29 |
| Spec | owner review | not needed for a change | skipped | owner (scope gate) | 2026-10-29 |
| Tickets | owner review | not needed for a change | skipped | owner (scope gate) | 2026-10-29 |
| PR | code-review | pending | pending | | |

## Session plan

| Session | Work | Exit |
| --- | --- | --- |
| 1 | classify, grill, implement, review, PR | PR URL, head SHA, checks |

## References

| Reference | Why | Path or URL | Stage |
| --- | --- | --- | --- |
| Repo agent rules | No em-dashes, concise process-driven skills, README sync | AGENTS.md | grill, implement |
| Router skill | The behavior under change | skills/engineering/router/SKILL.md | grill, implement |
| Mission skill | Session boundaries and handoff contract | skills/engineering/mission/SKILL.md | grill, implement |
| Handoff skill | Prompt format | skills/productivity/handoff/SKILL.md | implement |
| Existing mission artifact | Local format precedent | docs/agents/missions/restore-wait-what-setup-skills.md | grill |

## Decisions

- A draft diff exists in the worktree from the pre-mission discussion. Implementation refines it after the scope gate.
- The global skills under `~/.agents/skills` are copies, not symlinks. Reinstall after merge so the new router is live.
- Keep this as a one-session change with one PR. Spec and tickets are skipped.
- When an active mission artifact exists, the router hands the next step to `/mission continue`. The router answers standalone when there is no mission.
- The merge gate covers every PR that blocks the next ticket, not just the immediately previous PR. It reads draft, check, and merge state. A stacked PR is allowed only on an explicit owner request.
- The handoff prompt waits until the blocking PR is merged, so it can name the merge commit and start the next ticket from it.

## Artifacts

- spec: not needed (change)
- tickets: not needed (change)
- PRs: pending

## Next step

- Skill: `implement`, then `code-review`
- Gate: PR
- Method: implementation, then two-axis review
- Command: implement the approved change, run `code-review`, fix the findings, open one PR
- Produce: reviewable PR

## Handoff log

- 2026-10-29: session 1, classify, scope change, router, no handoff
- 2026-10-29: session 1, grill, four questions answered yes, scope and understanding gate approved by owner
- 2026-10-29: session 1, implement, started
