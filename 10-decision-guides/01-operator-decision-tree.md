# Metasploit Operator Decision Tree

## Objective

Turn the concepts in this repository into one reusable operator workflow.

The goal is not to memorize commands or module names.

The goal is to repeatedly answer:

```text
What do I know?
        ↓
What do I need to know?
        ↓
What capability do I need?
        ↓
Is Metasploit appropriate?
        ↓
How do I validate my assumptions?
        ↓
What did the result actually prove?
        ↓
What is the next authorized objective?
```

This decision tree should work even when the target, service, vulnerability, module, payload, and failure mode are unfamiliar.

## The Complete Operator Decision Tree

```text
START
  │
  ▼
CONFIRM AUTHORIZATION + SCOPE
  │
  ├── Scope unclear? ───────────────► STOP / CLARIFY
  │
  ▼
UNDERSTAND TARGET
  │
  ├── Insufficient information?
  │          │
  │          ▼
  │     PERFORM DISCOVERY
  │     USING APPROPRIATE TOOLS
  │          │
  │          ▼
  │     UPDATE TARGET MODEL
  │
  ▼
DEFINE OBJECTIVE
  │
  ├── Objective unclear? ───────────► STOP / CLARIFY
  │
  ▼
IDENTIFY MISSING INFORMATION
  │
  ├── Need service/version? ────────► ENUMERATE
  ├── Need OS/architecture? ────────► ENUMERATE
  ├── Need vulnerability evidence? ─► VALIDATE
  └── Already enough information? ──► CONTINUE
  │
  ▼
FIND RELEVANT CAPABILITY
  │
  ├── Matching Metasploit capability?
  │          │
  │          ├── NO ────────────────► USE ANOTHER TOOL
  │          │
  │          └── YES
  │
  ▼
READ MODULE
  │
  ▼
CHECK REQUIREMENTS
  │
  ├── Requirements not satisfied?
  │          │
  │          ▼
  │     GATHER INFORMATION
  │     OR CHANGE APPROACH
  │
  ▼
CONFIGURE
  │
  ▼
VALIDATE CONFIGURATION
  │
  ├── Validation fails?
  │          │
  │          ▼
  │     DIAGNOSE
  │     ↓
  │     CHANGE ONE ASSUMPTION
  │     ↓
  │     RETEST
  │
  ▼
EXECUTE
  │
  ▼
INTERPRET RESULT
  │
  ├── Expected result absent?
  │          │
  │          ▼
  │     CLASSIFY FAILURE
  │          ↓
  │     TROUBLESHOOT
  │          ↓
  │     RETEST OR CHANGE APPROACH
  │
  └── Expected result present
             │
             ▼
          VERIFY
             │
             ├── Not independently verified?
             │          │
             │          ▼
             │       COLLECT MORE EVIDENCE
             │
             ▼
        OBJECTIVE SATISFIED?
             │
             ├── YES ──────────────► DOCUMENT
             │                         ↓
             │                      CLEAN UP
             │                         ↓
             │                       STOP
             │
             └── NO
                  │
                  ▼
             DEFINE NEXT OBJECTIVE
                  │
                  ▼
             MANAGE SESSION
                  │
                  ▼
             POST-EXPLOITATION
                  │
                  ▼
             COLLECT EVIDENCE
                  │
                  ▼
             CLEAN UP
                  │
                  ▼
             NEXT AUTHORIZED OBJECTIVE
```

## Branch 1 — Start With Scope

Before opening Metasploit, determine what you are actually authorized to test.

### Ask

```text
What targets are in scope?
What targets are explicitly out of scope?
What actions are allowed?
What actions require additional approval?
What is the testing window?
What evidence is required?
What actions could cause instability?
```

### Stop Condition

Stop when:

* the target cannot be confidently associated with the authorized scope
* the requested action exceeds the agreed rules
* the impact of the action is unclear
* authorization is ambiguous

Do not solve an authorization problem with a technical assumption.

## Branch 2 — Do I Know Enough About the Target?

Metasploit is not a replacement for understanding the target.

Ask:

```text
What host am I dealing with?
What service is relevant?
Which port is relevant?
What technology appears to be present?
What version information do I have?
What operating system information do I have?
What evidence supports those assumptions?
```

