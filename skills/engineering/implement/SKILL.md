---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

## Reporting

Before each step, say what you are about to do, why now, why this way, and what
it unlocks. After it, say what changed and what the next step can now do. Scale
the detail to the step; never drop the why. Inside a mission, use the mission
output contract.

When the ticket, the spec, and the code do not say why a step is needed, stop
and ask. Record the answer in the ticket or the mission document. Never invent
a reason.

## Seams and checks

Call the Skill tool with "tdd" where possible, at pre-agreed seams. When the
work has no testable seam, say so and skip it.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, call the Skill tool with "code-review" to review the work.

Commit your work to the current branch.
