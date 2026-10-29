# Mission: Load a skill before producing its output

Status: active
Scope: change
Phase: review
Goal: Every phase starts by loading the skill that owns its procedure, so no agent improvises a round, a review, or a spec from a summary.
Exit: PR for user review and merge

## Flow

- [x] grill: diagnosis accepted; A, A, A approved
- [x] implement: mission, router, handoff, template
- [x] code-review: Standards and Spec, findings fixed
- [ ] PR: user review and merge

## Gates

| Gate | Method | Document | Status | Approved by | Date |
| --- | --- | --- | --- | --- | --- |
| Scope and understanding | grilling (no GLOSSARY.md or docs/adr) | this document plus the answers | approved | user | 2026-10-29 |
| Spec | user review | not needed for a change | skipped | user (scope gate) | 2026-10-29 |
| Tickets | user review | not needed for a change | skipped | user (scope gate) | 2026-10-29 |
| PR | code-review | not opened yet | pending | | |

## Session plan

| Session | Work | Exit |
| --- | --- | --- |
| 1 | classify, grill, implement, review, PR | PR URL, head SHA, checks |

## References

| Reference | Why | Path or URL | Phase |
| --- | --- | --- | --- |
| Failing session | mission and router loaded; grilling and domain-modeling not loaded; the first reply wrote a round without calling the Skill tool | quoted in chat | grill, implement |
| Mission skill | Delegated the grill with "model-invoked; call them" and "mission runs them as part of the flow" | skills/engineering/mission/SKILL.md | grill, implement |
| Router skill | Required reading a SKILL.md only before stating what a skill does | skills/engineering/router/SKILL.md | grill, implement |
| Handoff skill | Named "the skill to call first" and the suggested skills | skills/productivity/handoff/SKILL.md | implement |
| Wayfinder skill | The correct phrasing: "Call the Skill tool twice" | skills/engineering/wayfinder/SKILL.md | implement |
| Grilling skill | Owns the round format, the frontier, and the wait | skills/productivity/grilling/SKILL.md | implement |
| Repo rules | No em-dashes, concise skills, README sync | AGENTS.md | all |

## Decisions

- The failure is a skill-text defect, not a harness defect: `grilling` was available and listed; the entry skills never told the agent to load it.
- Fix shape A: mission keeps the policy it owns and drops the how-to; add the load rule and the stop to mission, router, and the handoff prompt.
- Stop rule A: if a named skill will not load, the session halts, names the skill, and does not simulate its output.
- Prompt chain A: the handoff prompt names the full load order, `wayfinder`, then `grilling` and `domain-modeling` for a grilling ticket.
- Scope is the entry skills and the prompt they write, not every skill in the repo. The others already say "Call the Skill tool".
- No README change: no skill added, renamed, or removed. No GLOSSARY or ADR: the wording fix is reversible.
- Verification: grep for the old phrases, read the diff, then replay the failing prompt after merge and reinstall.
- Code-review findings fixed: `domain-modeling` is conditional in every file; "artifact" became "output"; the stop rule has one canonical statement per entry skill plus the prompt; the router keeps the skip guard; the prompt names the chain in order and forbids simulation; the mission document no longer names an absolute skill path or an empty PR token.

## Outputs

- spec: not needed (change)
- tickets: not needed (change)
- PRs: none yet

## Next step

- Skill to load: pr
- Gate: PR
- Method: user review
- Command: open the PR on fix/load-skill-before-perform
- Produce: PR URL, head SHA, checks

## Handoff log

- None yet.
