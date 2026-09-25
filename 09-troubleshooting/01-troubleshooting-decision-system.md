# Troubleshooting Decision System

## Objective

Learn how to troubleshoot Metasploit failures systematically instead of responding with random module changes, payload changes, or repeated exploitation attempts.

By the end of this file, you should be able to:

* Classify a Metasploit failure.
* Identify which stage of the workflow failed.
* Separate target problems from configuration problems.
* Distinguish exploit, payload, handler, network, session, and database failures.
* Use evidence to narrow the problem.
* Change one assumption at a time.
* Avoid repeating ineffective attempts.
* Recognize when the tool is not the problem.
* Know when to abandon an approach and choose another tool.
* Document troubleshooting so another operator can reproduce it.

## Why Troubleshooting Matters

Metasploit failures are normal.

The professional difference is how you respond.

Poor workflow:

```text id="m8x3qc"
Exploit failed
  ↓
Try another exploit
  ↓
Try another payload
  ↓
Change port
  ↓
Try again
  ↓
Try again
```

Professional workflow:

```text id="q4n7vp"
FAILURE
  ↓
IDENTIFY FAILED STAGE
  ↓
COLLECT EVIDENCE
  ↓
FORM HYPOTHESIS
  ↓
CHANGE ONE ASSUMPTION
  ↓
RETEST
  ↓
COMPARE RESULT
```

The second approach produces knowledge.

## The Universal Troubleshooting Model

Use:

```text id="v6m2ka"
OBSERVED FAILURE
      ↓
WHAT EXACTLY FAILED?
      ↓
WHICH STAGE?
      ↓
WHAT EVIDENCE SUPPORTS THAT?
      ↓
WHAT ASSUMPTION COULD BE WRONG?
      ↓
CHANGE ONE THING
      ↓
RETEST
      ↓
DID THE RESULT CHANGE?
```

Never skip:

```text id="n3x8wp"
"What exactly failed?"
```

## The Metasploit Failure Chain

Map failures onto the normal workflow:

```text id="j5q9mc"
SCOPE
  ↓
TARGET
  ↓
RECONNAISSANCE
  ↓
MODULE
  ↓
REQUIREMENTS
  ↓
CONFIGURATION
  ↓
VALIDATION
  ↓
EXPLOIT
  ↓
PAYLOAD
  ↓
HANDLER
  ↓
SESSION
  ↓
POST-EXPLOITATION
```

The further down the chain the failure occurs, the less useful it is to restart from the beginning without evidence.

## First Question: What Actually Happened?

Avoid vague statements such as:

```text id="r2m7xn"
"It didn't work."
```

Replace them with:

```text id="c8v4qa"
The module rejected the target.
```

or:

```text id="p6k1ws"
The exploit ran but no session appeared.
```

or:

```text id="y9m3kc"
The session appeared and immediately terminated.
```

or:

```text id="f5x8nr"
The database is unavailable.
```

or:

```text id="a7q2vp"
The resource script stopped at a configuration error.
```

Specific failure descriptions dramatically reduce troubleshooting time.

## Failure Classification

Use these broad categories:

```text id="u4n8mc"
1. Scope / target problem
2. Reconnaissance problem
3. Module-selection problem
4. Module-requirement problem
5. Configuration problem
6. Target compatibility problem
7. Exploit problem
8. Payload problem
9. Handler problem
10. Network problem
11. Session problem
12. Privilege problem
13. Database/workspace problem
14. Automation/resource-script problem
15. Tool/environment problem
```

The goal is not to memorize the categories.

The goal is to identify where the workflow broke.

## Troubleshooting Starts With Evidence

Before changing anything, collect:

```text id="w7m2pk"
Module
Target
Relevant options
Error/output
Observed target state
Session state
Network assumptions
Database state
```

Then ask:

```text id="s3q9vx"
Which assumption does this evidence challenge?
```

## The One-Change Rule

