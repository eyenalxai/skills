# Skills

The vocabulary of the mission flow that this repository teaches and follows.

## Language

**Mission**:
A bounded piece of work with a flow, approval gates, and one reviewable PR per ticket at the end.
_Avoid_: Job, project

**Mission document**:
The work item issue that holds a mission's current state: frontier, concurrency cap, flow, gates, decisions, session plan, and outputs. It is created or claimed after the first gate; a fog map is created by `wayfinder` before its decision gates and becomes the record. It is updated only at a boundary. It carries no history and no handoff log.
_Avoid_: Mission issue, mission file, plan

**Record**:
The short form of mission document. It is the same work item issue, not a second artifact.
_Avoid_: Mission issue, mission file

**Owner**:
The person whose words approve a gate and whose merge accepts a PR. The owner is not the agent, the tracker, or the handoff prompt.
_Avoid_: User, maintainer, stakeholder

**Ticket**:
One unit of work in a mission that ends with a reviewable PR.
_Avoid_: Task, job

**Frontier**:
The set of tickets whose prerequisites are satisfied and that no one has claimed.
The wayfinder frontier is decision tickets; the implementation frontier is build
tickets. The crew chief runs the implementation frontier, up to the mission's
cap. The grill's open questions are the question frontier, a separate set.
_Avoid_: Queue, backlog

**Wave**:
The frontier tickets a session runs together, up to the cap. A wave may be a
stack of dependent tickets.
_Avoid_: Batch, sprint

**Stack**:
A chain of ticket PRs where each PR's base is the blocker's branch, so a
dependent ticket can start before its blocker merges. The owner merges bottom-up.
_Avoid_: Chain, queue

**Crew chief**:
The session that runs `implement-spec`, claims a frontier, and runs one worker
subagent per ticket. By default it is the mission session. It reviews the PRs,
reports the stack bottom-up, and stops.
_Avoid_: Orchestrator, manager

**Worker**:
A subagent that implements one ticket inside the current mission session. It gets
its ticket brief and context pointers, runs in its own checkout, and reports
back. A worker is not a handoff.
_Avoid_: Child session, handoff

**Tracking key**:
A ticket id, mission id, or issue key that identifies a tracker record. It is for the agent and the tracker, not for user-facing prose.
_Avoid_: Ticket name, issue number

**Plain name**:
A short description of the work in the user's language, used in replies and handoffs instead of a tracking key.
_Avoid_: Id, key

**Plain words**:
The style of naming work by what it does, not by a tracking key. A reply uses a plain name in plain words.
_Avoid_: Ticket language, tracker language

**Progress trail**:
The phase-by-phase state of a mission. It lives in the mission document and appears in a handoff prompt, not in every reply.
_Avoid_: Status header, context block

**Gate**:
A decision point where the owner approves the named document with their own
words. A record, a handoff prompt, a tracker status, or a program cannot approve
it.
_Avoid_: Checkpoint, approval step

**First gate**:
The gate after which the mission record is created: the scope and understanding
gate on a change, bug, or trivial, and the scope and spec gate on a feature. A fog
map is created before its decision gates.
_Avoid_: Initial approval, pre-gate

**Decision gate**:
A wayfinder gate that resolves one decision ticket before the spec on a fog path.
The map's decision tickets are the gates.
_Avoid_: Map approval, fog gate

**Scope and understanding gate**:
The gate after the grill. On a change, bug, or trivial it approves the problem,
the rule changes, and the file scope. On a feature or fog its decision is part of
the scope and spec gate.
_Avoid_: Scope gate (alone)

**Scope and spec gate**:
The single gate that approves the scope and the written spec on a feature or fog
path. The spec is the gate document.
_Avoid_: Spec gate, scope gate (when naming the combined gate)

**Handoff**:
The prompt that carries the mission state to a new session when the work travels
or the context is lost. A phase change in the same context is not a handoff. It
carries state, not authority; it reports a gate and never grants it.
_Avoid_: Summary, continuation
