# Test policy

The architecture pass keeps three kinds of behavior test and deletes the rest. The planner inventories the suite and writes the deletions into the plan; `implement` performs them. Why: the planner is read-only, and the owner approves the deletion at the gate.

[DEEPENING.md](../codebase-design/DEEPENING.md) deletes old unit tests on shallow modules once interface tests exist. This policy generalizes that rule to the whole suite. Why: a test outside the three categories is a change-detector wherever it sits.

## The three categories

### Algorithmic logic

A pure computation with no IO: a calculation, parser, validator, reducer, scheduler, or transition table. The expected value comes from an independent source: a fixed literal, a worked example, or the spec. A test that needs a mock of an internal collaborator is not algorithmic logic; it is an implementation-detail test. Rewrite it as one of the three or delete it. Why: a test that mocks an internal collaborator tests the mock, not the logic.

### Boundary IO

One adapter's contract at an edge: an HTTP handler, database repository, file system, queue, third-party client, or serialization. Use the real adapter against a local stand-in (PGlite, an in-memory filesystem, a fake server) or a contract test for the external service. Assert the contract: the request and response shape, the schema, the error mapping. Do not assert the logic behind the adapter. Why: the adapter is where the system meets the outside world, and the contract is the only thing the other side sees.

### Integration flow

A real path through two or more modules or a user journey: checkout, signup, a webhook to the database, a CLI command. Use the real wiring and put the fake at the outermost boundary. Assert the observable outcome, not the internal calls. Why: the flow is the only test that catches a missing side effect between modules.

## The decision tree

1. Does it drive a real path through two or more modules or a user journey? -> integration flow.
2. Does it test one adapter's contract at an edge? -> boundary IO.
3. Does it assert a pure computation with no IO and an independent expected value? -> algorithmic logic.
4. Otherwise -> delete.

When a test fits more than one, the highest level wins. An end-to-end checkout is an integration flow even though it crosses the database boundary. A repository test is boundary IO even though it exercises logic. Why: the highest level is the widest path the test protects, and it subsumes the lower-level check.

When a test is ambiguous, delete it, record the reason, and put it in the deletion list. A kept test needs a category. Why: the policy is a boundary, and a test on the wrong side of it costs every refactor.

## What gets deleted

- Component tests that mount internals and assert props, state, or a snapshot.
- Tests of private methods.
- Tests that mock internal collaborators.
- Tests that assert call counts or call order.
- Tests that reach past the interface to query the database or read a file.
- Tautological tests whose expected value recomputes the implementation.
- Tests of configuration constants, generated code, or the framework.
- A component or UI test that asserts a user-visible contract but crosses no module boundary. Move the assertion to the page-level integration flow.

The last bullet is the line the policy draws. A component test that renders the real component and asserts an accessibility role is still a single-module test. The page-level flow is where the user meets it. Why: a component test mounts internals and breaks on a refactor that changes no behavior.

The policy still tests through an interface at each level: the caller for algorithmic logic, the other system for boundary IO, the user for an integration flow. It does not reach past one. Why: the interface is the contract, and a test that reaches past it breaks on a refactor.

## What stays out of scope

Benchmarks, load tests, security scans, and accessibility audits are not behavior tests. The policy does not delete them. The report names them as out of scope. Why: they catch latency, capacity, exploit, and contrast failures that the three categories do not.

## Inventory

For every test file in the scan area, record:

| File | Test | Category | Keep or delete | Reason | Replacement |
| --- | --- | --- | --- | --- | --- |

Write `none` when no behavior is lost. The replacement names the kept test or the category that covers the behavior.

The coverage table carries the counts by category and the keep and delete totals. The candidate index carries the deletion list. Why: the owner reviews the deletion before the build, and the counts show whether the suite maps the three behaviors.

## The sweep candidate

A suite with many deletions gets one `test` candidate, sequenced first. A small suite carries its deletions in each candidate's steps.

The sweep candidate carries:

- **Evidence**: the inventory and the delete count.
- **Problem**: the suite has N tests outside the three categories. They break on refactors and duplicate coverage.
- **Solution**: delete them. The kept suite covers the three categories.
- **Plan**: the files, the tests, and the replacement for each deleted behavior.
- **Tests deleted**: the file and case list.
- **Acceptance test**: the kept suite passes, and the inventory's delete count is zero.
- **Rank**: impact, confidence, effort, and risk. The risk is the coverage a deletion loses before its replacement exists.
- **Blocked by**: none.

The sweep's acceptance is the empty delete count, not one of the three categories. It is the one candidate that verifies the policy itself. Why: the sweep deletes tests; it does not add a behavior.

## The acceptance test

Every other candidate's acceptance test names its category and the case:

`algorithmic logic: path/to/test.ts: sums the line items`
`boundary IO: path/to/repo.test.ts: rejects a duplicate email`
`integration flow: path/to/checkout.e2e.ts: checkout with a valid cart`

A candidate without a test in one of the three categories is not ready. Why: the policy applies to the tests the plan adds, not only the tests it deletes.

## Why the policy

A test earns its place only when it can fail on a behavior the code cannot check itself: the algorithm's result, the adapter's contract, or the flow's wiring. Every other test restates the implementation. It breaks on a refactor without a behavior change, duplicates one of the three, or verifies a mock. It costs maintenance and hides the seam. The three categories are the whole behavior surface: the logic, the edges, and the paths.
