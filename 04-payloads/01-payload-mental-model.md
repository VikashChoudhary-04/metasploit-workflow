# Payload Mental Model

## Objective

Understand what a Metasploit payload is, how it relates to an exploit, and how payload characteristics affect the result of an exploitation attempt.

By the end of this file, you should be able to:

* Explain the difference between an exploit and a payload.
* Understand where a payload fits into the exploitation chain.
* Distinguish command shells from Meterpreter sessions.
* Understand staged and stageless payloads.
* Distinguish reverse and bind connection models.
* Understand why payload compatibility matters.
* Identify the important payload dimensions.
* Explain why a successful exploit does not automatically produce a usable session.
* Choose payloads based on objective and target constraints rather than familiarity.

## Why Payloads Matter

A common beginner workflow is:

```text id="d7m2q8"
Find exploit
    ↓
Choose familiar payload
    ↓
Run
```

This is incomplete.

A more accurate model is:

```text id="p4x8n1"
TARGET
   ↓
EXPLOIT
   ↓
PAYLOAD
   ↓
EXECUTION / CONNECTION
   ↓
SESSION OR RESULT
```

The exploit and payload solve different problems.

Understanding that distinction is essential for troubleshooting.

## Exploit vs Payload

Think of an exploit as the mechanism used to trigger a vulnerable condition.

Think of a payload as the code or behavior intended to execute after the exploitation mechanism succeeds.

Conceptually:

```text id="m8q2v6"
EXPLOIT
"How do I trigger the vulnerable condition?"
```

and:

```text id="c5r9k3"
PAYLOAD
"What should happen after successful exploitation?"
```

This means:

```text id="j1v7p4"
Exploit success
≠
Payload success
```

and:

```text id="s6n3x8"
Payload success
≠
Usable session automatically
```

Several separate stages can fail.

## The Exploitation Chain

Use this model:

```text id="w2q7m5"
Target condition
      ↓
Exploit executes
      ↓
Payload executes
      ↓
Payload performs intended behavior
      ↓
Connection or local result occurs
      ↓
Session may be created
      ↓
Operator verifies result
```

Failure at any stage can produce a different symptom.

This is why payload knowledge is important for troubleshooting.

## A Practical Example

Suppose you have an authorized lab target.

Your exploit module is appropriate.

You execute it.

The module reports behavior consistent with exploitation, but no session appears.

Do not immediately conclude:

```text id="r9m4c1"
The exploit did not work.
```

Possible explanations include:

```text id="f6x2p8"
Exploit failed
```

or:

```text id="k3n7v5"
Payload was incompatible
```

or:

```text id="q8c1m6"
Payload executed but could not establish its connection
```

or:

```text id="t5r9d2"
Callback configuration was wrong
```

or:

```text id="v1m4x7"
Session was created and immediately died
```

Separating these possibilities makes troubleshooting much more systematic.

## Payload Selection Is a Separate Decision

A strong workflow is:

```text id="y7p3q9"
SELECT EXPLOIT
       ↓
UNDERSTAND TARGET
       ↓
SELECT COMPATIBLE PAYLOAD
       ↓
CONFIGURE
       ↓
VALIDATE
       ↓
EXECUTE
```

Do not treat payload selection as an afterthought.

The payload must fit:

```text id="n2c8m5"
Target
+
Exploit
+
Platform
+
Architecture
+
Connection model
+
Network path
+
Objective
```

## Payload Characteristics

A payload can be described using several dimensions.

Important dimensions include:

```text id="x4v8k1"
Execution behavior
Platform
Architecture
Connection direction
Transport
Staged / stageless design
Session type
Network requirements
Target compatibility
```

You do not need to memorize every payload.

You need to understand these dimensions well enough to select an appropriate one.

## Command Shell Payloads

A command shell provides command-line interaction with the target when the payload and target environment support it.

Conceptually:

```text id="q6m2r9"
Payload
   ↓
Command interpreter
   ↓
Operator interaction
```

The exact shell depends on the target platform.

A shell can be enough for some objectives.

For example:

```text id="c8p5n3"
Objective:
Run a small number of authorized commands
to verify controlled access.
```

You may not need a more feature-rich session.

## Meterpreter Payloads

