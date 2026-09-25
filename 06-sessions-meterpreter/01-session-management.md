# Session Management

## Objective

Learn how to manage Metasploit sessions as controlled engagement resources rather than treating a session as the end goal.

By the end of this file, you should be able to:

* Understand what a Metasploit session represents.
* List and identify sessions.
* Interact with the correct session.
* Background a session without losing it.
* Switch between multiple sessions.
* Distinguish session creation from session usability.
* Diagnose session instability.
* Manage sessions according to the engagement objective.
* Avoid unnecessary activity.
* Close sessions and clean up safely.

## Why Session Management Matters

Obtaining a session changes the problem.

Before exploitation:

```text id="rj5q6x"
Can I reach the intended target?
```

After exploitation:

```text id="3p8v1m"
What access do I have?
What target is it?
What can I legitimately do with it?
What evidence do I need?
When should I stop?
```

A session is therefore not the objective by itself.

Use:

```text id="w4r8hy"
SESSION
  ↓
IDENTIFY
  ↓
VERIFY
  ↓
MANAGE
  ↓
USE FOR OBJECTIVE
  ↓
DOCUMENT
  ↓
CLEAN UP
```

## What Is a Session?

A Metasploit session represents an established communication channel between Metasploit and a target.

Depending on the module and payload, the session may provide capabilities such as:

```text id="c1s7yb"
Command execution
Meterpreter interaction
Shell access
Other supported session types
```

The exact capabilities depend on:

```text id="9x5jqn"
Target
Payload
Operating system
Architecture
Network path
Session type
Privileges
Stability
```

Therefore:

```text id="4j7n3c"
SESSION ≠ UNIVERSAL ACCESS
```

## Session Lifecycle

A useful model is:

```text id="k7q4fd"
SESSION CREATED
      ↓
IDENTIFY
      ↓
VERIFY
      ↓
INTERACT
      ↓
BACKGROUND
      ↓
REUSE / SWITCH
      ↓
CLOSE
```

A session can also terminate unexpectedly:

```text id="m0s6az"
SESSION CREATED
      ↓
SESSION LOST
      ↓
DIAGNOSE
      ↓
DECIDE WHETHER RETRY IS JUSTIFIED
```

## First Rule — Know Which Session You Have

When sessions exist, do not immediately start interacting with one.

First inspect the available sessions.

A common command is:

```text id="v3m9f1"
sessions
```

The output can help establish:

```text id="q8t2zw"
Session ID
Session type
Target information
Connection information
```

The exact output can vary by Metasploit version and session type.

Your first question should be:

```text id="h2p4cx"
Which session corresponds to my current objective?
```

## Session IDs Matter

Suppose you have:

```text id="e9v1qa"
1   shell
2   meterpreter
3   meterpreter
```

Do not assume:

```text id="4a5m8p"
The newest session is automatically the correct session.
```

Instead determine:

```text id="d3r7ku"
Which target?
Which session type?
Which user context?
Which objective?
```

The session identifier is the reference you use to manage that session.

## Interacting With a Session

Once you identify the appropriate session, interact with it using the supported Metasploit session workflow.

For example:

```text id="n6v3ta"
sessions -i 2
```

This means:

```text id="f0s2ck"
Interact with session 2.
```

Do not memorize the command without understanding the purpose.

The reasoning is:

```text id="j4e9wu"
IDENTIFY SESSION
      ↓
SELECT SESSION
      ↓
INTERACT
```

## Verify Before Doing More

Once inside a session, do not immediately perform extensive actions.

First confirm:

```text id="z7q1px"
Am I on the expected target?
Am I using the expected session?
Is the session responsive?
Does it provide the capability I expected?
```

This prevents a common mistake:

```text id="m5y2da"
Wrong session
      ↓
Wrong assumption
      ↓
Wrong action
```

## Backgrounding a Session

Sometimes you need to leave a session temporarily while keeping it available.

The concept is:

```text id="q2d8hr"
INTERACTIVE SESSION
      ↓
BACKGROUND
      ↓
MSFCONSOLE
      ↓
SESSION REMAINS AVAILABLE
```

In supported interactive sessions, Metasploit provides a mechanism to background the session and return to the console.

The important distinction is:

```text id="1b9z0u"
BACKGROUND
≠
CLOSE
```

Backgrounding preserves the session for later use.

## Why Backgrounding Matters

Imagine:

```text id="v7x4pu"
Session 1 → Web Server
Session 2 → Database Server
Session 3 → Application Server
```

You may need to:

```text id="n4s8lw"
Inspect Session 1
      ↓
Background
      ↓
Inspect Session 2
      ↓
Background
      ↓
Return to Session 1
```

Without session management discipline, multiple sessions quickly become confusing.

## Returning to a Session

After backgrounding, you can select the appropriate session again.

Conceptually:

```text id="7k2wpf"
MSFCONSOLE
   ↓
LIST SESSIONS
   ↓
SELECT SESSION
   ↓
INTERACT
```

Always identify the session before re-entering it.

## Multiple Sessions

Multiple sessions are common during an assessment.

For example:

```text id="b8n4ms"
Session 1
Target: WEB-01
Type: Meterpreter

Session 2
Target: DB-01
Type: Shell

Session 3
Target: APP-01
Type: Meterpreter
```

Treat them as separate engagement resources.

Create a mental map:

```text id="s6f2kd"
Session ID
    ↓
Target
    ↓
Session Type
    ↓
User Context
    ↓
Objective
```

This prevents accidental interaction with the wrong system.

## Session Management Table

During labs, maintain a simple record:

| Session | Target | Type        | Context         | Objective         | Status       |
| ------- | ------ | ----------- | --------------- | ----------------- | ------------ |
| 1       | WEB-01 | Meterpreter | Lab user        | Initial access    | Active       |
| 2       | DB-01  | Shell       | Service account | Validate access   | Active       |
| 3       | APP-01 | Meterpreter | Admin lab user  | Post-exploitation | Backgrounded |

The exact information you record should depend on the engagement.

Do not record unnecessary sensitive information.

## Session State

A session can broadly be thought of as:

```text id="0g6w4m"
ACTIVE
BACKGROUND
UNSTABLE
LOST
CLOSED
```

These states are operationally different.

### Active

You are currently interacting with the session.

### Backgrounded

The session exists but you have returned to the Metasploit console.

### Unstable

The session exists but repeatedly fails or terminates.

### Lost

The communication channel no longer exists.

### Closed

You intentionally terminated the session.

## Session Stability

A newly created session should not automatically be treated as reliable.

Consider:

```text id="r1z8vc"
Session opened
      ↓
Interaction works
      ↓
Session terminates
```

Possible causes include:

```text id="g7v2qx"
Payload behavior
Target process termination
Network instability
Handler configuration
Host controls
Resource limitations
Exploit side effects
```

The correct response is diagnosis, not blind repetition.

## The Session Failure Model

Use:

```text id="2h9xmc"
SESSION LOST
     ↓
Was the target still reachable?
     │
     ├── NO → Investigate network/reachability
     │
     └── YES
           ↓
Was the target process still running?
           │
           ├── NO → Session dependency may have terminated
           │
           └── YES
                 ↓
Is the handler/session configuration correct?
                 │
                 ├── NO → Correct configuration
                 │
                 └── YES
                       ↓
Investigate payload/session compatibility
```

This prevents:

```text id="9w3f2a"
retry
retry
retry
retry
```

without learning anything.

## Session Stability vs Session Existence

These are different questions.

```text id="h5t9pe"
Does a session exist?
```

versus:

```text id="v1d6qm"
Can the session reliably perform the required objective?
```

A session may satisfy the first condition but fail the second.

## Session Capabilities

Before using a session, understand what it provides.

Ask:

```text id="a2k8vn"
What session type is this?
What capabilities does it expose?
What privileges does it have?
What limitations exist?
```

For Meterpreter:

```text id="q9m5bw"
What functionality is available?
```

For a basic shell:

```text id="d7p3ks"
What commands and environment are available?
```

Do not assume that every session provides the same functionality.

## Session Context

A session should be understood in context.

For example:

```text id="u8x2jl"
Target:
WEB-01

Session:
Meterpreter

User:
Low-privileged application account

Objective:
Demonstrate initial access
```

This is very different from:

```text id="q1v6tc"
Target:
WEB-01

Session:
Meterpreter

User:
Administrative lab account

Objective:
Validate privileged access
```