### If Information Is Missing

Do discovery first.

Possible sources include:

* Nmap
* service enumeration
* web enumeration
* Burp Suite
* Nessus/OpenVAS
* SMB enumeration
* DNS enumeration
* application-specific reconnaissance

The correct tool depends on the information gap.

### Example

Suppose you know only:

```text
Target: 192.0.2.50
```

That is not enough to select an exploit intelligently.

A better target model might become:

```text
Host: 192.0.2.50
Port: 445
Service: SMB
Version: <observed version>
OS: <observed OS>
Relevant evidence: <specific finding>
```

The important change is not the amount of information.

It is the reduction of uncertainty.

## Branch 3 — Define the Objective

Do not begin with:

```text
"What exploit can I run?"
```

Begin with:

```text
"What am I trying to prove or accomplish?"
```

Examples:

```text
Determine whether a suspected vulnerability is exploitable.
Validate the impact of a confirmed vulnerability.
Obtain a controlled session in an authorized lab.
Determine the privileges obtained after exploitation.
Collect evidence demonstrating impact.
Validate whether a specific security control can be bypassed.
```

An objective should be observable.

Weak:

```text
Test the server.
```

Better:

```text
Determine whether the identified service vulnerability allows remote code execution.
```

Better still:

```text
Determine whether exploitation results in code execution under the expected account and collect evidence without making persistent changes.
```

## Branch 4 — Identify the Missing Information

Once the objective is clear, ask:

```text
What must be true for this objective to be achieved?
What do I already know?
What do I still need to know?
```

Use this model:

```text
OBJECTIVE
    ↓
REQUIRED CONDITIONS
    ↓
KNOWN FACTS
    ↓
MISSING FACTS
    ↓
NEXT INFORMATION-GATHERING ACTION
```

### Example

Objective:

```text
Validate suspected remote code execution.
```

Required conditions might include:

```text
Target is reachable
Relevant service is exposed
Service/version matches the vulnerability
Target is compatible with the module
Module requirements are satisfied
Execution method is appropriate
Result can be verified safely
```

Do not jump from suspicion directly to exploitation.

## Branch 5 — Does Metasploit Have the Right Capability?

Now ask:

```text
Can Metasploit perform the required action?
```

Possible answers:

```text
YES
    ↓
Find and evaluate the capability.

NO
    ↓
Use another appropriate tool.

UNCERTAIN
    ↓
Search and inspect before deciding.
```

The existence of a Metasploit module does not automatically make Metasploit the right tool.

### Examples

| Objective                          | Potentially appropriate approach                         |
| ---------------------------------- | -------------------------------------------------------- |
| Discover exposed services          | Nmap                                                     |
| Intercept and modify web requests  | Burp Suite                                               |
| Validate a known exploit           | Metasploit                                               |
| Analyze packet behavior            | Wireshark                                                |
| Identify web directories           | Web enumeration tools                                    |
| Test a database vulnerability      | Appropriate database tooling / Metasploit where suitable |
| Collect post-exploitation evidence | Meterpreter or native tooling depending on objective     |

Tool selection should follow the objective.

## Branch 6 — Read the Module Before Running It

Once a relevant module is found:

```text
SEARCH
  ↓
IDENTIFY
  ↓
INFO
  ↓
UNDERSTAND
  ↓
CONFIGURE
```

Read:

* module purpose
* description
* references
* targets
* options
* required options
* default values
* payload compatibility
* supported platforms
* limitations
* notes that affect safe operation

### Critical Question

Ask:

```text
Why should this module work against this target?
```

If the answer is only:

```text
"The name looks right."
```

you do not have enough confidence.

## Branch 7 — Check Requirements

Build a requirement map.

| Requirement                  | Known? | Evidence                   |
| ---------------------------- | -----: | -------------------------- |
| Target reachable             | Yes/No | Network test               |
| Relevant port open           | Yes/No | Enumeration                |
| Service identified           | Yes/No | Service detection          |
| Version compatible           | Yes/No | Banner/version evidence    |
| OS compatible                | Yes/No | Enumeration                |
| Architecture compatible      | Yes/No | Target information         |
| Required module option known | Yes/No | Module documentation       |
| Payload compatible           | Yes/No | Payload/module information |
| Handler reachable            | Yes/No | Network configuration      |

