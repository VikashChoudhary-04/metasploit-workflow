# Guided Metasploit Labs

## Objective

Convert the concepts from the previous sections into repeatable practical skill.

These labs are intentionally guided.

At this stage, the goal is not to remove instructions completely. The goal is to build the correct operator habits before the guidance is gradually reduced.

The progression is:

```text
READ
  ↓
UNDERSTAND
  ↓
PERFORM
  ↓
EXPLAIN
  ↓
REPEAT
  ↓
MAKE DECISIONS
```

Every lab should be performed only against an authorized intentionally vulnerable target or an explicitly authorized assessment environment.

## Lab Philosophy

These are not command-copying exercises.

For every significant action, you should be able to answer:

```text
Why am I doing this?
What information supports it?
What should I expect?
How will I know whether it worked?
What will I do if it fails?
```

The labs therefore emphasize:

* target understanding
* objective definition
* module selection
* module reading
* configuration
* validation
* controlled exploitation
* result interpretation
* session management
* evidence collection
* troubleshooting
* cleanup
* tool selection

## Recommended Lab Environment

Use intentionally vulnerable systems that you control or are explicitly authorized to test.

Examples include:

* Metasploitable
* OWASP Juice Shop where relevant to the exercise
* DVWA where relevant to the exercise
* WebGoat where relevant to the exercise
* A deliberately vulnerable Windows or Linux virtual machine
* A private isolated lab network

Keep vulnerable systems isolated from networks where unintended access could occur.

A simple lab topology can be:

```text
┌──────────────────────────────┐
│        ATTACK MACHINE        │
│                              │
│       Metasploit             │
│       Nmap                   │
│       Supporting tools       │
└──────────────┬───────────────┘
               │
          Isolated Lab
             Network
               │
               ▼
┌──────────────────────────────┐
│       VULNERABLE TARGET      │
│                              │
│  Intentionally vulnerable   │
│  Lab VM / Application        │
└──────────────────────────────┘
```

## Lab Rules

Before every exercise:

```text
[ ] Confirm target identity
[ ] Confirm authorization
[ ] Confirm network isolation
[ ] Confirm objective
[ ] Confirm allowed actions
[ ] Know how to stop the test
```

During the exercise:

```text
[ ] Do not blindly copy commands
[ ] Record important observations
[ ] Change one assumption at a time when troubleshooting
[ ] Verify results
[ ] Avoid unnecessary actions
```

After the exercise:

```text
[ ] Collect required evidence
[ ] Close unnecessary sessions
[ ] Remove authorized test artifacts
[ ] Record what worked
[ ] Record what failed
[ ] Record what you learned
```

## Lab 1 — Metasploit Orientation

### Objective

Become comfortable with the Metasploit console without performing exploitation.

### Scenario

You have a local authorized lab environment.

Your first task is to understand the interface and workflow.

### Task

Open Metasploit and practice:

```text
Help
Search
Module information
Module selection
Option inspection
Option configuration
Returning to the previous context
Session listing
```

Do not attempt exploitation yet.

### Questions to Answer

```text
What is the current Metasploit context?

How do you determine which module is active?

How do you inspect module information?

How do you identify required options?

How do you return to the previous context?

How do you identify existing sessions?
```

### Success Criteria

You can navigate the console without relying on a command list.

The important skill is understanding the state of the interface.

## Lab 2 — Search Before Selection

### Objective

Learn to search for capabilities instead of guessing module names.

### Scenario

Your lab target exposes a service that you have identified through authorized enumeration.

You have:

```text
Target:
<lab target>

Port:
<observed port>

Service:
<observed service>

Version:
<observed version>
```

### Task

Use Metasploit's search functionality to investigate possible modules.

For each candidate, determine:

```text
What does the module do?
What target does it support?
What references exist?
What options are required?
What assumptions does the module make?
```

Do not immediately run the first result.

### Success Criteria

You can explain why one candidate is more relevant than another without saying:

```text
"It was the first result."
```

## Lab 3 — Read a Module

### Objective

Build the habit of reading a module before execution.

### Task

Choose an appropriate authorized lab module and inspect its information.

Create a small module analysis:

