---
name: improve-codebase-architecture
description: Survey a codebase for architectural improvements, rank them, and produce an executable plan with steps, acceptance tests, and dependency order, then feed the plan to a mission or a standalone decision. Use when the owner asks to audit architecture, find state machines or invariants, or run an architecture improvement pass.
disable-model-invocation: true
---

# Improve codebase architecture

Find the architectural changes worth making, rank them, and write a plan the mission can gate and execute. The plan names the module, the seam, the steps, the acceptance test, and the unslop check. The aim is testability, locality, and AI-navigability.

The planner reads the source and never edits it. It writes two files to the OS temp directory: a single-file HTML report for the owner and a compact candidate index for the mission. The standalone grill may update `GLOSSARY.md` and ADRs through `domain-modeling`; the build runs through `implement`.

Load `codebase-design` with the Skill tool before the first candidate. Use its vocabulary exactly: **module**, **interface**, **implementation**, **depth**, **seam**, **adapter**, **leverage**, **locality**. Why: the terms are the plan's precision, and synonyms drift the scan.

Read `GLOSSARY.md` and the ADRs in the scan area first. The domain language names the seams, and an ADR settles a decision the survey must not reopen.

## Process

### 1. Scope

Decide where to look before looking. YAGNI: a deepening pays back where the code changes.

- If the owner named a direction, take it.
- Otherwise, walk `git log --name-only --oneline` for the hot spots and start there.
- For the state and invariant pass, the scope is every lifecycle-bearing entity and every invariant in the repository. If the repository is too large for one pass, declare the audited packages and list the rest in the coverage table. Why: "all" and "every" are completeness claims, and a hot-spot sample cannot support them.
- Write down what is out before the first candidate. Why: an unmarked gap turns a sample into a false completeness claim.

### 2. Model

Build the conceptual model before judging the structure. Read [MODEL-AUDIT.md](MODEL-AUDIT.md) now. It owns the lifecycle, invariant, and abstraction procedures, the finding catalog, the scan order, the rank, the evidence rules, and the coverage table.

The model has three inventories:

- **Lifecycles**: every entity that moves through states, including status fields, state enums, boolean clusters, phase timestamps, queues, sessions, and subscriptions. For each, reconstruct states, events, transitions, guards, side effects, and persistence. Mark an edge unknown rather than guessing.
- **Invariants**: every fact the code assumes but does not enforce, across uniqueness, cardinality, ordering, ownership and tenancy, balance and totals, temporal, referential, idempotency, concurrency, and immutability. For each, name where it is assumed, how it is enforced now, what breaks when it fails, and the enforcement point.
- **Abstractions**: the domain concepts the code names, the concepts it is missing, and the concepts it fakes with duplicated or premature abstractions.

The model is the abstract exercise: name the concepts, then test the code against them. A finding without a model is a style opinion.

### 3. Find

Walk the codebase in the scan order in [MODEL-AUDIT.md](MODEL-AUDIT.md): churn and hot spots, state, invariants, seams, slop. Each step narrows the next. Why: a free-form walk finds what the reader happens to notice, and a fixed order finds the same classes of gap on every run.

The finding catalog in [MODEL-AUDIT.md](MODEL-AUDIT.md) names each architectural smell, its detection signal, and its fix shape. Read it before the scan; it decides what becomes a candidate. A finding without evidence at `file:line` and a concrete failure mode stays a note, not a candidate. Why: a pattern match without a failure mode is a style opinion.

Apply the **deletion test** to every suspected shallow module. Deleting it either concentrates complexity behind a smaller interface (deepen) or spreads it across callers (leave it). A candidate that fails the test is not a candidate. Classify a deepening's dependency with the categories in [DEEPENING.md](../codebase-design/DEEPENING.md): in-process, local-substitutable, ports and adapters, or mock.

### 4. Rank and sequence

Rank every candidate on impact, confidence, effort, risk, and dependency. Order by blocking edges first, then by the axes in [MODEL-AUDIT.md](MODEL-AUDIT.md). The top recommendation is the first candidate in the sequence, with the next two candidates named. Why: a menu moves the decision back to the owner, and a rank that ignores a blocking edge produces an order that cannot run.

[MODEL-AUDIT.md](MODEL-AUDIT.md) owns the scale for each axis. Why: the scales are the method, and one source keeps two sessions from ranking the same evidence differently.

### 5. Write the plan

Write two files to the OS temp directory. Resolve the temp dir from `$TMPDIR`, falling back to `/tmp` (`%TEMP%` on Windows), and use one run id:

- `<tmpdir>/architecture-review-<timestamp>.html`: the owner's report. Follow [HTML-REPORT.md](HTML-REPORT.md).
- `<tmpdir>/architecture-review-<timestamp>.json`: the candidate index the mission reads.

The report carries, in this order:

1. **Coverage**: what was audited, what was not, and why. The completeness claim lives here.
2. **State machines**: one card per lifecycle entity, with the implicit states and transitions, the failure modes, the explicit representation to adopt, the illegal states and transitions it removes, and the test that proves it. Use a before and after diagram.
3. **Invariants**: a table with the invariant, where assumed, current enforcement, failure mode, recommended enforcement, interface change, and evidence. Mark `none` where the code only assumes it. Put the plan block below the table for each invariant.
4. **Deepening candidates**: one card per candidate, with evidence, problem, solution, the deletion test, the dependency category, and a before and after diagram.
5. **Sequence and top recommendation**: the full sequence, with the first candidate expanded and the next two candidates named.

Every candidate carries the finding fields for its kind, plus the action block in [MODEL-AUDIT.md](MODEL-AUDIT.md): the files, steps, acceptance test, unslop check, rank, and blocking edges. The action block is the difference between a finding and a ticket. The state, invariant, and deepening sections in [HTML-REPORT.md](HTML-REPORT.md) own the finding fields for each kind. Why: a state machine needs a transition table, an invariant needs an enforcement layer, and a deepening needs a deletion test; one card shape for all three hides the difference.

The candidate index is a JSON object:

```json
{
  "run": { "repo": "", "commit": "", "timestamp": "", "report": "" },
  "coverage": [{ "area": "", "lifecycles": [], "invariants": [], "not_audited": [], "why": "" }],
  "sequence": ["deepening-01"],
  "candidates": [
    {
      "id": "deepening-01",
      "kind": "deepening",
      "title": "",
      "strength": "strong",
      "impact": "high",
      "confidence": "high",
      "effort": "M",
      "risk": "LOW",
      "evidence": ["path/file.ts:123"],
      "interface_change": "",
      "action": {
        "files": ["path/file.ts"],
        "steps": ["..."],
        "acceptance_test": "...",
        "unslop_check": "...",
        "blocked_by": []
      },
      "anchor": "#deepening-01"
    }
  ]
}
```

`kind` is `state`, `invariant`, or `deepening`. `strength` is the recommendation (`strong`, `worth exploring`, `speculative`); `impact` is the payoff (`high`, `med`, `low`); `confidence` is the evidence (`high`, `med`, `low`); `effort` is `S`, `M`, or `L`; `risk` is `LOW`, `MED`, or `HIGH`. `sequence` is the execution order, one entry per candidate, first to last. Each coverage entry carries the same completeness claim as the report table. The index is a summary; the report carries the full finding.

Open the HTML report (`xdg-open`, `open`, or `start`) and tell the owner the absolute path of both files. Then stop; step 6 owns the mode. Do not propose a full interface before the owner picks.

### 6. Compose

Ask whether this is a standalone pass or a mission pass when the invocation does not say.

**Mission**: the planning pass is the first phase of an architecture pass. The plan feeds the mission grill, and the gate approves the top candidate or the sequence. In a change, `implement` builds the approved candidate. In a feature or fog, `to-spec` drafts the spec from the approved plan, `to-tickets` cuts it, and `implement` or `implement-spec` builds the frontier. The planner never creates the work item before the gate. Why: the plan is not approved yet, and a record created before the gate hides the missing decision.

**Standalone**: load `grilling` and walk the chosen candidate: constraints, dependencies, the shape of the deepened module, what sits behind the seam, and which tests survive. Load `codebase-design` and use design-it-twice when the candidate has more than one credible interface shape or two or more callers with different needs. Why: a single guess hardens before the alternatives are compared. Name the deepened module from the glossary; add a missing term through `domain-modeling`. Record a rejected candidate with a lasting reason as an ADR through `domain-modeling`. The plan is ready to take into a mission.

The output is a plan, not a diff. In mission mode, the mission owns the spec, tickets, and fix. In standalone mode, the plan is ready to take into a mission.

## It is working if

- The coverage table names the audited paths and the paths left out.
- Every lifecycle entity in scope has a state card, and every invariant in scope has a row.
- No state or invariant is invented. Unknown edges and unenforced facts are marked.
- Every candidate has evidence at `file:line`, a concrete fix, a plan, and an acceptance test.
- The plan gives one sequence and a top recommendation, not a menu.
- The planner never edits audited source; standalone mode may update `GLOSSARY.md` and ADRs through `domain-modeling`.
- The mission composition is explicit, and the plan names the unslop pass.

When the owner challenges a finding, a recommendation, or a fact, load `pushback` before you reply.
