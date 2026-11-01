# Mission: Agent reports carry the why and the whole picture

Status: active
Scope: change
Phase: PR
Goal: Every mission reply states why the step exists, why this route, what it unlocks, and where it sits in the flow, so the user can judge progress without decoding labels.
Exit: PR for user review and merge

## Flow

- [x] grill: scope change, placement A, depth A, trail A; user approved
- [x] implement: mission, template, handoff, implement, pr, router updated
- [x] code-review: Standards and Spec, findings fixed
- [ ] PR: user review and merge

## Gates

| Gate | Method | Document | Status | Approved by | Date |
| --- | --- | --- | --- | --- | --- |
| Scope and understanding | grilling plus domain-modeling (repo has docs) | this document plus the answers | approved | user | 2026-11-01 |
| Spec | user review | not needed for a change | skipped | user (scope gate) | 2026-11-01 |
| Tickets | user review | not needed for a change | skipped | user (scope gate) | 2026-11-01 |
| PR | code-review | not opened | pending | | |

## Session plan

| Session | Work | Why | Exit |
| --- | --- | --- | --- |
| 1 | classify, grill, implement, review, PR | the defect is in the skill text, so the fix must live where a fresh session reloads it | PR URL, head SHA, checks |

## References

| Reference | Why | Path or URL | Phase |
| --- | --- | --- | --- |
| Repo agent rules | no em-dashes, concise process-driven skills, README sync | AGENTS.md | all |
| Mission skill | owns the output contract being replaced | skills/engineering/mission/SKILL.md | grill, implement, review |
| Mission template | records the why in the session plan and decisions | skills/engineering/mission/mission-template.md | implement |
| Handoff skill | carries the why into the next session prompt | skills/productivity/handoff/SKILL.md | implement |
| Implement skill | adds step-level reporting | skills/engineering/implement/SKILL.md | implement |
| PR skill | adds why and unlock to the PR body | skills/engineering/pr/SKILL.md | implement |
| Router skill | next-step line carries the why | skills/engineering/router/SKILL.md | implement |
| Prior mission | precedent for a one-session skill-text change | docs/agents/missions/load-skill-before-perform.md | grill |

## Decisions

- The defect is skill text, not one chat: the contract asks for state, never reasoning, alternatives, unlocks, or an overall progress view.
- Q1 yes: scope `change`, one session, one branch, one PR, tracked in this document; spec and tickets skipped because the diff is the reviewable document.
- Q2 A: the contract lives in `mission` and the surfaces it hands work to: `handoff`, `implement`, `pr`, and `router`. Grill, spec, and tickets already ask for reasoning in their own formats.
- Q3 A: every reply carries `Why now` and `Unlocks`, and `Why this way` always, allowed to say `no alternative: <reason>` in one clause.
- Q4 A: one inline progress trail that starts with the mission and marks done, current, and pending nodes.
- Plain words before labels: an id, PR, branch, or file never stands alone.
- Ask when the why is unknown, and record the answer here. Never invent a rationale.
- No GLOSSARY.md or ADR yet: the contract is reversible and belongs in the skill text, not the project domain language. Revisit if the terms spread beyond the skills.
- No README change: no skill is added, renamed, or removed.
- The global copies under ~/.agents/skills are copies, not symlinks. Reinstall after merge.
- Branch `feat/agent-writing-contract` is cut from `origin/main` at `a6cb988`.
- Code-review findings fixed: the handoff bullet no longer repeats the field list; `implement` defers the ask rule to the mission contract and drops the tdd exception; the template's decision line has a no-alternative fallback; `router` states the standalone contract and its second example names the unlock; `handoff` defers its reply to the mission contract; `/mission status` shows the trail, why, and unlock; `pr` uses the one-clause no-alternative shape.
- Accepted from code-review: the five-surface spread (approved Q2), the `Unlocks` label (Q3), the replaced header with `Method` and `Read` removed (Q4), and "plain words before labels" (the defect being fixed).

## Outputs

- spec: not needed (change)
- tickets: not needed (change)
- PRs: not opened

## Next step

- Skill to load: pr
- Gate: PR pending
- Method: code-review
- Why: the diff is reviewed and the findings are fixed, so the next step is the reviewable PR the user merges.
- Unlocks: the user can merge, then reinstall the global skills so the new contract is live.
- Command: load pr, write the body, open the PR
- Produce: PR URL, head SHA, checks

## Handoff log

- 2026-11-01: session 1, classify, scope change, no handoff
- 2026-11-01: session 1, grill, four questions answered yes, scope and understanding gate approved by user
- 2026-11-01: session 1, implement, committed `142ac4e`
- 2026-11-01: session 1, code-review, Standards and Spec findings, fixes applied