```text
Module:
<module path>

Purpose:
<what it does>

Target:
<what it applies to>

Required options:
<list>

Optional options:
<list>

Payload considerations:
<notes>

Expected result:
<what success should look like>

Potential failure points:
<what could prevent success>
```

### Questions

```text
Why does this module apply?

What evidence supports the selection?

What assumptions does the module make?

Which option is most important to verify?

What would make this module inappropriate?
```

### Success Criteria

You understand the module before configuring it.

## Lab 4 — Build a Requirement Map

### Objective

Learn to identify prerequisites before exploitation.

### Scenario

You have selected a module for an authorized vulnerable target.

### Task

Create this table:

| Requirement                   | Known? | Evidence | Action if unknown |
| ----------------------------- | ------ | -------- | ----------------- |
| Target reachable              |        |          |                   |
| Relevant port open            |        |          |                   |
| Service identified            |        |          |                   |
| Version compatible            |        |          |                   |
| Target platform compatible    |        |          |                   |
| Required module options known |        |          |                   |
| Payload compatible            |        |          |                   |
| Callback path valid           |        |          |                   |

Do not execute until you can explain the important unknowns.

### Success Criteria

You can distinguish:

```text
Known fact
```

from:

```text
Assumption
```

This distinction becomes increasingly important in later labs.

## Lab 5 — Configure Deliberately

### Objective

Learn to configure modules based on evidence.

### Task

Take the module from Lab 4.

For every important option, write:

```text
Option:
<name>

Value:
<value>

Why:
<reason>

Evidence:
<what supports the value>
```

Example structure:

```text
RHOSTS
Value: <authorized lab target>
Why: identifies the target being tested
Evidence: lab scope
```

Do the same for other important options.

### Success Criteria

You can explain every important configuration value.

## Lab 6 — Validation Before Exploitation

### Objective

Learn to distinguish validation from exploitation.

### Task

For a suitable authorized lab module:

1. Inspect whether a validation mechanism is available.
2. Understand what the validation mechanism actually tests.
3. Run it where appropriate.
4. Record the result.
5. Determine what the result does and does not prove.

Create:

```text
Validation result:
<result>

What it supports:
<interpretation>

What it does not prove:
<limitation>

Next action:
<decision>
```

### Critical Rule

Never turn:

```text
CHECK RESULT
```

into:

```text
CERTAINTY
```

without understanding what was actually checked.

## Lab 7 — First Controlled Exploitation

### Objective

Perform a complete exploitation workflow against an intentionally vulnerable lab target.

### Scenario

You have:

```text
Known target
Known service
Known vulnerability
Appropriate module
Known requirements
Authorized exploitation
```

### Workflow

```text
SCOPE
  ↓
TARGET
  ↓
OBJECTIVE
  ↓
MODULE
  ↓
READ
  ↓
REQUIREMENTS
  ↓
CONFIGURE
  ↓
VALIDATE
  ↓
EXECUTE
  ↓
VERIFY
  ↓
EVIDENCE
  ↓
CLEANUP
```

### Task

Complete the workflow.

Do not skip the reasoning steps because the target is intentionally vulnerable.

### Success Criteria

You can explain:

```text
Why this module?
Why this configuration?
Why this payload?
What did execution prove?
How did you verify success?
What evidence did you collect?
What did you clean up?
```

## Lab 8 — Payload Selection

### Objective

Understand payload selection as a compatibility decision.

### Scenario

The exploit module supports multiple payloads.

### Task

Before selecting one, identify:

```text
Target operating system
Target architecture
Required session type
Network direction
Callback requirements
Module compatibility
Lab network topology
```

Then choose a payload appropriate to the objective.

### Decision Model

```text
TARGET
  ↓
PLATFORM
  ↓
ARCHITECTURE
  ↓
SESSION REQUIREMENT
  ↓
NETWORK PATH
  ↓
PAYLOAD COMPATIBILITY
  ↓
PAYLOAD
```

### Success Criteria

You can explain why the selected payload fits the target and objective.

Not:

```text
"I always use this payload."
```

## Lab 9 — Handler and Callback Reasoning

### Objective

Understand why a payload may execute without producing the expected session.