A missing requirement is an information problem.

Do not hide it by guessing.

## Branch 8 — Configure Deliberately

Configuration should answer:

```text
What does this option control?
Why am I setting it?
What value should it have?
What evidence supports that value?
```

Avoid blindly copying configurations.

For example:

```text
RHOSTS
```

should represent the authorized target.

```text
LHOST
```

should represent the address reachable by the target when a reverse connection is required.

The exact value depends on the lab or engagement network.

### Configuration Rule

```text
Every important option should have a reason.
```

If you cannot explain why an option has its current value, stop and investigate it.

## Branch 9 — Validate Before Execution

Use available validation mechanisms when appropriate.

For example:

```text
check
```

may provide useful information for modules that support it.

But remember:

```text
CHECK RESULT ≠ GUARANTEED EXPLOIT SUCCESS
```

Likewise:

```text
NO CHECK
    ≠
NOT VULNERABLE
```

A module may not support reliable pre-exploitation validation.

Therefore, interpret validation in context.

## Branch 10 — Execute

Before execution ask:

```text
Is this action authorized?
Is the target correct?
Is the configuration correct?
Is the expected impact acceptable?
Do I know how I will verify success?
Do I know how I will stop or clean up if something goes wrong?
```

Then execute.

The objective is controlled validation, not maximum activity.

## Branch 11 — Interpret the Result

Never treat console output as proof by itself.

After execution ask:

```text
What did the tool actually report?
What should I expect if successful?
What evidence would independently confirm success?
Did I obtain the expected capability?
Did I obtain a session?
Under which context?
Is the objective actually satisfied?
```

### Example

A message indicating that an exploit was sent does not necessarily prove:

```text
Code execution
```

A session may provide stronger evidence, but even then you should verify:

```text
Who am I?
Where am I?
What privileges do I have?
What objective-specific evidence exists?
```

Mental model:

```text
EXPLOIT OUTPUT
      ↓
OBSERVED RESULT
      ↓
VERIFICATION
      ↓
CONCLUSION
```

## Branch 12 — If Execution Fails

Do not immediately change five things.

First classify the failure.

```text
Scope / target?
Recon?
Module selection?
Module requirements?
Configuration?
Compatibility?
Exploit?
Payload?
Handler?
Network?
Session?
Privilege?
Database?
Automation?
Environment?
```

Then ask:

```text
What evidence supports this classification?
```

Use the troubleshooting workflow:

```text
FAILURE
  ↓
DESCRIBE PRECISELY
  ↓
CLASSIFY
  ↓
FORM HYPOTHESIS
  ↓
CHANGE ONE THING
  ↓
RETEST
  ↓
OBSERVE
  ↓
CONCLUDE
```

### Example

Bad troubleshooting:

```text
Change payload
Change port
Change LHOST
Change target
Change module
Run again
```

Good troubleshooting:

```text
Hypothesis:
The reverse connection cannot reach my listener.

Change:
Use the correct reachable callback address.

Retest.

Observation:
Connection succeeds / still fails.

Conclusion:
Update the next hypothesis.
```

## Branch 13 — If a Session Appears

A session is not the final objective.

Think:

```text
SESSION
  ↓
IDENTIFY CONTEXT
  ↓
IDENTIFY PRIVILEGE
  ↓
DEFINE POST-EXPLOITATION OBJECTIVE
  ↓
COLLECT REQUIRED EVIDENCE
  ↓
STOP WHEN OBJECTIVE IS SATISFIED
```

First determine:

```text
Which session?
Which host?
Which user?
Which privilege level?
Which operating system?
How stable is the session?
```

Then ask:

```text
Why am I using this session?
```

Do not explore randomly.

## Branch 14 — Post-Exploitation

Use objective-driven post-exploitation.

Examples:

```text
Objective:
Demonstrate current user context.

Action:
Collect identity evidence.

Objective:
Demonstrate privilege level.

Action:
Collect privilege evidence.

Objective:
Demonstrate access to a specific protected resource.

Action:
Collect only the evidence required.

Objective:
Determine whether persistence is possible.

Action:
Only perform explicitly authorized persistence testing.
```