Meterpreter is a Metasploit payload/session technology designed to provide a richer interactive session than a basic command shell.

It can support capabilities such as:

* Session interaction
* File operations
* Process interaction
* System information gathering
* Additional post-exploitation functionality

The exact capabilities depend on the platform, session type, and available functionality.

The important distinction is:

```text id="m3x7q1"
Command shell
=
basic command-line interaction
```

while:

```text id="h9v2c6"
Meterpreter
=
Metasploit-oriented interactive session with additional capabilities
```

Neither should be treated as automatically "better."

The correct choice depends on the objective.

## Choosing a Session Type

Ask:

```text id="p1r6m8"
What do I actually need to accomplish?
```

If the objective is:

```text id="t4c9x2"
Verify basic code execution
```

a simple shell may be sufficient.

If the objective is:

```text id="v7n3k5"
Perform authorized post-exploitation tasks
```

a Meterpreter session may provide more appropriate functionality.

Do not choose a complex payload merely because it has more features.

## Staged Payloads

A staged payload separates the delivery process into multiple parts.

Conceptually:

```text id="b8q2m4"
Initial component
      ↓
Establishes communication
      ↓
Additional payload transferred
      ↓
Final session/functionality
```

This can be useful because the initial stage and larger payload are handled separately.

The exact behavior depends on the payload and transport.

## Stageless Payloads

A stageless payload contains the required payload functionality together rather than depending on a separate second-stage retrieval.

Conceptually:

```text id="j5x8r1"
Single payload
      ↓
Execution
      ↓
Session / result
```

The key distinction is:

```text id="n6c3p9"
Staged
=
initial stage + later payload
```

```text id="q4m7v2"
Stageless
=
payload functionality delivered together
```

## Why Staged vs Stageless Matters

This is not merely a naming difference.

The delivery model affects:

* Network behavior
* Payload size
* Reliability
* Handler behavior
* Troubleshooting
* Whether an additional stage can be retrieved

Therefore, if a staged payload fails, possible causes include:

```text id="w3p8k6"
Initial execution failed
```

or:

```text id="r1m5c9"
Initial connection succeeded
but second-stage retrieval failed
```

These are different failure conditions.

## Reverse Payloads

A reverse payload causes the target to initiate a connection back toward the operator-controlled listener.

Conceptually:

```text id="k7v2n4"
TARGET
  │
  │ outbound connection
  ▼
OPERATOR LISTENER
```

This is commonly used when the target can make outbound connections but the operator cannot directly connect to the target in the required way.

However, it introduces callback requirements.

You must consider:

```text id="c5x9p1"
Callback address
Callback port
Routing
Firewall rules
NAT
Network reachability
Listener configuration
```

## Bind Payloads

A bind payload causes the target to listen for an incoming connection.

Conceptually:

```text id="m8q4r7"
OPERATOR
   │
   │ inbound connection
   ▼
TARGET LISTENER
```

This can be useful in environments where the operator can reach the target directly and the target can accept the connection.

It can fail when:

```text id="p2n6v8"
Firewall blocks inbound traffic
```

or:

```text id="d7c1m5"
Network segmentation prevents access
```

or:

```text id="f4r9x3"
The target is not reachable from the operator
```

## Reverse vs Bind

The distinction is:

```text id="x6q3m1"
Reverse:
Target → Operator
```

```text id="v9k5p2"
Bind:
Operator → Target
```

This should be treated as a network design decision.

Do not select one because it is the payload you usually use.

Ask:

```text id="j2r7c4"
Which direction can actually work in this network?
```

## Network Reality

A payload does not operate in isolation.

Consider:

```text id="s8n3q6"
Exploit
  ↓
Payload
  ↓
Network path
  ↓
Listener
  ↓
Session
```

The payload can be technically correct while the network path is impossible.

For example:

```text id="a5m9v2"
Target
    X
    │
    │ outbound connection blocked
    ▼
Operator
```

The vulnerability may still exist.

The failure is the communication path.

## LHOST and LPORT as Concepts

Reverse payloads commonly require information describing where the target should connect.

Conceptually:

```text id="h4p8x1"
LHOST
=
local/listener-side address used for the connection
```

```text id="q7m2c5"
LPORT
=
local/listener-side port used for the connection
```