### Scenario

An authorized exploit requires a reverse connection.

### Task

Map the communication path:

```text
TARGET
   │
   │ reverse connection
   ▼
YOUR LISTENER
   │
   ▼
SESSION
```

Then answer:

```text
Which address should the target reach?
Which port should it reach?
Is the route valid?
Is the address reachable from the target?
Is the listener actually available?
```

### Success Criteria

You understand the difference between:

```text
Exploit execution
```

and:

```text
Session establishment
```

## Lab 10 — Session Management

### Objective

Learn to manage multiple sessions deliberately.

### Scenario

Your authorized lab contains multiple vulnerable targets or multiple sessions.

### Task

Practice:

```text
List sessions
Identify a session
Interact with a session
Background a session
Return to another session
Determine session context
Close sessions when no longer needed
```

Create a session table:

| Session | Host | User/context | Purpose | Status |
| ------- | ---- | ------------ | ------- | ------ |
|         |      |              |         |        |
|         |      |              |         |        |

### Questions

```text
Which session belongs to which target?

Which session is relevant to the current objective?

Which sessions are no longer needed?

What evidence confirms the session identity?
```

### Success Criteria

You never lose track of which session you are operating.

## Lab 11 — Meterpreter by Objective

### Objective

Stop thinking of Meterpreter as a command list.

### Scenario

You have an authorized session.

Complete several objective-driven tasks.

Examples:

```text
Objective A:
Identify the current context.

Objective B:
Determine the system identity.

Objective C:
Determine the privilege level.

Objective D:
Collect one piece of evidence required by the lab.

Objective E:
Terminate or background the session appropriately.
```

For each task record:

```text
Objective:
<what you needed>

Capability:
<what Meterpreter capability was relevant>

Action:
<what you performed>

Evidence:
<what you observed>

Conclusion:
<what the evidence means>
```

### Success Criteria

You select actions because of the objective.

## Lab 12 — Post-Exploitation With a Purpose

### Objective

Practice controlled post-exploitation.

### Scenario

You have a verified authorized session.

Your objective is:

```text
Determine the security context obtained after exploitation.
```

### Task

Collect only the evidence required to establish:

```text
Host identity
Current user/context
Privilege level
Relevant system information
```

Do not perform unrelated enumeration merely because the session permits it.

### Success Criteria

You can stop once the objective is proven.

## Lab 13 — Evidence Collection

### Objective

Learn to create evidence that another person can understand.

### Task

Create an evidence record:

```text
Target:
<lab target>

Objective:
<objective>

Finding:
<finding>

Module:
<module>

Configuration:
<important configuration>

Execution:
<summary>

Result:
<observed result>

Verification:
<verification evidence>

Impact:
<what the evidence demonstrates>

Cleanup:
<actions taken>
```

### Success Criteria

Another operator should be able to understand what happened without watching your terminal.

## Lab 14 — Cleanup

### Objective

Build cleanup into the workflow.

### Task

After completing an authorized exploitation exercise, identify:

```text
Sessions
Listeners
Temporary files
Temporary accounts
Configuration changes
Processes
Persistence mechanisms
Other test artifacts
```

Remove only artifacts created as part of the authorized test.

Record:

```text
Artifact:
<what existed>

Action:
<what was done>

Result:
<cleanup result>
```

### Success Criteria

The lab is left in the intended state.

## Lab 15 — Guided Troubleshooting

### Objective

Apply the troubleshooting decision system to a controlled failure.

### Scenario

Your authorized lab workflow fails to produce the expected result.

You know:

```text
Target is reachable.
Relevant service exists.
Module appears relevant.
```

But execution does not produce the expected result.

### Task

Do not immediately change everything.

Follow:

```text
OBSERVE
  ↓
DESCRIBE FAILURE
  ↓
CLASSIFY
  ↓
HYPOTHESIS
  ↓
CHANGE ONE THING
  ↓
RETEST
  ↓
OBSERVE
```

Record:

```text
Initial failure:
<precise description>

Classification:
<failure category>

Hypothesis:
<what you think is wrong>

Change:
<one change>

Retest result:
<observation>

Conclusion:
<what you learned>

Next hypothesis:
<if required>
```