Mental model:

```text
POST-EXPLOITATION
        =
WHAT MUST I PROVE?
```

Not:

```text
"What commands can I run?"
```

## Branch 15 — Did I Satisfy the Objective?

Ask explicitly:

```text
What was the original objective?
What evidence proves it?
Is that evidence sufficient?
Is any additional action necessary?
```

If yes:

```text
DOCUMENT
   ↓
CLEAN UP
   ↓
STOP
```

Do not continue simply because you still have access.

This is an important professional habit.

## Branch 16 — Evidence

Evidence should connect:

```text
Finding
  ↓
Action
  ↓
Observed Result
  ↓
Verification
  ↓
Impact
```

Capture only what is necessary and authorized.

Useful evidence may include:

* target identity
* service/version information
* relevant module information
* configuration used
* execution result
* session identifier
* identity/privilege evidence
* objective-specific proof
* timestamps
* relevant screenshots or terminal output

Avoid collecting unnecessary sensitive information.

## Branch 17 — Cleanup

Before ending the engagement step, ask:

```text
Did I create files?
Did I create users?
Did I modify configuration?
Did I create persistence?
Did I create temporary artifacts?
Did I launch processes that should be terminated?
Did I leave sessions or listeners running?
```

Then remove authorized test artifacts.

Cleanup is part of the workflow:

```text
EXECUTE
  ↓
VERIFY
  ↓
EVIDENCE
  ↓
CLEANUP
```

Not an optional final thought.

## Branch 18 — When to Switch Tools

Switch tools when the current tool no longer provides the most appropriate capability.

Examples:

```text
Need packet-level visibility?
→ Wireshark

Need complex web request manipulation?
→ Burp Suite

Need broad network discovery?
→ Nmap

Need web content enumeration?
→ Appropriate web enumeration tooling

Need vulnerability scanning?
→ Nessus/OpenVAS or another appropriate scanner

Need a capability Metasploit does not provide efficiently?
→ Use the appropriate specialized tool
```

The objective remains constant even when the tool changes.

```text
OBJECTIVE
    ↓
BEST AVAILABLE CAPABILITY
    ↓
APPROPRIATE TOOL
```

## Decision Table — What Should I Do Next?

| Current state           | Next question                                   | Typical next action                |
| ----------------------- | ----------------------------------------------- | ---------------------------------- |
| Scope unclear           | Am I authorized to perform this action?         | Clarify scope                      |
| Target unknown          | What system am I testing?                       | Discovery                          |
| Service unknown         | What is exposed?                                | Service enumeration                |
| Version unknown         | What software/version is present?               | Version enumeration                |
| Objective unclear       | What must I prove?                              | Define objective                   |
| No suitable module      | Does another tool provide the capability?       | Switch tools                       |
| Module found            | Why should this module apply?                   | Read module information            |
| Requirement missing     | What evidence is needed?                        | Gather information                 |
| Configuration uncertain | Why is this value being used?                   | Validate configuration             |
| Check fails             | Is the target actually incompatible?            | Diagnose                           |
| Exploit fails           | What exactly failed?                            | Classify failure                   |
| Session absent          | Did execution occur, and can the callback work? | Diagnose exploit/payload/network   |
| Session appears         | What context did I obtain?                      | Verify session                     |
| Objective incomplete    | What evidence is still missing?                 | Objective-driven post-exploitation |
| Objective complete      | What evidence proves it?                        | Document                           |
| Testing complete        | What artifacts remain?                          | Cleanup                            |
| Tool no longer fits     | What capability do I need now?                  | Change tool                        |

## Red Flags

Stop and reassess when you see any of these:

### Scope Red Flags

```text
Target identity is uncertain.
Target moved outside the agreed range.
Authorization is unclear.
Requested action is not explicitly permitted.
```

### Technical Red Flags

```text
Module does not match the observed service.
Version assumptions are unsupported.
Required options are guessed.
Payload architecture is unknown.
Callback path is uncertain.
Exploit impact is unclear.
```

### Operational Red Flags

