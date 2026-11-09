---
name: improve-codebase-architecture
description: Survey a codebase for implicit state machines, unenforced invariants, and deepening opportunities, present a visual HTML report with a structured candidate index, then grill the chosen candidate or hand the index to the mission. Use when asked to audit architecture, find state machines or invariants, or run a full architecture pass.
disable-model-invocation: true
---

# Improve Codebase Architecture

Surface architectural friction and propose **deepening opportunities**, after modelling the lifecycles and invariants the code already relies on. The aim is testability, locality, and AI-navigability.

The survey is read-only on source code. It writes two files to the OS temp directory: a single-file HTML report for the owner, and a compact candidate index for the mission. It never edits the audited code. The standalone grill may update `GLOSSARY.md` and ADRs through `domain-modeling`.

Load `codebase-design` with the Skill tool before the first candidate. Use its vocabulary exactly: **module**, **interface**, **implementation**, **depth**, **seam**, **adapter**, **leverage**, **locality**. Why: the terms are the report's precision, and synonyms drift the scan.

Read `GLOSSARY.md` and the ADRs in the scan area first. The domain language names good seams, and an ADR settles a decision the survey must not re-litigate.

## Process

### 1. Scope

Decide where to look before looking. YAGNI: a deepening pays back where the code changes.

- If the owner named a direction, take it.
- Otherwise, walk `git log --oneline` for the hot spots and start there.
- For the state and invariant pass, the scope is every lifecycle-bearing entity and every invariant in the repository. If the repository is too large for one pass, declare the audited packages and list the rest in the coverage table. Why: "all" and "every" are completeness claims, and a hot-spot sample cannot support them.
- Write down what is out before the first candidate. Why: an unmarked gap turns a sample into a false completeness claim.

### 2. Model

Build the conceptual model before judging the structure. Read [MODEL-AUDIT.md](MODEL-AUDIT.md) now. It owns the state-machine, invariant, and abstraction procedures, the evidence rules, and the coverage table.

The model has three inventories:

- **Lifecycles**: every entity that moves through states, including status fields, state enums, boolean clusters, phase timestamps, queues, sessions, and subscriptions. For each, reconstruct states, events, transitions, guards, side effects, and persistence. Mark an edge unknown rather than guessing.
- **Invariants**: every fact the code assumes but does not enforce, across uniqueness, cardinality, ordering, ownership and tenancy, balance and totals, temporal, referential, idempotency, concurrency, and immutability. For each, name where it is assumed, how it is enforced now, what breaks when it fails, and the enforcement point.
- **Abstractions**: the domain concepts the code names, the concepts it is missing, and the concepts it fakes with duplicated or premature abstractions.

The model is the abstract exercise: name the concepts, then test the code against them. A finding without a model is a style opinion.

### 3. Scan for deepening

Walk the codebase and note where the model meets friction. Use the codebase-design principles.

- Where does understanding one concept require bouncing between many small modules?
- Where are modules **shallow**, with an interface nearly as complex as the implementation?
- Where have pure functions been extracted for testability while the real bugs hide in how they are called (no **locality**)?
- Where do tightly-coupled modules leak across their **seams**?
- Which parts are untested, or hard to test through the current interface?
- Which missing abstraction would collapse the most call sites behind one interface?

Apply the **deletion test** to every suspected shallow module. Deleting it concentrates complexity behind a smaller interface (deepen), or spreads it across callers (leave it). A candidate that fails the test is not a candidate. Classify a deepening's dependency with the categories in [DEEPENING.md](../codebase-design/DEEPENING.md): in-process, local-substitutable, ports and adapters, or mock.

### 4. Report

Write two files to the OS temp directory. Resolve the temp dir from `$TMPDIR`, falling back to `/tmp` (`%TEMP%` on Windows), and use one run id:

- `<tmpdir>/architecture-review-<timestamp>.html`: the owner's report. Follow [HTML-REPORT.md](HTML-REPORT.md).
- `<tmpdir>/architecture-review-<timestamp>.json`: the candidate index the mission reads.

The report carries, in this order:

1. **Coverage**: what was audited, what was not, and why. The completeness claim lives here.
2. **State machines**: one card per lifecycle entity, with the implicit states and transitions, the failure modes, the explicit representation to adopt, the illegal states and transitions it removes, and the test that proves it. Use a before/after diagram.
3. **Invariants**: a table with the invariant, where assumed, current enforcement, failure mode, recommended enforcement, interface change, and evidence. Mark `none` where the code only assumes it.
4. **Deepening candidates**: one card per candidate, with evidence, problem, solution, the deletion test, the dependency category, and a before/after diagram.
5. **Top recommendation**: one candidate first, with the sequence after it.

Every candidate gets a one-line **interface change**: what the caller stops knowing. The top candidate gets a before/after interface sketch: entry points, invariants, ordering, error modes, and what sits behind the seam. Do not write a full interface for every candidate. The grill designs the chosen one. Why: the report ranks the work, and an interface written before the decision hardens a guess.

The candidate index is a JSON object:

```json
{
  "run": { "repo": "", "commit": "", "timestamp": "", "report": "" },
  "coverage": [{ "area": "", "lifecycles": [], "invariants": [], "not_audited": [], "why": "" }],
  "candidates": [
    {
      "id": "state-01",
      "kind": "state",
      "title": "",
      "strength": "strong",
      "confidence": "high",
      "effort": "M",
      "risk": "LOW",
      "evidence": ["path/file.ts:123"],
      "interface_change": "",
      "anchor": "#state-01"
    }
  ]
}
```

`kind` is `state`, `invariant`, or `deepening`. `strength` is the recommendation (`strong`, `worth exploring`, `speculative`); `confidence` is the evidence (`high`, `med`, `low`); `effort` is `S`, `M`, or `L`; `risk` is `LOW`, `MED`, or `HIGH`. Each coverage entry carries the same completeness claim as the report table: the lifecycles and invariants audited, the areas not audited, and why. The index is a summary; the report carries the full finding. Keep the index machine-readable; the mission reads it instead of parsing HTML.

Open the HTML report (`xdg-open`, `open`, or `start`) and tell the owner the absolute path of both files. Then stop; step 5 owns the mode. Do not propose a full interface before the owner picks.

### 5. Decide

Ask whether this is a standalone survey or a mission survey when the invocation does not say.

**Standalone**: load `grilling` and walk the chosen candidate: constraints, dependencies, the shape of the deepened module, what sits behind the seam, which tests survive. Load `codebase-design` and use design-it-twice when the candidate has more than one credible interface shape or two or more callers with different needs. Why: a single guess hardens before the alternatives are compared. Name the deepened module from the glossary; add a missing term through `domain-modeling`. Record a rejected candidate with a lasting reason as an ADR through `domain-modeling`.

**Mission**: stop after the report and return the candidate index. The mission's grill owns the decision, and the mission's spec and tickets own the fix. Why: two grills and two specs produce two sources of truth.

The output is a decision, not a diff. In mission mode, the mission owns the spec, tickets, and fix. In standalone mode, the decision is ready to take into a mission.

## It is working if

- The coverage table names the audited paths and the paths left out.
- Every lifecycle entity has a state card, and every invariant in scope has a row.
- No state or invariant is invented. Unknown edges and unenforced facts are marked.
- Every deepening candidate passes the deletion test and has evidence at `file:line`.
- The report gives one top recommendation and a sequence, not a menu.
- The source tree is untouched by the survey. The standalone grill may update `GLOSSARY.md` and ADRs through `domain-modeling`; the mission mode stops after the report and index.
- In mission mode, the mission receives the index and owns the grill, spec, and fix.
