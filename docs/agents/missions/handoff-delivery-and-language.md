# Mission: Handoff delivery and upstream language

Status: active
Scope: change
Phase: PR
Goal: Handoff prompts are delivered in chat only, never in an issue, PR, commit, repo, or mission document, and the fork preserves the upstream author's language in the skills it adapts.
Exit: PR for user review and merge

## Flow

- [x] grill: one PR; preserve upstream wording and voice; fork-changed files only; prompt delivered in chat; no hotfix and no tracker edit before the PR
- [x] implement: mission, handoff, router, template, AGENTS.md, README; setup reviewed, no wording change
- [x] code-review: Standards and Spec, three rounds, no blocking findings
- [ ] PR: user review and merge

## Gates

| Gate | Method | Document | Status | Approved by | Date |
| --- | --- | --- | --- | --- | --- |
| Scope and understanding | grill (grilling only; no GLOSSARY.md or docs/adr) | this document plus grill answers | approved | user | 2026-10-29 |
| Spec | user review | not needed for a change | skipped | user (scope gate) | 2026-10-29 |
| Tickets | user review | not needed for a change | skipped | user (scope gate) | 2026-10-29 |
| PR | code-review | https://github.com/eyenalxai/skills/pull/3 | pending | | |

## Session plan

| Session | Work | Exit |
| --- | --- | --- |
| 1 | classify, grill, implement, review, PR | PR URL, head SHA, checks |

## References

| Reference | Why | Path or URL | Phase |
| --- | --- | --- | --- |
| Mission skill | The defect: it tells the agent to put the prompt in the document | skills/engineering/mission/SKILL.md | grill, implement, review |
| Handoff skill | The prompt contract and the temp document | skills/productivity/handoff/SKILL.md | grill, implement, review |
| Router skill | Next-step answer and prompt delivery | skills/engineering/router/SKILL.md | implement, review |
| Mission template | Handoff log shape | skills/engineering/mission/mission-template.md | implement |
| Repo rules | No em-dashes, concise skills, README sync | AGENTS.md | all |
| Upstream fork point | The original author's language to preserve | commit 59db609 | grill, implement |
| JYO-33 | Incident: the prompt was pasted into the mission issue | https://linear.app/utbetdix/issue/JYO-33/mission-screen-sync | grill, review |

## Decisions

- One PR covers both fixes. They touch the same files and share one review.
- Preserve the upstream wording verbatim wherever a skill is copied. Write only new text in the same direct, second-person voice. The language pass covers the fork's new prose in `mission`, `router`, `README`, the handoff additions, and `mission-template.md`; it does not rewrite untouched upstream skills. `setup-agent-skills` was reviewed and needs no wording change because its differences are the fork's names and label vocabulary.
- Align the fork's vocabulary with the upstream author's: user instead of owner, phase instead of stage, document instead of artifact, flow instead of route, grill instead of process grill. A per-scope path is a path; the whole mission path is the flow.
- The prompt is delivered in chat. The mission document records a one-line log entry only.
- No global skill hotfix and no JYO-33 edit before the PR. Follow the process: implement, review, PR, user merge, then reinstall.
- Keep the optional handoff summary in `$TMPDIR`. It is not the mission document and it never carries the prompt.

## Outputs

- spec: not needed (change)
- tickets: not needed (change)
- PRs: https://github.com/eyenalxai/skills/pull/3

## Next step

- Skill: none until merge
- Gate: PR
- Method: user review
- Command: review and merge https://github.com/eyenalxai/skills/pull/3
- Produce: merged PR, then reinstall the global skills

## Handoff log

- 2026-10-29: session 1, code-review, Standards and Spec, three rounds, no blocking findings.
- 2026-10-29: session 1, PR, opened https://github.com/eyenalxai/skills/pull/3 at `a17aa47`; no checks configured; mergeable; user review pending.
- None yet for a handoff. Handoff prompts are delivered in chat, never pasted here.
