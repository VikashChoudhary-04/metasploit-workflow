# Payload Selection and Handlers

## Objective

Learn how to select, configure, and troubleshoot Metasploit payloads and handlers in an authorized lab environment.

By the end of this file, you should be able to:

* Select a payload based on target and objective.
* Inspect payload compatibility before execution.
* Understand the role of a handler.
* Configure reverse-connection payloads correctly.
* Understand the relationship between `LHOST`, `LPORT`, and the listener.
* Distinguish exploit configuration from payload configuration.
* Understand automatic versus manually configured handlers.
* Recognize common handler and callback failures.
* Diagnose "exploit succeeded but no session" situations.
* Avoid randomly cycling through payloads.
* Decide when a payload is inappropriate for the environment.

## The Payload Workflow

The complete workflow is:

```text id="p7m2c8"
EXPLOIT
   ↓
SUPPORTED PAYLOADS
   ↓
FILTER BY COMPATIBILITY
   ↓
CHOOSE SESSION TYPE
   ↓
CHOOSE CONNECTION MODEL
   ↓
CONFIGURE PAYLOAD
   ↓
CONFIGURE HANDLER
   ↓
VERIFY NETWORK PATH
   ↓
EXECUTE
   ↓
VERIFY SESSION
```

When something fails:

```text id="x4n8q1"
NO SESSION
   ↓
DETERMINE WHERE THE CHAIN FAILED
```

Do not immediately select another payload.

## Payload Selection Starts With the Exploit

Payload selection is constrained by the exploit module.

Start with:

```text id="m5c9r3"
use <exploit-module>
```

Then inspect available payloads when appropriate:

```text id="v8q2k6"
show payloads
```

This gives you the payloads supported by the selected exploit module.

Do not assume that every Metasploit payload works with every exploit.

## Supported Does Not Mean Best

Suppose:

```text id="j3p7m1"
show payloads
```

returns:

```text id="a8c2x5"
Payload A
Payload B
Payload C
Payload D
```

All four may technically be supported.

That does not mean all four are equally appropriate.

Evaluate:

```text id="r6n1v4"
Target platform
Architecture
Session requirements
Connection direction
Network path
Staging model
Operational objective
```

## The Selection Filter

Use this filter:

```text id="q9m3k7"
SUPPORTED BY EXPLOIT?
        ↓
TARGET PLATFORM?
        ↓
ARCHITECTURE?
        ↓
SESSION TYPE?
        ↓
NETWORK PATH?
        ↓
REVERSE OR BIND?
        ↓
STAGED OR STAGELESS?
        ↓
OBJECTIVE?
        ↓
SELECT
```

The first compatible payload you see is not automatically the correct choice.

## Choosing the Session Type

Start with the required result.

Ask:

```text id="w2p8c5"
What do I need after exploitation?
```

Possible answers:

```text id="d6m1r9"
Basic command execution
```

```text id="x4q7n2"
Interactive Meterpreter session
```

```text id="v9c3k6"
A different supported session/result
```

Use the least complex option that satisfies the authorized objective.

## When a Basic Shell Is Enough

A command shell may be sufficient when the objective is:

```text id="h1m7p4"
Verify code execution.
```

or:

```text id="c8q2v5"
Run a small number of authorized commands.
```

or:

```text id="n5r9x3"
Demonstrate controlled access.
```

There is no requirement to obtain a Meterpreter session merely because Metasploit supports one.

## When Meterpreter Is Useful

Meterpreter can be useful when the authorized objective requires capabilities associated with a richer session.

Examples include:

```text id="m4k8p2"
Session-oriented interaction
File operations
Process interaction
System information gathering
Authorized post-exploitation workflows
```

However:

```text id="z7c1q5"
More features
≠
automatically better
```

More functionality can also mean more operational complexity.

## Reverse Payload Selection

A reverse payload requires the target to connect back toward the operator.

Conceptually:

```text id="q3n8m6"
TARGET
   │
   │ outbound connection
   ▼
OPERATOR / LISTENER
```

Before choosing it, verify:

```text id="v1r7k4"
Can the target reach the operator?
```

Do not answer this from intuition.

Consider:

* Network routing
* Interface selection
* Firewall rules
* NAT
* Segmentation
* VPN topology
* Lab network design

## Bind Payload Selection

A bind payload causes the target to listen for an incoming connection.

Conceptually:

```text id="p5m2x9"
OPERATOR
   │
   │ inbound connection
   ▼
TARGET
```

Before choosing it, ask:

```text id="k8c4n1"
Can the operator reach the target listener?
```

Again, this is a network decision.

## Reverse vs Bind Decision

Use:

```text id="y6q3r8"
Can target → operator work?
       │
       ├── YES → Reverse may be suitable
       │
       └── NO
             ↓
Can operator → target work?
       │
       ├── YES → Bind may be suitable
       │
       └── NO → Reassess network path
```

This is a conceptual decision tree.

Real networks may permit both directions, neither direction, or only particular ports.

## LHOST

For reverse payloads, `LHOST` represents the local/listener-side address used by the payload's callback configuration.

For example:

```text id="f8q2m5"
set LHOST <reachable-local-address>
```

The correct value is not necessarily:

```text id="s1n7v3"
127.0.0.1
```

and not necessarily:

```text id="d4m9k6"
the first IP shown by the operating system
```

It must correspond to an address through which the target can reach the intended listener.

## LPORT

`LPORT` identifies the local/listener-side port used by the payload's connection.

Conceptually:

```text id="x7c3p8"
Target
   ↓
LHOST:LPORT
   ↓
Listener
```

The selected port must be usable in the authorized environment and consistent with the handler configuration.

## The Callback Test

Before execution, ask:

```text id="m2r8q4"
If the target follows the payload's instructions,
where exactly will it connect?
```

Then:

```text id="v5n1c7"
Can that address be reached?
```

Then:

```text id="q9k3x6"
Is the corresponding listener ready?
```

This simple reasoning catches many failures before exploitation.

## Handlers

A handler is the component that listens for incoming connections from compatible payloads.

For a reverse connection:

```text id="c4p8m2"
Payload
   ↓
Target initiates connection
   ↓
Handler listens
   ↓
Session created
```

The handler therefore provides the receiving side of the communication.

## Automatic Handlers

Some exploit workflows can automatically configure and start the appropriate handler when the module is executed.

This can simplify normal lab workflows.

However, automatic behavior can hide important details from beginners.

You should understand:

```text id="j7n3v5"
What address is being used?
What port is being used?
What payload is configured?
What handler is listening?
```

Do not rely on automation without understanding what it is doing.

## Manual Handlers

A handler can also be configured explicitly using a handler module.

A common workflow involves:

```text id="r5m1q8"
use exploit/multi/handler
```

Then configure the payload and its relevant options.

For example, conceptually:

```text id="n8c4p2"
set PAYLOAD <compatible-payload>
set LHOST <reachable-address>
set LPORT <listener-port>
```

Then inspect:

```text id="v2k7m9"
show options
```

Only execute in an authorized lab.

## Why Manual Handlers Matter

Manual handlers are useful because they make the connection model explicit.

You can separately control:

```text id="x6p3q1"
Payload
Listener address
Listener port
Handler behavior
```

This becomes especially useful when:

* The payload is generated separately.
* Exploitation and connection handling are separate stages.
* You need to restart a listener.
* You need to understand a failed callback.
* You are working with a payload created outside the exploit module.

## Handler and Payload Must Match

The handler must understand the payload it receives.

Conceptually:

```text id="k4m8v2"
Payload A
   ↓
Compatible Handler
   ↓
Session
```

Do not assume:

```text id="q1p7c5"
Any handler
+
Any payload
=
Session
```

The communication details must match.

## The Three-Way Compatibility Check

Before execution, verify:

```text id="z8n3r6"
EXPLOIT
   ↕
PAYLOAD
   ↕
HANDLER
```

All three must be compatible with the target and each other.

For a reverse payload, also verify:

```text id="m5c1x9"
TARGET
   ↕
NETWORK
   ↕
HANDLER
```

## Payload Configuration vs Exploit Configuration

Keep these concepts separate.

### Exploit Configuration

Answers:

```text id="r7q2k4"
Which target?
Which service?
Which exploit-specific values?
Which target configuration?
```

### Payload Configuration

Answers:

```text id="v3n8m1"
What should execute?
How should it communicate?
Where should it connect?
What session should it create?
```

A single module may expose options belonging to both.

Read the descriptions carefully.

## Reviewing All Options

After configuration:

```text id="p8m4c7"
show options
```

Then review:

```text id="j1x6n9"
Target settings
Payload settings
Callback settings
Exploit-specific settings
```

Do not only verify:

```text id="w5q2r8"
RHOSTS
```

while ignoring:

```text id="c9m3v6"
LHOST
LPORT
PAYLOAD
```

where those settings are relevant.

## Staged Payloads and Handlers

With a staged payload, the connection process can involve multiple steps.