```text
The target is unstable.
The action may cause denial of service.
You cannot explain what the next action will prove.
You are repeatedly changing configuration without evidence.
You are continuing after the objective has already been satisfied.
```

These are signals to stop guessing.

## Stop Conditions

A professional operator should know when to stop.

Stop when:

```text
The objective is satisfied.
```

Also stop when:

```text
Authorization becomes unclear.
```

```text
The expected impact becomes unsafe or unacceptable.
```

```text
Required evidence cannot be obtained without exceeding scope.
```

```text
The current approach repeatedly fails without a defensible new hypothesis.
```

```text
The target becomes unstable.
```

```text
Another tool is clearly more appropriate.
```

Stopping is not failure.

Uncontrolled activity after the objective is satisfied is not good methodology.

## Practical Exercise 1 — Simple Lab Target

### Objective

Practice the entire decision tree against an authorized vulnerable lab.

### Scenario

You have:

```text
Target:
An intentionally vulnerable virtual machine.

Authorization:
Full exploitation is allowed inside the lab.

Objective:
Validate one discovered vulnerability and demonstrate its impact safely.
```

### Task

Without starting with a known exploit:

1. Confirm scope.
2. Identify the target.
3. Enumerate relevant services.
4. Define the exact objective.
5. Identify the information required.
6. Determine whether Metasploit is appropriate.
7. Search for relevant functionality.
8. Read the candidate module.
9. Verify requirements.
10. Configure it.
11. Validate where possible.
12. Execute.
13. Interpret the result.
14. Verify the result independently.
15. Collect objective-specific evidence.
16. Clean up.
17. Document the workflow.

### Success Criteria

You can explain:

```text
Why you selected the module.
Why the configuration was appropriate.
What the execution proved.
What it did not prove.
How you verified success.
Why you stopped.
```

## Practical Exercise 2 — Ambiguous Target

### Scenario

You receive:

```text
Target:
192.0.2.60

Objective:
"See whether the server is vulnerable."
```

### Task

Do not immediately search for exploits.

Identify what is missing.

Your questions should include:

```text
Which service?
Which port?
Which application?
Which version?
Which vulnerability?
What does "vulnerable" mean for this engagement?
What evidence is required?
```

### Success Criteria

You recognize that:

```text
"Test the server"
```

is not a sufficiently precise exploitation objective.

You convert ambiguity into measurable questions.

## Practical Exercise 3 — No Matching Module

### Scenario

You identify a vulnerability but cannot find an appropriate Metasploit module.

### Task

Determine:

```text
Is the vulnerability correctly identified?
Is the module search sufficiently broad?
Does Metasploit actually provide the required capability?
Would another tool validate it more appropriately?
```

### Success Criteria

You do not force Metasploit into the workflow merely because the repository is about Metasploit.

## Practical Exercise 4 — Exploit Succeeds but No Session Appears

### Scenario

The console reports successful exploitation, but no expected session is available.

### Task

Use the troubleshooting decision system.

Investigate systematically:

```text
Did execution actually occur?
Was the selected payload compatible?
Can the target reach the handler?
Is the callback address correct?
Is the callback port reachable?
Is the session being created and immediately dying?
Is the target context different from the assumption?
```

### Rule

Change one meaningful assumption at a time.

Do not randomly rotate payloads and network settings.

## Practical Exercise 5 — Objective Satisfied Early

### Scenario

Your objective is:

```text
Demonstrate that the target is executing code with the expected privilege level.
```

You obtain a verified session and collect sufficient identity and privilege evidence.

### Task

Decide what happens next.

The correct workflow is:

```text
Verify evidence
    ↓
Document
    ↓
Cleanup
    ↓
Stop
```

Do not perform unrelated post-exploitation simply because access exists.

## Practical Exercise 6 — Metasploit or Another Tool?

### Scenario

You need to inspect and manipulate a complex web request to determine whether an application is vulnerable.

### Task

Ask:

```text
What capability is required?
Does Metasploit provide that capability efficiently?
Would Burp Suite provide better visibility and control?
```

The exercise is not about choosing Metasploit.

It is about choosing the appropriate capability.

## Operator Decision Card

Use this compact version during practical work:

```text
1. SCOPE
   Am I authorized to do this?

2. TARGET
   What exactly am I testing?

3. OBJECTIVE
   What must I prove?

4. INFORMATION
   What do I know?
   What is missing?

5. CAPABILITY
   What technical capability do I need?

6. TOOL
   Is Metasploit the right tool?

7. MODULE
   Why does this module apply?

8. REQUIREMENTS
   What must be true before execution?

9. CONFIGURATION
   Why is each important option set this way?

10. VALIDATION
    Can I validate my assumptions first?

11. EXECUTION
    What controlled action am I taking?

12. RESULT
    What actually happened?

13. VERIFICATION
    What proves the result?

14. SESSION
    What capability did I obtain?

15. POST-EXPLOITATION
    What evidence is still required?

16. STOP
    Is the objective satisfied?

17. DOCUMENT
    Can another operator reproduce my reasoning?

18. CLEANUP
    Did I remove authorized test artifacts?
```

## Professional Operator Workflow

A mature Metasploit workflow looks like this:

```text
UNDERSTAND
    ↓
QUESTION
    ↓
ENUMERATE
    ↓
FORM HYPOTHESIS
    ↓
SELECT CAPABILITY
    ↓
READ
    ↓
VALIDATE
    ↓
EXECUTE
    ↓
VERIFY
    ↓
INTERPRET
    ↓
COLLECT EVIDENCE
    ↓
STOP OR DEFINE NEXT OBJECTIVE
    ↓
CLEAN UP
```

Notice what is missing:

```text
Random exploit selection
Random payload rotation
Blind command copying
Unnecessary post-exploitation
Continuing after success
```

The operator controls the tool.

The tool does not control the operator.

## Common Mistakes

### Starting With the Exploit

```text
"I know this is a vulnerable machine, so I will search for exploits."
```

Better:

```text
Understand the target → define the objective → identify the capability.
```

### Treating Module Names as Proof

A module with a matching-looking name is not evidence that it applies.

### Treating `check` as Absolute Truth

A check result is one piece of evidence.

### Treating a Session as the Objective

A session is a capability.

The objective determines what you do with it.

### Troubleshooting by Random Changes

Random changes destroy your ability to understand the failure.

### Continuing After Success

If the objective is satisfied, stop.

### Using Metasploit for Everything

A strong operator knows when another tool provides better visibility, control, or evidence.

## Completion Checklist

Before considering this decision system mastered, you should be able to:

* [ ] Start from scope rather than from an exploit.
* [ ] Build a target model from evidence.
* [ ] Define a measurable objective.
* [ ] Identify missing information.
* [ ] Decide whether Metasploit is appropriate.
* [ ] Find relevant capabilities.
* [ ] Read modules before using them.
* [ ] Identify module requirements.
* [ ] Configure deliberately.
* [ ] Validate assumptions.
* [ ] Execute in a controlled manner.
* [ ] Interpret results correctly.
* [ ] Distinguish output from verified evidence.
* [ ] Diagnose failed exploitation systematically.
* [ ] Manage sessions by objective.
* [ ] Perform focused post-exploitation.
* [ ] Collect sufficient evidence.
* [ ] Recognize when the objective is complete.
* [ ] Know when to switch tools.
* [ ] Clean up after testing.
* [ ] Document the reasoning behind your actions.

## Key Mental Model

```text
DO NOT ASK:

"What Metasploit command should I run?"

ASK:

"What is my objective?"

        ↓

"What do I know?"

        ↓

"What am I missing?"

        ↓

"What capability do I need?"

        ↓

"Is Metasploit the right tool?"

        ↓

"Why does this module apply?"

        ↓

"What must be true before execution?"

        ↓

"What did the result actually prove?"

        ↓

"What evidence is still required?"

        ↓

"Is the objective satisfied?"

        ↓

"Should I stop, troubleshoot, or change tools?"
```

This is the operator mindset the rest of the repository is designed to build.

## Next Step

Continue to:

`10-decision-guides/02-metasploit-or-another-tool.md`

That file turns the final tool-selection question into a practical decision framework for choosing Metasploit versus Nmap, Burp Suite, Wireshark, vulnerability scanners, web enumeration tools, and other specialized tooling.
