# Mission: Handoff delivery and upstream language

Status: active
Scope: change
Phase: code-review
Goal: Handoff prompts are delivered in chat only, never in an issue or PR, and the fork preserves the upstream author's language in the skills it adapts.
Exit: PR for user review and merge

## Flow

- [x] grill: one PR; preserve upstream wording and voice; fork-changed files only; prompt delivered in chat; no hotfix and no tracker edit before the PR
- [x] implement: mission, handoff, router, template, AGENTS.md, README
- [ ] code-review: Standards and Spec
- [ ] PR: user review and merge

## Gates

| Gate | Method | Document | Status | Approved by | Date |
| --- | --- | --- | --- | --- | --- |
| Scope and understanding | grill (grilling only; no GLOSSARY.md or docs/adr) | this document plus grill answers | approved | user | 2026-10-29 |
| Spec | user review | not needed for a change | skipped | user (scope gate) | 2026-10-29 |
| Tickets | user review | not needed for a change | skipped | user (scope gate) | 2026-10-29 |
| PR | code-review | TBD | pending | | |

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
- Preserve the upstream wording verbatim wherever a skill is copied. Write only new text in the same direct, second-person voice.
- Limit the language pass to the files this fork changed: `mission`, `router`, `handoff`, `README`, `AGENTS.md`, and `setup-agent-skills`.
- The prompt is delivered in chat. The mission document records a one-line log entry only.
- No global skill hotfix and no JYO-33 edit before the PR. Follow the process: implement, review, PR, user merge, then reinstall.
- Keep the optional handoff summary in `$TMPDIR`. It is not the mission document and it never carries the prompt.

## Artifacts

- spec: not needed (change)
- tickets: not needed (change)
- PRs: TBD

## Next step

- Skill: code-review
- Gate: Scope and understanding approved
- Method: two-axis review
- Command: run `code-review` against `cad4d38`
- Produce: Standards and Spec findings

## Handoff log

- None yet. Handoff prompts are delivered in chat, never pasted here.