The exact correct values depend on the network.

Do not interpret `LHOST` as:

```text id="z3v6n9"
always my computer's most obvious IP
```

Instead ask:

```text id="w1r4k8"
Which local address is reachable by the target
through the intended network path?
```

## Why Callback Address Errors Are Common

Suppose your machine has:

```text id="p6c2m7"
Interface A
Interface B
Interface C
```

Only one may be reachable from the target.

Choosing the wrong address can result in:

```text id="n8q4v1"
Exploit appears to execute
but no session arrives.
```

Therefore payload configuration requires network understanding.

## Platform Compatibility

Payloads are not universally compatible.

A payload intended for one platform may not be appropriate for another.

For example:

```text id="r5x7m2"
Windows payload
```

is not automatically appropriate for:

```text id="k1p9c4"
Linux target
```

Always verify platform compatibility.

## Architecture Compatibility

Architecture can also matter.

Examples:

```text id="v3n8q6"
x86
x64
ARM
```

A payload must be compatible with the target environment and the exploit's execution context.

Do not assume:

```text id="d9m2r5"
Exploit works
=
every payload works
```

Exploit compatibility and payload compatibility are separate checks.

## Transport

Payloads may use different network transports.

Conceptually:

```text id="q4x7n1"
Payload
  ↓
Transport
  ↓
Communication
```

Transport characteristics can affect:

* Network requirements
* Listener configuration
* Connectivity
* Reliability
* Detection
* Troubleshooting

You do not need to memorize every transport for the current stage.

Understand that the transport is another compatibility dimension.

## Session Type

The payload can influence what type of session is produced.

Conceptually:

```text id="j8c3m6"
Payload
   ↓
Session type
   ↓
Available capabilities
```

Possible outcomes include:

```text id="p5n9v2"
Command shell
Meterpreter session
Other supported session type
```

The correct session depends on the objective.

## Payload Selection as a Constraint Problem

Instead of asking:

```text id="m7r2c5"
Which payload do I like?
```

ask:

```text id="x4q8n1"
Which payload satisfies all constraints?
```

Those constraints may include:

```text id="s6p3v9"
Exploit compatibility
Target platform
Architecture
Connection direction
Network path
Transport
Session requirements
Engagement objective
```

Conceptually:

```text id="f9k2m7"
EXPLOIT
   +
TARGET
   +
NETWORK
   +
OBJECTIVE
   ↓
PAYLOAD SELECTION
```

## Payload Selection Example

Suppose:

```text id="r3v8c1"
Target:
Authorized lab Windows host

Exploit:
Compatible with target

Network:
Target can reach operator
through a specific interface

Objective:
Obtain a controlled interactive session
```

Your reasoning becomes:

```text id="w7m4p2"
Target platform
      ↓
Compatible payload family
      ↓
Choose connection model
      ↓
Verify callback reachability
      ↓
Choose appropriate session type
      ↓
Configure
      ↓
Validate
```

Notice that the payload name is the final decision, not the first.

## Payload Names Encode Information

Metasploit payload names often contain useful structural information.

A payload path can communicate information about:

```text id="k5n9x2"
Platform
Architecture
Connection model
Staging model
Payload family
```

You should learn to read the structure rather than memorize individual strings.

Conceptually, a payload may communicate:

```text id="p8c3m6"
platform / architecture / connection / payload
```

The exact naming structure varies by payload family.

Use:

```text id="j2r7v4"
show payloads
```

and module information to inspect what is actually supported.

## Why "Favorite Payload" Thinking Is Dangerous

A common habit is:

```text id="m6q1x8"
"I always use this payload."
```

This fails because environments change.

For example:

```text id="z9v4p2"
Network A
supports reverse connection
```

while:

```text id="c7n3m5"
Network B
blocks the required outbound path
```

The same payload cannot be assumed to work in both environments.

Use environmental evidence.

## Payload Selection Workflow

Use:

```text id="x1p8m4"
1. Identify exploit.
2. Determine target platform.
3. Determine architecture where relevant.
4. Identify supported payloads.
5. Determine network connectivity.
6. Choose reverse or bind based on network reality.
7. Determine staged/stageless suitability.
8. Determine required session type.
9. Configure callback/listener settings.
10. Verify configuration.
11. Execute in the authorized environment.
12. Verify the resulting session.
```