Conceptually:

```text id="n7k2p5"
Exploit
   ↓
Initial payload stage
   ↓
Connection to handler
   ↓
Additional stage
   ↓
Final session
```

Therefore, a staged payload can fail after the initial connection appears successful.

Possible problem areas include:

```text id="q4v8m1"
Initial execution
Network connection
Handler
Stage transfer
Target execution environment
Session initialization
```

## Stageless Payloads and Handlers

A stageless payload carries the required payload functionality together.

Conceptually:

```text id="x3m9c6"
Exploit
   ↓
Complete payload
   ↓
Connection
   ↓
Session
```

This can simplify some troubleshooting because there is no separate second-stage retrieval.

However, it does not guarantee reliability.

The network and payload still need to work.

## Payload Size and Delivery

Payload design can affect delivery.

A larger payload may introduce constraints depending on the exploit mechanism.

Therefore:

```text id="k8r2v5"
Exploit compatibility
+
Payload compatibility
+
Delivery constraints
```

must all be considered.

Do not assume that because a payload appears in `show payloads`, it is equally suitable for every target configuration.

## Network Path Validation

For a reverse payload, map the path:

```text id="m4p7x1"
TARGET
   │
   │ outbound route
   ▼
NETWORK
   │
   ▼
OPERATOR INTERFACE
   │
   ▼
LISTENER
```

Ask:

```text id="s9c3n8"
Which interface is the listener bound to?

Can the target route to that interface?

Is the selected port reachable?

Is NAT involved?

Is a firewall blocking the connection?

Is a VPN interface involved?
```

This is often more important than changing payload names.

## Common Callback Mistake

Suppose your system has:

```text id="v2m8q4"
Ethernet IP
VPN IP
VM interface IP
Loopback IP
```

You configure:

```text id="x7p1c6"
LHOST = wrong interface
```

The exploit may execute correctly.

The target may attempt the callback.

But the session never arrives.

The problem is not necessarily:

```text id="n5r9k3"
wrong exploit
```

It may simply be:

```text id="j4q8m2"
wrong callback path
```

## Handler Port Conflicts

A handler cannot use a port that another process already occupies.

A typical symptom is:

```text id="c6v2p9"
Failed to bind
Address already in use
```

The correct response is not:

```text id="z1m7r4"
change payload immediately
```

Instead investigate:

```text id="w8q3n5"
Which process is using the port?
Is another Metasploit handler running?
Did a previous job remain active?
Should the existing listener be reused or stopped?
```

## Jobs and Handlers

Handlers may run as jobs.

You can inspect jobs with:

```text id="p4m9x2"
jobs
```

This matters because you may think:

```text id="q7c3v8"
"I started a new handler."
```

while an old handler is still occupying the port.

State awareness prevents confusion.

## Multiple Handlers

You may eventually encounter situations involving multiple listeners.

For example:

```text id="k2n8m5"
Handler A → port X
Handler B → port Y
```

Do not assume:

```text id="f6r1q9"
the newest handler
=
the handler for my payload
```

Track:

```text id="s3m7v2"
Payload
Listener
Address
Port
Target
```

Keep the relationship explicit.

## Handler Verification

Before executing a payload, verify:

```text id="v8p2c5"
Payload matches.
LHOST is correct.
LPORT is correct.
Listener is active.
Network path exists.
Target can reach the listener.
```

This creates:

```text id="m1q6r9"
PAYLOAD
  ↓
CONFIGURATION
  ↓
LISTENER
  ↓
NETWORK
```

## No Session Troubleshooting

When a reverse payload produces no session, do not randomly change payloads.

Use:

```text id="x4c9m7"
1. Did the exploit execute?
2. Did the target execute the payload?
3. Is the payload compatible?
4. Is the callback address correct?
5. Is the callback port correct?
6. Is the listener running?
7. Can the target reach the listener?
8. Is a firewall blocking the connection?
9. Is a staged payload failing during stage delivery?
10. Was a session created and immediately lost?
```

Each question represents a different hypothesis.

## Troubleshooting Branch 1 — Listener Failure

Symptom:

```text id="r5n8q2"
Handler does not start.
```

Investigate:

```text id="j7m3c9"
Port conflict
Address unavailable
Incorrect interface
Existing job
Permission/environment issue
```

## Troubleshooting Branch 2 — No Callback

Symptom:

```text id="c2v9m6"
Exploit appears to execute
but listener receives nothing.
```

Investigate:

```text id="p8k1r4"
Correct LHOST?
Correct LPORT?
Correct route?
Firewall?
NAT?
Target can make outbound connection?
```

## Troubleshooting Branch 3 — Connection Arrives but No Session

Symptom:

```text id="w4m7x2"
Handler receives activity
but usable session does not appear.
```

Investigate:

```text id="n9q3c6"
Payload compatibility
Handler compatibility
Stage delivery
Target architecture
Session initialization
```

## Troubleshooting Branch 4 — Session Appears Then Dies

Symptom:

```text id="v6r2p8"
Session opens
then immediately closes.
```

Possible causes include:

```text id="m3x7q1"
Unstable payload
Target process termination
Network interruption
Target defenses
Incorrect execution context
Environmental restrictions
```

Do not immediately switch to an unrelated payload.

Identify what changed.

## Troubleshooting Branch 5 — Wrong Session

Symptom:

```text id="k8p4n1"
A session exists,
but it is not the target you expected.
```

Investigate:

```text id="q5m9c3"
Which target generated it?
Which handler received it?
Which payload was used?
Which job is associated with it?
```

This is especially important when working with multiple lab targets.

## The One-Variable Rule

When troubleshooting payloads:

```text id="z2c6v8"
Change one meaningful variable.
```

For example:

```text id="p7m1r5"
Test callback reachability
```

before:

```text id="n4x8q3"
Changing payload
+
changing port
+
changing handler
+
changing architecture
```

Otherwise you cannot identify what fixed the problem.

## Practical Exercise 1 — Payload Selection

### Objective

Select a payload using constraints.

### Task

Choose an authorized lab exploit module.

Inspect:

```text id="c9m4x7"
show payloads
```

Choose several candidates.

For each, record:

```text id="r2p8n5"
Payload:
Platform:
Architecture:
Session type:
Reverse/bind:
Staged/stageless:
Network assumptions:
Objective fit:
Decision:
```

Do not execute until the decision is justified.

## Practical Exercise 2 — Handler Setup

### Objective

Understand the relationship between payload and handler.

In an authorized lab, configure a handler appropriate to a compatible payload.

Record:

```text id="v6k1q9"
Payload:
Handler:
LHOST:
LPORT:
Why this address:
Why this port:
Expected connection direction:
Expected session:
```

Then inspect the configuration before starting it.

## Practical Exercise 3 — Callback Reasoning

### Scenario

Your lab contains:

```text id="x3m7p2"
Target:
192.168.56.101

Operator:
Multiple network interfaces
```

### Task

Determine:

```text id="j8c4n6"
Which operator interface should receive the callback?

Why?

Can the target route to it?

What would happen if LHOST pointed to an unreachable interface?
```

Do not guess.

Use the lab's network information.

## Practical Exercise 4 — Handler Failure

### Scenario

You receive:

```text id="q5n2r8"
Address already in use
```

Build the troubleshooting sequence:

```text id="m7p3c1"
Identify port
   ↓
Determine what owns it
   ↓
Check Metasploit jobs
   ↓
Determine whether existing handler is useful
   ↓
Reuse / stop / change listener as appropriate
```

The goal is to diagnose the handler rather than change the payload.

## Practical Exercise 5 — No Session

### Scenario

The exploit appears to execute but no session is created.

Create a table:

| Hypothesis               | Evidence Needed               | Test                         |
| ------------------------ | ----------------------------- | ---------------------------- |
| Exploit failed           | Module output/target behavior | Review exploit result        |
| Payload incompatible     | Payload/target details        | Verify compatibility         |
| Callback wrong           | Network configuration         | Verify route/address         |
| Listener unavailable     | Handler/jobs state            | Inspect listener             |
| Firewall blocks callback | Network behavior              | Test permitted connectivity  |
| Stage failed             | Handler/output                | Inspect staged communication |
| Session died             | Session/job state             | Check session history        |

The purpose is to turn a vague failure into testable hypotheses.

## Practical Exercise 6 — Expected vs Actual

Before executing:

```text id="w9k4p2"
Expected:
Target executes compatible payload
and connects to configured handler.
```

After execution:

```text id="f1m8c5"
Actual:
...
```

Then classify the failure, if any:

```text id="r6q3n7"
Exploit
Payload
Handler
Network
Session
Unknown
```

Do not call the entire operation a failure until you identify where the chain broke.

## Common Mistakes

### Mistake 1 — Choosing Payload First

Correction:

```text id="p8x2m6"
Select exploit
→ inspect supported payloads
→ filter by constraints
```

### Mistake 2 — Using a Favorite Payload Everywhere