The session type is the same.

The operational meaning is different.

## Session Selection by Objective

Use the objective to determine the session.

Example:

```text id="c4x7nd"
Objective:
Validate access to WEB-01.
```

Select:

```text id="e2s8qa"
Session associated with WEB-01
```

Not:

```text id="j5k9zr"
The session with the highest privileges
```

The most privileged session is not automatically the relevant session.

## Session Handover

A session may become the starting point for another authorized objective.

For example:

```text id="q8m1df"
Initial Access
      ↓
Session
      ↓
Verify Context
      ↓
Post-Exploitation Objective
```

This transition should be deliberate.

Before moving forward, ask:

```text id="r5x9ub"
What is the next authorized objective?
What evidence is required?
What actions are permitted?
```

## Do Not Turn Session Access Into Unlimited Activity

A common beginner pattern is:

```text id="p3y7kg"
I got a session.
```

followed by:

```text id="a8n2vq"
Enumerate everything.
Dump everything.
Search everything.
Move everywhere.
```

That is not a disciplined workflow.

Instead:

```text id="t6w4mx"
SESSION
  ↓
OBJECTIVE
  ↓
MINIMUM NECESSARY ACTION
  ↓
EVIDENCE
  ↓
STOP / NEXT OBJECTIVE
```

## Session Commands as Functions

Do not memorize commands as isolated strings.

Think in functions:

| Function   | Question                           |
| ---------- | ---------------------------------- |
| List       | What sessions exist?               |
| Select     | Which session do I need?           |
| Interact   | What can this session do?          |
| Background | Can I temporarily leave it?        |
| Switch     | Which other session should I use?  |
| Inspect    | What context does it have?         |
| Close      | Should this session be terminated? |

This functional model is more durable than memorizing syntax.

## Practical Exercise 1 — Session Identification

In an authorized lab:

1. Establish more than one session if the lab scenario permits.
2. List the available sessions.
3. Record their IDs.
4. Identify their targets.
5. Identify their types.
6. Determine which session belongs to your current objective.
7. Interact only with that session.

Document:

```text id="d4m8ws"
Session ID:
Target:
Type:
Context:
Objective:
Why this session was selected:
```

### Success Criteria

You can explain why the selected session is relevant without relying on:

```text id="x3q6hp"
"It was the first one listed."
```

## Practical Exercise 2 — Background and Return

Use an authorized lab session.

Practice:

```text id="v9b2ke"
INTERACT
   ↓
VERIFY
   ↓
BACKGROUND
   ↓
LIST SESSIONS
   ↓
RETURN TO SESSION
```

Record:

```text id="w1s7za"
What changed when the session was backgrounded?
Did the session remain available?
Could you return to it?
```

### Success Criteria

You can move between the console and a session without confusing:

```text id="q3n8yf"
backgrounding
```

with:

```text id="h6r1px"
closing
```

## Practical Exercise 3 — Multiple Sessions

Create a lab scenario with multiple sessions.

Build:

```text id="s8d2vj"
Session Map

Session 1 → Target A → Type → Objective
Session 2 → Target B → Type → Objective
Session 3 → Target C → Type → Objective
```

Then:

1. List all sessions.
2. Select one.
3. Verify it.
4. Background it.
5. Select another.
6. Return to the first.

### Success Criteria

You can identify every session without guessing.

## Practical Exercise 4 — Session Loss

Use a lab environment where session instability can be safely observed.

When a session disappears, do not immediately rerun the exploit.

Record:

```text id="n7c4qy"
Last known session:
Target:
What was happening?
When was it lost?
Was the target reachable?
Was the process still present?
What changed?
```

Then determine:

```text id="u2m9fh"
Exploit problem?
Payload problem?
Handler problem?
Network problem?
Target process problem?
Environmental problem?
Unknown?
```

### Success Criteria

Your first response to session loss is diagnosis rather than repetition.

## Practical Exercise 5 — Session-to-Objective Mapping

For each active session, write:

```text id="f6w3sk"
Session:
Target:
Current context:
Authorized objective:
Required evidence:
Next action:
Stop condition:
```

This forces you to treat sessions as engagement resources.

## Common Mistakes

