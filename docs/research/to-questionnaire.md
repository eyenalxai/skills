# Restore `to-questionnaire`

Research date: 2026-11-02. Fork baseline: `1b8ca90` (`feat/restore-questionnaire-skill`, after PR #9).

## Verdict

Restore one file, `skills/productivity/to-questionnaire/SKILL.md`, byte-identical to upstream. Add a README entry, which the repo rules require. Add a router entry for consistency. Do not restore the upstream docs page. The skill content needs no edits for this fork's rules.

The report lives in `docs/research/`, a new directory: baseline `docs/` has only agent setup notes, and the repo has no research notes convention.

## Verified facts

### Upstream

1. Upstream `main` still has the skill. The raw file at <https://raw.githubusercontent.com/mattpocock/skills/main/skills/productivity/to-questionnaire/SKILL.md> returned HTTP 200 and 2,904 bytes when fetched on 2026-11-02. The GitHub contents API lists it as a file with blob SHA `dadd0c00d6a350acaa15bb9ca1b96b20ead684ca` (<https://api.github.com/repos/mattpocock/skills/contents/skills/productivity/to-questionnaire/SKILL.md>).

2. The upstream `main` head at verification was `4ae2317ddb22b046e26455e4f9c9b3aaaf62212d`, committed 2026-10-31, message `software-factory: glossary skill for designing a factory (#1316)` (<https://api.github.com/repos/mattpocock/skills/commits/main>).

3. Upstream path history for this file has only two commits (<https://api.github.com/repos/mattpocock/skills/commits?path=skills/productivity/to-questionnaire/SKILL.md>):
   - `0f2bdbdb06220d2df3718b8f0483157c6c8a8600`, 2026-07-28, `feat: graduate to-questionnaire into the productivity bucket`.
   - `321658273cb1d20b76026717d027d505790106d4`, 2026-08-19, `Remove all em-dashes from the repo`.

4. The upstream file is byte-identical to the copy deleted in the prune. Both have SHA-256 `b5eb929842ee0e93d867c5e906d183d350f2f2d149eaeaa86967d94d8eda1d3b`, and both hash to git blob `dadd0c00d6a350acaa15bb9ca1b96b20ead684ca`. `diff -u` between `git show cca2dc0^:skills/productivity/to-questionnaire/SKILL.md` and the fetched upstream file produced no output.

5. Upstream lists the skill in its README at line 252 (<https://raw.githubusercontent.com/mattpocock/skills/main/README.md>).

### This fork

6. The prune commit `cca2dc0bd48128cf3bae5ed1be1b0a011dcbb12b` is dated 2026-10-29 and deleted the skill entry, including `skills/productivity/to-questionnaire/SKILL.md` (54 lines) and `docs/productivity/to-questionnaire.md` (78 lines) (`git show cca2dc0 --stat -- skills/productivity/to-questionnaire docs/productivity/to-questionnaire.md`).

7. The prune parent is `4c68988a50aa791c6cb657dd76385a05bcd3fa31` (`git rev-parse cca2dc0^`).

8. Baseline `1b8ca90` does not contain the skill. `git grep -n -i questionnaire 1b8ca90 -- .` returns no matches. There are no other references in the repo at baseline.

9. The folder convention is `skills/<bucket>/<skill>/SKILL.md`. Baseline has 22 `SKILL.md` files (`git ls-tree -r --name-only 1b8ca90 -- skills`). The `skills/productivity/` bucket holds `grilling`, `grill-me`, `handoff`, and `wait-what`, so `to-questionnaire` fits that bucket unchanged.

10. The deleted copy uses `name`, `description`, and `disable-model-invocation: true` at lines 1 to 5 of `git show cca2dc0^:skills/productivity/to-questionnaire/SKILL.md`. Every field it uses is current, and the field order matches the other skills.

11. The deleted copy has zero em dashes. A character scan found one en dash, in `steps 1–2` at line 16. `AGENTS.md` line 8 bans em dashes only, so no punctuation edit is required.

12. `AGENTS.md` rules that bear on the restore:
    - Line 7: `Skills are markdown.`
    - Line 10: `Keep the README skill list in sync when a skill is added, renamed, or removed.`
    - Line 11: install uses `-s '*'`, so there is no per-skill manifest to update.
    - Line 12: `This fork targets the universal agent.`
    - Line 16: scope is the mission flow, its direct dependencies, and a small set of utilities.

13. `README.md` at baseline has a `### Utilities` section at lines 71 to 73, containing only `wait-what`. The restore must add a bullet there to satisfy `AGENTS.md` line 10.

14. `skills/engineering/router/SKILL.md` at baseline describes itself as `A router over the skills in this repo` at line 3. Its `## Standalone` list at lines 107 to 111 names `grill-me`, `grilling`, `prototype`, `research`, and `handoff`, but not `to-questionnaire`. No written rule requires a router entry, but the list is the router's inventory of standalone skills, so leaving it out makes the router incomplete.

15. Baseline `docs/` contains only `docs/agents/domain.md`, `docs/agents/issue-tracker.md`, and `docs/agents/triage-labels.md` (`git ls-tree -r --name-only 1b8ca90 -- docs`). There is no `docs/productivity/` and no per-skill docs convention, so the deleted `docs/productivity/to-questionnaire.md` has no home in this fork.

### Upstream drift after the fork point

There is none for this skill. The last upstream commit touching the file is `3216582` on 2026-08-19, which is before the fork prune on 2026-10-29. The current upstream file is byte-identical to the fork's deleted copy, so there is nothing newer to carry over.

## Restore plan

1. Create `skills/productivity/to-questionnaire/SKILL.md` with the exact content of `git show cca2dc0^:skills/productivity/to-questionnaire/SKILL.md`, which equals the current upstream file (blob `dadd0c0`). The content already satisfies the fork's rules: no em dashes, supported frontmatter fields, and a `productivity` bucket path.
2. Leave the frontmatter as `name`, `description`, `disable-model-invocation: true`. Do not add `argument-hint` or any other field. Keeping the flag preserves upstream behavior: the skill is user-invoked, and the flag stops the agent from invoking it on its own.
3. Do not restore `docs/productivity/to-questionnaire.md` or create any other file for the skill. The fork ships one file per skill and has no per-skill docs pages.
4. Add this bullet under `### Utilities` in `README.md`, above the existing `wait-what` bullet:
   `- **to-questionnaire**: Turn a decision you can't answer alone into a questionnaire for the one person who can fill it in.`
5. Add this bullet under `## Standalone` in `skills/engineering/router/SKILL.md`, after the `research` bullet:
   `` - `to-questionnaire` turns a decision someone else holds into a questionnaire. ``
6. Do not change `mission` or any flow skill. The router edit is a registration change, not a behavior change.
7. Verify with `grep -rn $'\u2014' skills/productivity/to-questionnaire` (expect no matches) and confirm the install wildcard picks the skill up without a manifest change.

## Primary sources

- Upstream raw file: <https://raw.githubusercontent.com/mattpocock/skills/main/skills/productivity/to-questionnaire/SKILL.md>
- Upstream main head: <https://api.github.com/repos/mattpocock/skills/commits/main> (`4ae2317ddb22b046e26455e4f9c9b3aaaf62212d`, 2026-10-31)
- Upstream path history: <https://api.github.com/repos/mattpocock/skills/commits?path=skills/productivity/to-questionnaire/SKILL.md> (latest `3216582`, 2026-08-19)
- Fork prune: `cca2dc0bd48128cf3bae5ed1be1b0a011dcbb12b`, parent `4c68988a50aa791c6cb657dd76385a05bcd3fa31`
- Fork baseline: `1b8ca905beb1830775e65bfd534756e224dcbe55`