One of the most important rules in this repository:

```text id="k5x8mq"
CHANGE ONE ASSUMPTION
      ↓
RETEST
```

Suppose:

```text id="n8v3yc"
Exploit fails.
```

You simultaneously change:

```text id="r4m7qp"
Target
Payload
LHOST
LPORT
Module
```

and it works.

What did you learn?

```text id="j2c9va"
Almost nothing.
```

Instead:

```text id="p6x4mw"
Change LHOST
      ↓
Retest
      ↓
Compare
```

Now the result provides information.

## Troubleshooting as an Experiment

Think scientifically:

```text id="v9m3ks"
HYPOTHESIS
  ↓
CHANGE
  ↓
TEST
  ↓
OBSERVATION
  ↓
CONCLUSION
```

For example:

```text id="q7r2nc"
Hypothesis:
Callback cannot reach the handler.

Change:
Correct the callback address.

Test:
Repeat controlled execution.

Observation:
Session appears.

Conclusion:
Previous callback configuration was incorrect.
```

This is much stronger than:

```text id="w5k8xp"
"Changing things until it worked."
```

## Target Problems

Start here when the target itself may be wrong.

Ask:

```text id="m4n7qc"
Is the target in scope?
Is the address correct?
Is the target reachable?
Is the expected service present?
Is the expected port open?
Is the target version what I assumed?
```

A module cannot compensate for an incorrect target assumption.

## Target Reachability

Before blaming the exploit, establish basic connectivity.

Conceptually:

```text id="x8p3vr"
YOUR SYSTEM
    ↓
NETWORK PATH
    ↓
TARGET
    ↓
EXPECTED SERVICE
```

If the path fails, exploitation troubleshooting is premature.

## Service Mismatch

Suppose your assumption is:

```text id="f6q2km"
HTTP service on port 8080.
```

But current evidence shows:

```text id="c9m4ya"
SSH service on port 8080.
```

The problem is not necessarily Metasploit.

The problem is:

```text id="j3v8pn"
TARGET ASSUMPTION
```

Return to reconnaissance.

## Module-Selection Problems

A module may be technically valid but irrelevant to the target.

Ask:

```text id="q5x1md"
Does the module actually correspond to the target technology?
Does the target version match?
Does the module description support this scenario?
Are the required conditions present?
```

Do not select a module simply because its name resembles the service.

## Module Requirements

Use module information to determine prerequisites.

A module may require:

```text id="u8m3qx"
Specific target version
Specific service
Specific option
Authentication
A reachable endpoint
A particular configuration
```

The correct troubleshooting question is:

```text id="r7k4vc"
Which requirement is not currently satisfied?
```

## Configuration Problems

Configuration errors are among the easiest failures to eliminate.

Review:

```text id="n2p9wx"
RHOSTS
RPORT
LHOST
LPORT
Target selection
Required module options
Payload options
Other module-specific settings
```

Do not change every option simultaneously.

Find the one that is inconsistent with the environment.

## Required vs Optional Options

Use module information to distinguish:

```text id="v5x8qa"
Required
```

from:

```text id="j1m7kc"
Optional
```

An unset required option should be fixed before execution.

An optional option should not automatically be changed.

## Payload Problems

If the exploit appears to work but no session appears, investigate the payload path.

Think:

```text id="q6n3yr"
EXPLOIT
  ↓
PAYLOAD DELIVERY
  ↓
PAYLOAD EXECUTION
  ↓
CALLBACK / CONNECTION
  ↓
HANDLER
  ↓
SESSION
```

A failure anywhere in this chain can produce:

```text id="p8v4mx"
No usable session.
```

Therefore:

```text id="s5k2qc"
NO SESSION
≠
EXPLOIT FAILED
```

## Payload Compatibility

Review:

```text id="m9x3ka"
Target OS
Architecture
Payload type
Staged vs stageless behavior
Transport
Network path
Session type
```