### Success Criteria

Your troubleshooting log demonstrates reasoning rather than random experimentation.

## Lab 16 — Failure Classification Drill

### Objective

Become faster at identifying where a workflow is failing.

For each scenario, identify the most likely branch before changing anything.

### Scenario A

```text
The target IP is wrong.
```

Classification:

```text
Scope / target
```

### Scenario B

```text
The module requires a target option that has not been provided.
```

Classification:

```text
Module requirement / configuration
```

### Scenario C

```text
The target service does not match the module's supported service.
```

Classification:

```text
Target compatibility / module selection
```

### Scenario D

```text
The exploit appears to execute, but the expected callback never reaches the listener.
```

Potential branches:

```text
Payload
Handler
Network
Configuration
```

### Scenario E

```text
A session appears but immediately terminates.
```

Potential branches:

```text
Session stability
Payload compatibility
Target conditions
Network
```

The purpose is not to guess the exact cause.

The purpose is to identify the correct investigation branch.

## Lab 17 — Use the Database

### Objective

Understand how Metasploit's database can support an engagement workflow.

### Task

In an authorized lab:

1. Create or select an appropriate workspace.
2. Import or record relevant target information.
3. Review hosts.
4. Review services.
5. Record relevant findings.
6. Inspect how information is associated with the engagement.
7. Keep separate lab engagements separated.

### Questions

```text
What information is stored?

What information came from enumeration?

What information came from exploitation?

Which information is current?

Which information has been verified?
```

### Success Criteria

You understand that:

```text
DATABASE RECORD
    ≠
CURRENT TRUTH
    ≠
VALIDATED VULNERABILITY
    ≠
SUCCESSFUL EXPLOITATION
```

## Lab 18 — Resource Script

### Objective

Practice controlled repetition.

### Scenario

You have several authorized lab targets that require the same safe initial workflow.

### Task

Identify which steps are suitable for automation.

Good candidates:

```text
Repeated setup
Repeated module configuration
Repeated information collection
Predictable non-destructive actions
```

Poor candidates:

```text
Judgment-heavy decisions
Ambiguous target selection
Unverified exploitation
High-impact actions
Final conclusions
```

### Principle

```text
AUTOMATE REPETITION.
DO NOT AUTOMATE AWAY JUDGMENT.
```

## Lab 19 — Tool Selection

### Objective

Practice deciding whether Metasploit is the correct tool.

### Scenario

You are given these objectives:

```text
A. Identify open ports.

B. Inspect and modify an HTTP request.

C. Analyze packet-level traffic.

D. Validate a known supported vulnerability.

E. Manage an authorized Meterpreter session.
```

### Task

For each objective, write:

```text
Objective:
<task>

Required capability:
<capability>

Preferred tool:
<tool>

Why:
<reason>
```

The goal is not to memorize a universal answer.

The goal is to justify tool selection.

## Lab 20 — End-to-End Guided Assessment

### Objective

Combine the entire repository into one guided workflow.

### Scenario

You receive an authorized intentionally vulnerable target.

You are given:

```text
Target:
<lab target>

Scope:
The specified lab target only.

Objective:
Identify one exploitable vulnerability and demonstrate its authorized impact.

Rules:
Do not attack other systems.
Do not perform unnecessary destructive actions.
Collect sufficient evidence.
Clean up after testing.
```

### Workflow

#### Phase 1 — Scope

Confirm:

```text
Target
Network
Allowed actions
Objective
```

#### Phase 2 — Reconnaissance

Determine:

```text
Open ports
Services
Versions
Relevant technologies
```

#### Phase 3 — Hypothesis

Write:

```text
I suspect __________ because __________.
```

#### Phase 4 — Capability

Determine:

```text
What capability is needed?
Is Metasploit appropriate?
```

#### Phase 5 — Module

Identify:

```text
Candidate module
Module purpose
Requirements
Compatibility
Payload considerations
```

#### Phase 6 — Configuration

Configure only what is required.

#### Phase 7 — Validation

Validate assumptions where possible.

#### Phase 8 — Execution

Perform controlled exploitation.

#### Phase 9 — Verification

Determine:

```text
Did exploitation actually succeed?
What evidence proves it?
What privilege/context was obtained?
```

#### Phase 10 — Post-Exploitation

Perform only actions necessary to satisfy the objective.

#### Phase 11 — Evidence

Record:

```text
Finding
Module
Configuration
Result
Verification
Impact
```

#### Phase 12 — Cleanup

Remove authorized test artifacts.

### Success Criteria

You can complete the assessment without needing a command-by-command recipe.

At this stage, the guidance is still present, but the decisions increasingly belong to you.

## Guided Lab Reporting Template

For every lab, use:

```text
# Lab Report

## Objective

<what I needed to prove>

## Scope

<authorized target and limits>

## Initial Knowledge

<what was known before testing>

## Missing Information

<what needed to be discovered>

## Reconnaissance

<important findings>

## Hypothesis

<what I believed and why>

## Tool Selection

<why the selected tool was appropriate>

## Module / Capability

<module or capability used>

## Requirements

<important requirements>

## Configuration

<important settings and reasons>

## Validation

<validation performed>

## Execution

<what happened>

## Result

<observed result>

## Verification

<evidence confirming or disproving the result>

## Post-Exploitation

<only objective-relevant actions>

## Evidence

<evidence collected>

## Troubleshooting

<failures and reasoning>

## Cleanup

<cleanup performed>

## Lessons Learned

<what changed in my understanding>
```

## Common Mistakes

### Copying Commands Without Understanding

If you cannot explain why a command is being used, stop and understand it.

### Starting With Exploitation

Reconnaissance and target understanding exist for a reason.

### Ignoring Module Requirements

A module is not a magic button.

### Treating Sessions as Success

A session is evidence of a capability, not automatically proof that the engagement objective is complete.

### Collecting Everything

Post-exploitation should be objective-driven.

### Skipping Cleanup

A successful exploit followed by poor cleanup is still an incomplete workflow.

### Troubleshooting Randomly

Use one hypothesis and one meaningful change at a time.

### Using Metasploit for Everything

Choose tools based on capability.

## Progression Rule

These guided labs should be completed in order.

The intended progression is:

```text
LAB 1–5
Interface + module fundamentals

LAB 6–10
Validation + exploitation + sessions

LAB 11–15
Meterpreter + post-exploitation + evidence + troubleshooting

LAB 16–19
Decision-making + database + automation + tool selection

LAB 20
End-to-end workflow
```

Do not rush through the early labs.

The later labs assume that the earlier reasoning patterns are automatic.

## Completion Checklist

Before moving to scenario-based labs, verify that you can:

* [ ] Establish scope before testing.
* [ ] Build a target model.
* [ ] Define a measurable objective.
* [ ] Identify missing information.
* [ ] Search for relevant capabilities.
* [ ] Read a module before using it.
* [ ] Build a requirement map.
* [ ] Configure deliberately.
* [ ] Validate assumptions.
* [ ] Select compatible payloads.
* [ ] Understand callback requirements.
* [ ] Execute controlled exploitation.
* [ ] Verify results.
* [ ] Manage sessions.
* [ ] Use Meterpreter by objective.
* [ ] Perform focused post-exploitation.
* [ ] Collect evidence.
* [ ] Troubleshoot systematically.
* [ ] Use the database appropriately.
* [ ] Automate repeatable work carefully.
* [ ] Select the appropriate tool.
* [ ] Clean up after testing.
* [ ] Explain your reasoning.

## Key Mental Model

```text
GUIDED LABS ARE NOT ABOUT MEMORIZING THE ANSWER.

They are about practicing the decision process
until the decision process becomes natural.
```

The progression should feel like:

```text
"I need to be told what to do."
          ↓
"I know the workflow."
          ↓
"I know what information I need."
          ↓
"I know what capability I need."
          ↓
"I can choose the tool."
          ↓
"I can troubleshoot failures."
          ↓
"I can complete the objective independently."
```

## Next Step

Continue to:

`11-practical-labs/02-scenario-labs.md`

The next stage removes much of the procedural guidance and introduces realistic scenarios where you must decide what to investigate, which capability to use, how to validate your assumptions, and when to stop.
