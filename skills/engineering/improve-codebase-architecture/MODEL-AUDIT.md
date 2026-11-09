# Model audit

How to build the lifecycle, invariant, and abstraction inventories before the deepening scan. This file uses the vocabulary in [SKILL.md](SKILL.md) and [codebase-design](../codebase-design/SKILL.md): **module**, **interface**, **seam**, **adapter**, **invariant**, plus the model terms **state**, **state machine**, **lifecycle**, and **transition**.

## Why the model comes first

A shallow-module scan sees files. A state machine and an invariant are facts about behavior that cross files. Without the model, the scan finds style and misses the bugs: a status field with no transition table, a uniqueness rule enforced only by check-then-insert, a balance updated in two places. The model gives the scan a target.

## 1. Lifecycles: find the state machines

### What counts

A lifecycle-bearing entity has more than one state and a rule about which state can follow which. Look for:

- a `status`, `state`, `phase`, `stage`, or `step` field, column, or enum;
- boolean clusters that encode a state (`isActive`, `isVerified`, `isDeleted`) where some combinations are illegal;
- timestamps that imply phases (`createdAt`, `paidAt`, `shippedAt`, `expiresAt`);
- queues, jobs, sessions, subscriptions, orders, payments, documents, accounts, invitations, and drafts;
- event handlers, reducers, webhooks, and cron jobs that move a record;
- comments and docs that use lifecycle words such as pending, active, expired, or canceled.

### Reconstruct the machine

For each entity, write a table:

| State | Event | Guard | Next state | Side effects | Persistence | Evidence |
| --- | --- | --- | --- | --- | --- | --- |

Then answer:

- What are the states, including the implicit ones such as draft, pending, active, suspended, expired, and deleted?
- Which events move the entity? Which are external (API, webhook, client action) and which are internal (job, timer)?
- Which guards decide the transition, and where are they enforced?
- Which side effects run (email, payment, inventory, audit log), and do they run inside the transition or after it?
- What is the persistence model: one column, several flags, or a value derived from timestamps?
- Which transitions are illegal but currently representable? Which are legal but unreachable?
- What happens on retry, concurrent transition, or out-of-order event?

Mark an edge `unknown` when the code does not decide it. Do not fill the gap with a plausible lifecycle. Why: an invented state is a finding that cannot be checked, and the next session will build on it.

### Render the diagram

Write one Mermaid `stateDiagram-v2` per entity. Show the states and transitions the code supports. Use dashed edges for unknown or unenforced transitions. Keep it small enough to read. Why: a diagram that needs scrolling hides the illegal edge.

### Recommend the explicit form

Choose the smallest representation that makes illegal states unrepresentable:

- **Typed states plus a pure transition function**: the default for in-process logic. Use a union of states, a union of events, and a `transition(state, event)` that returns the next state or an error. Tests exercise the table.
- **A transition table or reducer**: use it when the machine has many states or the product needs to inspect it. Keep the table as data; the reducer walks it.
- **A persisted status column plus a check constraint and a transaction**: use it when the state lives in a database. The constraint rejects unknown states; the transaction makes the transition and its side effects atomic.
- **A workflow engine or statechart library**: use it only when the machine has parallel regions, history, or long-running timers. A library is a cost; do not reach for it for a four-state flow.

Every recommendation names the illegal states it removes, the illegal transitions it rejects, and the test that proves it. Why: a recommendation without a removed state and a test is a preference.

## 2. Invariants: find the facts the code assumes

### What counts

An invariant is a fact that must stay true across every operation. The common families:

- **Uniqueness**: one email per account, one active subscription per customer, one draft per author.
- **Cardinality**: an order has at least one line; a team has at most one lead.
- **Ordering**: payment before shipment; a webhook after its event; a step after its prerequisite.
- **Ownership and tenancy**: a record belongs to one tenant; only its owner can mutate it.
- **Balance and totals**: the sum of line items equals the order total; the ledger balances; the counter matches the rows.
- **Temporal**: an expiry is after its creation; a booking does not overlap another; a retry happens after the timeout.
- **Referential**: a foreign key points to a live row; a deleted parent has no live children.
- **Idempotency**: the same request applied twice has one effect.
- **Concurrency**: two writers cannot both win; the read-modify-write is atomic.
- **Immutability**: a settled record does not change; an audit row is append-only.