### Mistake 1 — Treating Any Session as the Right Session

Correction:

```text id="k9p3za"
Match the session to the target and objective.
```

### Mistake 2 — Confusing Background With Close

Correction:

```text id="c6v1rm"
Background preserves the session.
Closing terminates it.
```

### Mistake 3 — Ignoring Session Context

Correction:

```text id="t4q8nd"
Verify target and user context before taking further action.
```

### Mistake 4 — Assuming Meterpreter Means Full Control

Correction:

```text id="y7m2px"
Session capability depends on the session, target, context, and privileges.
```

### Mistake 5 — Repeating a Failed Session Attempt

Correction:

```text id="b5r9wk"
Diagnose the failed stage before retrying.
```

### Mistake 6 — Keeping Every Session Forever

Correction:

```text id="p8x4jd"
Manage sessions according to the engagement lifecycle.
```

### Mistake 7 — Performing Post-Exploitation Without an Objective

Correction:

```text id="e3n7va"
Define the next authorized objective before acting.
```

## Professional Session Workflow

A disciplined operator follows:

```text id="h8w3qc"
SESSION CREATED
      ↓
LIST
      ↓
IDENTIFY
      ↓
VERIFY TARGET
      ↓
VERIFY CONTEXT
      ↓
VERIFY CAPABILITY
      ↓
DEFINE OBJECTIVE
      ↓
INTERACT
      ↓
COLLECT MINIMUM EVIDENCE
      ↓
BACKGROUND / SWITCH / CLOSE
      ↓
DOCUMENT
```

The key transition is:

```text id="x6f1mz"
SESSION
  ↓
CONTEXT
  ↓
OBJECTIVE
  ↓
ACTION
```

not:

```text id="d2k7qp"
SESSION
  ↓
RANDOM ENUMERATION
```

## Session Cleanup

At the end of the authorized objective, determine whether the session should remain open.

Ask:

```text id="n4v8ys"
Is the session still needed?
Is additional activity authorized?
Has the evidence been collected?
Could leaving it open create unnecessary risk?
```

If it is no longer required, close it according to the engagement procedure.

The goal is:

```text id="j1r5wb"
MINIMUM REQUIRED ACCESS
```

not:

```text id="m7c3za"
MAXIMUM POSSIBLE ACCESS
```

## Know When to Stop

Stop session activity when:

```text id="u5p9kd"
The objective is satisfied.
```

or:

```text id="s2x7nf"
The required evidence has been collected.
```

or:

```text id="v8m4qc"
Further interaction would exceed the defined scope.
```

or:

```text id="a6r1tz"
The target is becoming unstable.
```

A session is a means to accomplish an authorized objective.

It is not a reason to continue testing indefinitely.

## Completion Checklist

Before moving to Meterpreter-by-objective, confirm that you can:

```text id="m3q8vf"
[ ] Explain what a Metasploit session represents.
[ ] List sessions.
[ ] Identify session IDs.
[ ] Identify session types.
[ ] Match sessions to targets.
[ ] Match sessions to objectives.
[ ] Interact with a selected session.
[ ] Background a session.
[ ] Return to a backgrounded session.
[ ] Work with multiple sessions.
[ ] Track session context.
[ ] Recognize unstable sessions.
[ ] Diagnose session loss systematically.
[ ] Distinguish backgrounding from closing.
[ ] Understand that session type does not equal unlimited access.
[ ] Avoid unnecessary post-exploitation.
[ ] Document relevant session evidence.
[ ] Close sessions when they are no longer required.
[ ] Know when to stop.
```

## Key Mental Model

Remember:

```text id="q4y7ns"
SESSION
  ↓
IDENTIFY
  ↓
VERIFY
  ↓
OBJECTIVE
  ↓
INTERACT
  ↓
EVIDENCE
  ↓
MANAGE
  ↓
CLEAN UP
```

And:

```text id="r9c2wx"
A SESSION IS A CAPABILITY,
NOT THE OBJECTIVE.
```

## Next Step

The next file is:

```text id="f1k6zt"
06-sessions-meterpreter/02-meterpreter-by-objective.md
```

There we will learn **Meterpreter by objective**—how to select and use its capabilities based on a specific authorized task rather than memorizing a giant command dictionary.
