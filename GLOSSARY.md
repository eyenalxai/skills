# Skills

The vocabulary of the mission flow that this repository teaches and follows.

## Language

**Mission**:
A bounded piece of work with a flow, approval gates, and one reviewable PR per ticket at the end.
_Avoid_: Job, project

**Mission document**:
The work item issue that holds a mission's current state: frontier, concurrency cap, flow, gates, decisions, session plan, and outputs. It is created or claimed after the scope gate and updated only at a boundary. It carries no history and no handoff log.
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
The `implement-spec` session that claims a frontier and runs one worker subagent
per ticket. It reviews the PRs, reports the stack bottom-up, and stops.
_Avoid_: Orchestrator, manager

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

**Handoff**:
The prompt that carries the mission state to a new session when context is lost.
It carries state, not authority; it reports a gate and never grants it.
_Avoid_: Summary, continuation