### Inventory the gap

For each invariant, add a row:

| Invariant | Where assumed | Current enforcement | Failure mode | Recommended enforcement | Interface change | Evidence |
| --- | --- | --- | --- | --- | --- | --- |

`Current enforcement` is one of: type, schema, database constraint, transaction, runtime guard, test, documentation, none. Record `schema` at the API edge when validation runs there. `Failure mode` is the concrete corruption or wrong result, not "data inconsistency". `Recommended enforcement` names the smallest layer that can reject the violation. `Interface change` is what the caller stops knowing once the rule sits behind the seam.

The layers, in order of preference:

1. **Type**: make the illegal value unrepresentable. Use a branded ID, a non-empty list, or a discriminated union.
2. **Schema**: validate at the boundary. Use Zod, JSON Schema, or a parser at the API edge.
3. **Database constraint**: use a unique index, foreign key, check constraint, or exclusion constraint. The database is the last writer; a rule enforced only in the app loses under concurrency.
4. **Transaction**: make the read-modify-write atomic; the invariant and its side effects commit together.
5. **Runtime guard**: assert at the point of mutation when the type and schema cannot express the rule.
6. **Test**: write a property or integration test that proves the rule holds across operations. A test documents and detects; it does not prevent production writes.

Prefer the earliest layer that can express the rule. A rule enforced only in the UI is a `none` finding; the UI is not an enforcement layer. Why: the next client, job, or webhook bypasses the UI.

### Mark the unknowns

If the code does not show the enforcement, write `none` and say what the code assumes. If the invariant is documented but not enforced, write `documentation`. If an ADR says the gap is deliberate, cite the ADR and report the tradeoff, not a violation. Do not invent an invariant the code does not rely on. Why: an unmarked assumption reads as enforced.

## 3. Abstractions: find the missing and the premature

An abstraction is a named concept with an interface. The model asks three questions:

- **Missing**: which concept appears in several modules under different names, so one change touches all of them in lockstep? The fix is one module with one interface. This is a deepening candidate.
- **Premature**: which interface has one implementation and no second caller, so it adds indirection without leverage? The fix is to inline it until a second adapter or caller exists. Report it as a deepening candidate whose solution is deletion: collapse the interface and inline the one implementation.
- **Leaking**: which concept's rules live outside its module, so callers must know the invariants? The fix is to move the rules behind the seam and make the interface carry them.

Use the domain glossary to name the concept. If the code has no name for it, that is the missing abstraction. If two glossary terms map to one module, either the glossary is wrong or the module is two modules; resolve the term before the candidate. If one glossary term maps to three modules, either the term is too broad or the module has three shallow faces; resolve the term first.

## Coverage table

End the model pass with a table the report can carry:

| Area | Lifecycles audited | Invariants audited | Not audited | Why |
| --- | --- | --- | --- | --- |

The table is the completeness claim. A repository too large for one pass lists the audited packages and the rest. Why: "all" is a claim about the scope, and an unmarked gap turns a sample into a false promise.

## Finding format

Every state, invariant, and abstraction finding comes back in this shape:

### [KIND-NN] Short imperative title

- **Evidence**: `path/file.ts:123`, repeated for the strongest locations.
- **Failure mode**: the concrete corruption, stuck state, or wrong result.
- **Current enforcement**: type, schema, database constraint, transaction, runtime guard, test, documentation, or none.
- **Recommended form**: the explicit states or the enforcement layer.
- **Interface change**: what the caller stops knowing.
- **Strength**: `strong` (the evidence is real and the failure mode is consequential; for a deepening, the deletion test passes), `worth exploring` (plausible, and the payoff depends on the direction), or `speculative` (weak evidence).
- **Confidence**: high (read the code), med (strong signal), low (needs investigation).
- **Fix effort**: S (hours), M (a day), L (multi-day), including tests.
- **Fix risk**: LOW, MED, or HIGH, with one line why.

The report carries this full format. The candidate index carries the id, kind, title, strength, confidence, effort, risk, evidence, interface change, and anchor.