## Payload Failure Does Not Automatically Mean Exploit Failure

This distinction deserves emphasis.

Suppose:

```text id="q6r2n8"
Exploit
   ↓
Target behavior changes
   ↓
No session
```

Possible explanations include:

```text id="v4m7c1"
Exploit failed.
```

```text id="n9x3p5"
Payload failed.
```

```text id="j2k8s6"
Callback failed.
```

```text id="d5q1r7"
Session died.
```

You need evidence to distinguish them.

This is why later session and troubleshooting sections will build on this model.

## Practical Exercise 1 — Exploit vs Payload

### Objective

Make the distinction operational.

Write an explanation for:

```text id="c8m4p1"
Exploit:
...

Payload:
...

Session:
...
```

Then explain:

```text id="r7n2v6"
What could happen if the exploit succeeds
but the payload fails?
```

Do not answer with:

```text id="x1q9k3"
"Nothing."
```

Describe the possible observable outcomes.

## Practical Exercise 2 — Payload Dimensions

Choose an appropriate module in your authorized lab and inspect:

```text id="m5p8r2"
show payloads
```

For several compatible payloads, identify:

```text id="q3v7n1"
Platform:
Architecture:
Connection direction:
Staged/stageless:
Session type:
Network assumptions:
```

Do not execute them.

The objective is to learn to read payload characteristics.

## Practical Exercise 3 — Reverse vs Bind

### Scenario

Your authorized lab has this network behavior:

```text id="h8c2m5"
Operator → Target
reachable

Target → Operator
reachable
```

Determine what additional information you need before choosing between reverse and bind.

Then create:

```text id="n4x7p1"
Option:
Network direction:
Required listener:
Required route:
Firewall consideration:
Why suitable:
```

The goal is not to memorize a preferred payload.

The goal is to understand network direction.

## Practical Exercise 4 — No Session

### Scenario

You run an authorized lab exploit.

The module reports apparent successful exploitation, but no session appears.

Build a troubleshooting tree:

```text id="v2m9c6"
No session
   ↓
Did exploitation occur?
   ↓
Was payload compatible?
   ↓
Did payload execute?
   ↓
Was callback/bind path reachable?
   ↓
Was listener configured correctly?
   ↓
Was session created?
   ↓
Did session immediately terminate?
```

Document what evidence would answer each question.

## Practical Exercise 5 — Objective-Driven Payload Selection

### Scenario A

```text id="s7p3m8"
Objective:
Verify basic command execution.
```

Question:

```text id="c5n1q9"
What level of session functionality is actually necessary?
```

### Scenario B

```text id="r4v8k2"
Objective:
Perform authorized post-exploitation enumeration.
```

Question:

```text id="m6x2p7"
What session capabilities may be useful?
```

The objective is to prevent feature-driven payload selection.

## Practical Exercise 6 — Configuration Worksheet

For one payload in your authorized lab, document:

```text id="j9c4n1"
Exploit:
...

Target platform:
...

Architecture:
...

Payload:
...

Staged or stageless:
...

Connection direction:
...

Transport:
...

Local callback/listener address:
...

Port:
...

Why this configuration:
...

Network evidence:
...

Expected session:
...

Verification method:
...
```

Do not execute until you can explain every field.

## Common Mistakes

### Mistake 1 — Treating Exploit and Payload as the Same Thing

Correction:

```text id="p7m3x8"
Exploit:
triggers the vulnerable condition

Payload:
defines the resulting behavior
```

### Mistake 2 — Always Using the Same Payload

Correction:

```text id="q4n8c2"
Select based on target and network constraints.
```

### Mistake 3 — Ignoring Architecture

Correction:

```text id="v6r1m9"
Verify compatibility where relevant.
```

### Mistake 4 — Ignoring Network Direction

Correction:

```text id="k2p7x5"
Determine whether target → operator
or operator → target connectivity is feasible.
```

### Mistake 5 — Assuming No Session Means No Exploit

Correction:

```text id="d8m4q1"
Separate exploitation,
payload execution,
connection,
and session creation.
```