Do not randomly cycle through payloads.

Choose one based on the target and objective.

## Handler Problems

A reverse connection requires a listener/handler path that matches the payload configuration.

Check:

```text id="r4n8vp"
Expected callback address
Expected callback port
Listener configuration
Network reachability
Local interface/address
```

A payload can execute correctly while the callback never reaches the handler.

## LHOST Is Not "My IP"

One common conceptual error is:

```text id="c7m2qx"
LHOST = any IP address on my machine
```

The relevant question is:

```text id="f1v8nk"
Which local address can the target actually reach for this callback?
```

The correct value depends on the network topology.

## LPORT Is Not Automatically the Target Port

Another common mistake is assuming:

```text id="w5q3ma"
Target service port
=
Callback port
```

These are conceptually different.

```text id="j8n2vc"
RPORT
→ target service

LPORT
→ local callback listener
```

The values may happen to be similar in some environments, but they represent different things.

## Network Problems

When a reverse session does not appear, inspect the network path.

Think:

```text id="a6m9xr"
TARGET
  ↓
CALLBACK ADDRESS
  ↓
NETWORK ROUTE
  ↓
LOCAL LISTENER
  ↓
SESSION
```

Potential problems include:

```text id="p3v7kc"
Wrong interface
Wrong route
Firewall
NAT
Segmentation
VPN configuration
Listener binding
Incorrect address
```

Do not immediately change the payload if the callback cannot reach the listener.

## Session Problems

If the session appears and then dies:

```text id="x4k8mq"
SESSION CREATED
      ↓
SESSION TERMINATED
```

Investigate:

```text id="n7r2va"
Target process
Payload stability
Network interruption
Handler behavior
Target controls
Session type
```

The exploit may have succeeded.

The session may simply be unstable.

## Privilege Problems

A session can work correctly while an action fails because the current context lacks the required privileges.

For example:

```text id="m5q9xc"
Session:
Works.

Action:
Requires elevated privileges.

Result:
Denied.
```

Do not label this a session failure.

The more accurate diagnosis is:

```text id="r8v3kp"
PRIVILEGE LIMITATION
```

## Database Problems

If database-backed functionality fails, first establish:

```text id="q2n6wm"
Is the database connected?
Is the correct workspace selected?
Is the expected data present?
```

Use:

```text id="d9m4xa"
db_status
```

Then inspect workspace state as needed.

Do not troubleshoot database queries before establishing database connectivity.

## Stale Database Data

Suppose:

```text id="j7x3pn"
Database:
Port 80 open.
```

But current testing shows:

```text id="v5m8qc"
Port 80 closed.
```

Do not assume Metasploit is broken.

The database may simply contain historical information.

The correct response is:

```text id="k1r7ya"
Refresh / validate current state.
```

## Resource-Script Problems

If automation fails:

```text id="f6m2vr"
Identify the exact failing command.
```

Do not immediately rerun the entire script.

Use:

```text id="p8x4kc"
SCRIPT
  ↓
FAILED STEP
  ↓
ISOLATE
  ↓
UNDERSTAND
  ↓
CORRECT
  ↓
RETEST
```

## Tool-Version Problems

Sometimes behavior changes between versions.

If a command or workflow behaves differently than expected:

```text id="w3q9mx"
Check local help
Check module information
Check installed version
Check official documentation
```

Do not assume an old tutorial exactly matches your current installation.

## Environment Problems

Sometimes the environment itself is the problem.

Examples:

```text id="s7n2vp"
Database service unavailable
Dependency missing
Incorrect VM networking
Container not running
Firewall changed
VPN disconnected
Resource limitations
```

Before changing exploitation logic, verify the environment.

## The Full Troubleshooting Decision Tree

Use:

```text id="u4m8qc"
FAILURE
   ↓
WHAT EXACTLY FAILED?
   ↓
SCOPE / TARGET?
   │
   ├── YES → Verify scope, address, reachability, service
   │
   └── NO
        ↓
MODULE?
   │
   ├── YES → Verify module relevance and requirements
   │
   └── NO
        ↓
CONFIGURATION?
   │
   ├── YES → Correct one invalid option
   │
   └── NO
        ↓
EXPLOIT?
   │
   ├── YES → Review target conditions and module behavior
   │
   └── NO
        ↓
PAYLOAD?
   │
   ├── YES → Review compatibility and configuration
   │
   └── NO
        ↓
HANDLER / NETWORK?
   │
   ├── YES → Verify callback path
   │
   └── NO
        ↓
SESSION?
   │
   ├── YES → Diagnose stability/context
   │
   └── NO
        ↓
DATABASE / AUTOMATION / ENVIRONMENT?
```

This is a classification system, not a rigid sequence.

Use the evidence to choose the relevant branch.

## Troubleshooting by Observable State

### State 1 — Module Will Not Run

Check:

```text id="k6m3xp"
Required options
Module compatibility
Target configuration
Module-specific requirements
```

### State 2 — Module Runs but Reports Failure

Check:

```text id="q9r2va"
Target assumptions
Target version
Vulnerability conditions
Module limitations
```

### State 3 — Exploit Appears to Work but No Session

Check:

```text id="n5x7kc"
Payload
Handler
Callback address
Callback port
Network path
Target process
```

### State 4 — Session Appears but Dies

Check:

```text id="f2m8qp"
Session stability
Payload behavior
Network reliability
Target process
Environmental controls
```

### State 5 — Session Works but Action Fails

Check:

```text id="v7k3mx"
Privileges
Session type
Operating system
Required capability
Target state
```

### State 6 — Database Query Returns Unexpected Results

Check:

```text id="c4n9wr"
Database status
Workspace
Data freshness
Data source
```

## Troubleshooting Log

Maintain a simple record:

```text id="j8p2vq"
# Troubleshooting Record

Failure:
...

Expected:
...

Observed:
...

Stage:
...

Evidence:
...

Hypothesis:
...

One change:
...

Retest:
...

Result:
...

Conclusion:
...

Next action:
...
```

This prevents circular troubleshooting.

## Example Troubleshooting Session

Scenario:

```text id="m3x7ka"
Exploit appears to run.
No session is created.
```

Bad response:

```text id="r5n2vc"
Try five different payloads.
```

Better:

```text id="q8v4mx"
1. Verify target.
2. Verify module requirements.
3. Confirm exploit behavior.
4. Inspect payload configuration.
5. Verify callback address.
6. Verify callback port.
7. Verify network reachability.
8. Change one assumption.
9. Retest.
```

If the callback address is corrected and the session appears:

```text id="w6k1pn"
Previous callback configuration was the likely cause.
```

Now the troubleshooting produced knowledge.

## What Not to Change Randomly

Avoid simultaneously changing:

```text id="p9m3xq"
Module
Target
RPORT
Payload
LHOST
LPORT
Target selection
```

because you lose causality.

Use:

```text id="h4v8kc"
ONE CHANGE
→
ONE TEST
→
ONE OBSERVATION
```

## When to Abandon the Approach

Troubleshooting does not mean forcing Metasploit to succeed.

Stop and reconsider when:

```text id="u7n2ma"
The target does not match the module.
```

```text id="x3q8vp"
Required conditions are absent.
```

```text id="f5m9kc"
The module is not appropriate for the target.
```

```text id="r2v6yn"
The objective is better answered by another tool.
```

```text id="k8m4qx"
Further attempts create unnecessary risk.
```

The correct conclusion may be:

```text id="s6p3wa"
Metasploit is not the appropriate tool for this objective.
```

That is a successful decision, not a failure.

## Practical Exercise 1 — Failure Classification

For each scenario, identify the most likely failure category:

