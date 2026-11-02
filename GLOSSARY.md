# Skills

The vocabulary of the mission flow that this repository teaches and follows.

## Language

**Mission**:
A bounded piece of work with a flow, approval gates, and one reviewable PR at the end.
_Avoid_: Job, project

**Mission document**:
The tracker issue that holds a mission's flow, gates, decisions, session plan, and handoff log. It is the only state that survives a session.
_Avoid_: Mission file, plan

**Ticket**:
One unit of work in a mission that ends with a reviewable PR.
_Avoid_: Task, job

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
A decision point where the user approves the named document before the next phase.
_Avoid_: Checkpoint, approval step

**Handoff**:
The prompt that carries the mission state to a new session when context is lost.
_Avoid_: Summary, continuation