### Mistake 6 — Choosing the Most Feature-Rich Payload Automatically

Correction:

```text id="n5c9v3"
Use the minimum functionality required by the objective.
```

### Mistake 7 — Ignoring Staging

Correction:

```text id="r2x7m6"
Understand whether the payload requires
additional stage delivery.
```

## Professional Payload Decision Tree

Use:

```text id="w8p3n1"
What is my objective?
        ↓
What exploit am I using?
        ↓
What platform is the target?
        ↓
What architecture matters?
        ↓
What payloads does the exploit support?
        ↓
What session type do I need?
        ↓
What network path is available?
        ↓
Reverse or bind?
        ↓
Staged or stageless?
        ↓
Configure
        ↓
Verify
        ↓
Execute
        ↓
Verify session/result
```

## Payload Troubleshooting Tree

When a session does not appear:

```text id="f3m9q2"
NO SESSION
    ↓
Was the exploit actually triggered?
    │
    ├── NO → Troubleshoot exploit applicability
    │
    └── YES
          ↓
     Did payload execute?
          │
          ├── NO → Check compatibility/execution
          │
          └── YES
                ↓
       Can the connection complete?
                │
                ├── NO → Check network/callback/listener
                │
                └── YES
                      ↓
               Was a session created?
                      │
                      ├── NO → Investigate payload/session behavior
                      │
                      └── YES
                            ↓
                      Verify session
```

This model will be used later in the troubleshooting section.

## Industry Considerations

In an authorized engagement, payload selection should consider more than technical compatibility.

Consider:

```text id="y6q2r8"
Scope
Objective
Network architecture
Operational risk
Detection expectations
Stability
Required evidence
Cleanup requirements
```

For example, if the objective can be satisfied through a controlled validation method, obtaining a feature-rich interactive session may create unnecessary risk.

The payload should serve the engagement objective.

The engagement should not be shaped around the payload.

## Know When to Stop

Stop or reconsider when:

```text id="t4m8p1"
No payload satisfies the target constraints.
```

or:

```text id="c7n2v5"
The network path required by the payload is unavailable.
```

or:

```text id="j9r3x6"
The payload requires functionality outside the authorized scope.
```

or:

```text id="q5m1k8"
The objective can already be satisfied without obtaining a session.
```

or:

```text id="v2p7n4"
Repeated payload changes are producing no new information.
```

At that point:

```text id="x8c4m6"
diagnose
```

or:

```text id="n3r9q1"
change approach
```

rather than blindly cycling through payloads.

## Completion Checklist

Before moving to payload selection and handlers, confirm that you can:

```text id="k7m2p9"
[ ] Explain exploit vs payload.
[ ] Explain where the payload fits in the exploitation chain.
[ ] Distinguish command shell and Meterpreter sessions.
[ ] Explain staged vs stageless payloads.
[ ] Explain reverse vs bind payloads.
[ ] Understand callback direction.
[ ] Understand the conceptual role of LHOST/LPORT.
[ ] Identify platform compatibility requirements.
[ ] Identify architecture requirements.
[ ] Understand that payload transport matters.
[ ] Choose a payload based on the objective.
[ ] Explain why a successful exploit may produce no session.
[ ] Separate exploit failure from payload failure.
[ ] Separate payload failure from network/callback failure.
[ ] Know when not to keep changing payloads.
```

## Key Mental Model

Remember:

```text id="p1x8m4"
EXPLOIT
  ↓
Triggers vulnerability
  ↓
PAYLOAD
  ↓
Defines resulting behavior
  ↓
NETWORK / EXECUTION
  ↓
SESSION OR RESULT
  ↓
VERIFY
```

And:

```text id="q6n3v9"
Payload choice
=
compatibility decision
```

not:

```text id="w4m7c2"
Payload choice
=
personal preference
```

## Section Progress

You now have the foundation required to understand payloads operationally:

```text id="r8c2m5"
Exploit
+
Payload
+
Target
+
Network
+
Objective
=
Complete exploitation design
```

The next file is:

```text id="z5p1n7"
04-payloads/02-payload-selection-and-handlers.md
```

That file will turn this mental model into a practical workflow for **selecting compatible payloads, configuring handlers, understanding callback problems, and troubleshooting session creation**.