```text id="m4x8qp"
1. Required RHOSTS option is missing.
2. Target service is not reachable.
3. Exploit runs but no callback arrives.
4. Session opens and immediately terminates.
5. Action is denied because of insufficient privileges.
6. Database commands show no expected hosts.
7. Resource script fails at a workspace command.
8. Module does not support the target version.
```

For each, explain:

```text id="j7n2vc"
What evidence supports your classification?
What should be checked next?
```

## Practical Exercise 2 — One-Change Troubleshooting

Scenario:

```text id="q5m9xa"
A lab exploit runs but no session appears.

Current assumptions:
Target verified.
Module verified.
Payload selected.
Handler configured.
```

Choose exactly one assumption to investigate first.

Document:

```text id="v3k7pw"
Hypothesis:
...

Evidence:
...

One change:
...

Expected result:
...

Retest:
...

Observed result:
...
```

### Success Criteria

You do not change multiple variables simultaneously.

## Practical Exercise 3 — Build a Failure Tree

Take one failed lab attempt and map:

```text id="n8x4qm"
Failure
  ↓
Stage
  ↓
Possible causes
  ↓
Evidence
  ↓
Next test
```

### Success Criteria

You can identify a logical next test without guessing.

## Practical Exercise 4 — Session Failure

Scenario:

```text id="c2v7mp"
A session was created successfully.

Thirty seconds later:
The session disappears.
```

Investigate:

```text id="f9m3ka"
What changed?
Was the target reachable?
Was the target process still present?
Was the session stable before termination?
Could network conditions explain the loss?
```

### Success Criteria

You do not automatically rerun the exploit.

## Practical Exercise 5 — Database Failure

Scenario:

```text id="r6p2xn"
hosts
```

returns unexpected or empty results.

Determine:

```text id="w4m8qc"
1. Is the database connected?
2. What workspace is active?
3. Does that workspace contain data?
4. Was the expected data actually imported?
5. Is the data stale?
```

### Success Criteria

You troubleshoot the data path rather than assuming the query is broken.

## Practical Exercise 6 — Decide Whether to Switch Tools

Scenario:

```text id="k9x3va"
Objective:
Analyze HTTP request behavior.

Metasploit:
Provides limited visibility.

Burp Suite:
Provides direct request/response inspection.
```

Decide:

```text id="m2q7pw"
Should Metasploit remain the primary tool?
Why?
```

The lesson is:

```text id="v8n4kc"
Tool selection follows the objective.
```

## Common Mistakes

### Mistake 1 — "Try Another Payload"

Correction:

```text id="p4x8mq"
First identify which stage failed.
```

### Mistake 2 — Changing Five Variables

Correction:

```text id="j7m3vc"
Change one assumption at a time.
```

### Mistake 3 — Treating No Session as Exploit Failure

Correction:

```text id="r8q2ka"
Investigate the payload, handler, and network path.
```

### Mistake 4 — Treating Access Denied as Tool Failure

Correction:

```text id="n6v4xp"
Check privileges and session capabilities.
```

### Mistake 5 — Ignoring Target Verification

Correction:

```text id="c3m9wa"
Return to reconnaissance when target assumptions are uncertain.
```

### Mistake 6 — Repeating the Same Failed Attempt

Correction:

```text id="x5k8qn"
Change the hypothesis, not just the command.
```

### Mistake 7 — Assuming Database Data Is Current

Correction:

```text id="f2r7mc"
Check freshness and validate against the target.
```

### Mistake 8 — Blaming Metasploit for Environment Problems

Correction:

```text id="u9p4vx"
Check networking, dependencies, VM state, database state, and target reachability.
```

### Mistake 9 — Troubleshooting Forever

Correction:

```text id="m8q3ka"
Know when the evidence says the approach is inappropriate.
```

## Professional Troubleshooting Workflow

Use:

```text id="z6x2mp"
OBSERVE FAILURE
      ↓
DESCRIBE PRECISELY
      ↓
LOCATE FAILED STAGE
      ↓
COLLECT EVIDENCE
      ↓
FORM HYPOTHESIS
      ↓
CHANGE ONE THING
      ↓
RETEST
      ↓
COMPARE
      ↓
DOCUMENT
      ↓
CONTINUE OR ABANDON
```

This is the same reasoning pattern used throughout the repository.

## The Troubleshooting Hierarchy

When something fails, investigate from the simplest assumptions toward the more complex ones:

```text id="k4m8vq"
SCOPE
  ↓
TARGET
  ↓
REACHABILITY
  ↓
SERVICE
  ↓
MODULE
  ↓
REQUIREMENTS
  ↓
CONFIGURATION
  ↓
EXPLOIT
  ↓
PAYLOAD
  ↓
HANDLER
  ↓
SESSION
  ↓
POST-EXPLOITATION
```

This does not mean every failure must follow this exact order.

It means:

```text id="p7x3nc"
Do not jump to complex explanations
before checking simpler ones.
```

## Troubleshooting as a Skill

A strong Metasploit operator is not someone who never encounters errors.

It is someone who can transform:

```text id="v2n8qa"
ERROR
```

into:

```text id="j5m3xc"
EVIDENCE
  ↓
HYPOTHESIS
  ↓
TEST
  ↓
KNOWLEDGE
```

That skill transfers beyond Metasploit.

It applies to:

```text id="r9k4wp"
Burp Suite
Nmap
Nessus
Wireshark
Linux
Windows
Cloud environments
Security tooling generally
```

## Know When to Stop

Stop troubleshooting when:

```text id="m6q2vx"
The issue is resolved.
```

or:

```text id="c8p4na"
The objective can be satisfied another way.
```

or:

```text id="x1r7mk"
The module is demonstrably inappropriate.
```

or:

```text id="f9v3qc"
Further attempts would create unnecessary impact.
```

A professional conclusion can be:

```text id="s5k8wp"
"Tested approach was not suitable for the observed target state."
```

That is better than forcing an exploit simply to obtain a session.

## Completion Checklist

Before moving to decision guides, confirm that you can:

```text id="q7m4xn"
[ ] Describe a failure precisely.
[ ] Identify the failed workflow stage.
[ ] Classify common failure types.
[ ] Verify target reachability.
[ ] Verify module relevance.
[ ] Review module requirements.
[ ] Review configuration.
[ ] Distinguish exploit failure from payload failure.
[ ] Distinguish payload failure from handler failure.
[ ] Investigate callback/network problems.
[ ] Diagnose session instability.
[ ] Recognize privilege limitations.
[ ] Troubleshoot database/workspace problems.
[ ] Troubleshoot resource scripts.
[ ] Apply the one-change rule.
[ ] Form and test hypotheses.
[ ] Document troubleshooting.
[ ] Know when to abandon an approach.
[ ] Know when another tool is more appropriate.
[ ] Know when to stop.
```

## Key Mental Model

Remember:

```text id="w3m8qx"
FAILURE
  ↓
LOCATE
  ↓
EXPLAIN
  ↓
CHANGE ONE THING
  ↓
RETEST
  ↓
LEARN
```

And:

```text id="a9v2kc"
DO NOT TROUBLESHOOT BY RANDOMNESS.

TROUBLESHOOT BY EVIDENCE.
```

The professional objective is not:

```text id="n4x7mp"
"Make Metasploit work."
```

It is:

```text id="r6q1va"
"Determine why the approach succeeded or failed,
then choose the most appropriate next action."
```

## Next Step

The next file is:

```text id="k2m8qx"
10-decision-guides/01-operator-decision-tree.md
```

There we will consolidate the repository's concepts into a **single operator decision tree for moving from scope and reconnaissance through module selection, exploitation, sessions, post-exploitation, evidence, troubleshooting, and cleanup**.