Correction:

```text id="c5n9r1"
Network and target conditions determine suitability.
```

### Mistake 3 — Treating LHOST as "My IP"

Correction:

```text id="v7m3q8"
LHOST must represent a reachable callback/listener address.
```

### Mistake 4 — Ignoring the Listener

Correction:

```text id="j2p6k4"
A reverse payload requires a compatible receiving side.
```

### Mistake 5 — Ignoring Existing Jobs

Correction:

```text id="n8q4c1"
Inspect jobs and listener state before starting another handler.
```

### Mistake 6 — Changing Multiple Variables

Correction:

```text id="x3r7m9"
Change one meaningful variable at a time.
```

### Mistake 7 — Assuming No Session Means Exploit Failure

Correction:

```text id="k6v1p5"
Separate exploit,
payload,
handler,
network,
and session stages.
```

### Mistake 8 — Using More Payload Features Than Necessary

Correction:

```text id="s9m2q7"
Choose the minimum payload functionality
that satisfies the objective.
```

## Professional Payload Workflow

Use this sequence:

```text id="w4p8n3"
1. Define objective.
2. Select exploit.
3. Inspect supported payloads.
4. Determine target platform.
5. Determine architecture where relevant.
6. Determine required session type.
7. Determine network direction.
8. Select reverse or bind.
9. Determine staged/stageless suitability.
10. Configure payload.
11. Configure or verify handler.
12. Verify listener address and port.
13. Verify network path.
14. Execute in authorized scope.
15. Verify session/result.
16. Diagnose failures by stage.
17. Document the result.
```

## The Full Exploitation Design

At this point, your mental model should be:

```text id="q7m1c5"
OBJECTIVE
    ↓
TARGET EVIDENCE
    ↓
EXPLOIT
    ↓
SUPPORTED PAYLOADS
    ↓
PAYLOAD COMPATIBILITY
    ↓
HANDLER
    ↓
NETWORK PATH
    ↓
EXECUTION
    ↓
SESSION / RESULT
    ↓
VERIFICATION
```

Every arrow is a possible failure point.

That is why payload troubleshooting becomes much easier when you stop thinking of exploitation as a single event.

## Know When to Stop

Stop changing payloads when:

```text id="n3x8r6"
The evidence points to an exploit-side problem.
```

or:

```text id="m5q2p9"
The target cannot reach the listener.
```

or:

```text id="c7v4k1"
The objective does not require a session.
```

or:

```text id="j8r1m6"
No supported payload satisfies the environment.
```

or:

```text id="p2n9q5"
Repeated attempts are not producing new information.
```

At that point:

```text id="x6m3v8"
diagnose
```

or:

```text id="r4k7c2"
change the approach
```

rather than continuing a payload lottery.

## Completion Checklist

Before moving into exploitation, confirm that you can:

```text id="w1q8m4"
[ ] Explain exploit vs payload.
[ ] Inspect supported payloads.
[ ] Select payloads using constraints.
[ ] Determine target platform compatibility.
[ ] Determine architecture compatibility.
[ ] Choose an appropriate session type.
[ ] Explain reverse payloads.
[ ] Explain bind payloads.
[ ] Explain LHOST and LPORT conceptually.
[ ] Understand handler responsibilities.
[ ] Configure a compatible handler in a lab.
[ ] Distinguish automatic and manual handlers.
[ ] Understand staged and stageless payload behavior.
[ ] Validate callback/network assumptions.
[ ] Diagnose handler failures.
[ ] Diagnose callback failures.
[ ] Diagnose session creation failures.
[ ] Use the one-variable troubleshooting rule.
[ ] Know when changing payloads is not the correct next step.
```

## Key Mental Model

Remember:

```text id="k9p3v6"
EXPLOIT
   ↓
PAYLOAD
   ↓
HANDLER
   ↓
NETWORK
   ↓
SESSION
```

When there is no session, ask:

```text id="q4m8x1"
Where did the chain break?
```

Not:

```text id="s7n2c5"
Which random payload should I try next?
```

That question will become one of the most important troubleshooting habits in the rest of this curriculum.

## Section Complete

You have now completed the payload foundation:

```text id="v5c1m8"
Payload Mental Model
        +
Payload Selection
        +
Handler Configuration
        +
Callback Troubleshooting
```

The next section moves into exploitation.

The next file is:

```text id="z2r7p4"
05-exploitation/01-exploitation-workflow.md
```

There we will combine everything learned so far into a disciplined **evidence → validation → controlled exploitation → result verification** workflow.
