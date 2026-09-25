# Metasploit Mental Model

Metasploit becomes much easier once you stop thinking of it as a collection of commands and start thinking of it as a **framework of capabilities**.

The purpose of this lesson is to build the minimum mental model required to operate Metasploit confidently.

## What Is Metasploit?

Metasploit Framework is a security-testing framework that provides reusable functionality for activities such as:

* Information gathering.
* Enumeration.
* Vulnerability validation.
* Exploitation.
* Payload handling.
* Session management.
* Authorized post-exploitation.

The most important idea is:

> **Metasploit provides reusable modules for specific security-testing tasks.**

Think of Metasploit as a **toolbox**.

The tools inside that toolbox are its modules.

## The Core Mental Model

Start with this:

```text
TARGET
  ↓
OBJECTIVE
  ↓
CAPABILITY
  ↓
MODULE
  ↓
CONFIGURATION
  ↓
RESULT
```

For example:

```text
Target:
An authorized Windows lab host

Objective:
Validate an identified vulnerability

Capability:
Exploitation

Module:
Relevant exploit module

Configuration:
Target + required options + target selection + payload

Result:
Validated / failed / inconclusive
```

The important point is:

> **The objective comes before the command.**

## Metasploit Is a Framework

Metasploit is not one giant exploit program.

It contains different types of functionality that solve different problems.

The most important categories are:

```text
                 METASPLOIT
                     │
        ┌────────────┼────────────┐
        │            │            │
    AUXILIARY     EXPLOIT       POST
        │            │            │
    information   execution   after-access
        │            │            │
        └────────────┼────────────┘
                     │
                  PAYLOAD
                     │
              what executes
             after exploitation
```

Other module categories also exist:

```text
ENCODER
NOP
EVASION
```

You will encounter those later, but they are not equally important to the initial operator workflow.

## The Core Terms

Before learning commands, understand these concepts:

```text
MODULE
OPTION
TARGET
PAYLOAD
SESSION
JOB
```

These concepts appear repeatedly throughout the repository.

## Modules

A **module** is a reusable piece of Metasploit functionality.

Mental model:

```text
MODULE
  ↓
"A specialized capability"
```

A module may perform tasks such as:

* Scan a service.
* Enumerate information.
* Test authentication.
* Validate a vulnerability.
* Attempt exploitation.
* Perform authorized post-exploitation.

When you select a module, your first question should be:

> **What does this module expect from me?**

That leads to:

```text
MODULE
  ↓
INFORMATION
  ↓
OPTIONS
  ↓
TARGETS
  ↓
PAYLOADS
  ↓
EXECUTION
```

## Module Categories

The categories should be understood by **purpose**, not memorized as definitions.

| Category  | Operational purpose                                         |
| --------- | ----------------------------------------------------------- |
| Auxiliary | Useful actions that do not necessarily involve exploitation |
| Exploit   | Attempts to leverage a vulnerability                        |
| Payload   | Defines what executes after successful exploitation         |
| Post      | Performs authorized actions after access                    |
| Encoder   | Transforms payload representations for supported use cases  |
| NOP       | Generates NOP-related payload components                    |
| Evasion   | Supports specialized evasion-oriented testing               |

The four categories you will use most heavily are:

```text
AUXILIARY
EXPLOIT
PAYLOAD
POST
```

## Auxiliary Modules

Auxiliary modules perform useful security-testing actions without necessarily exploiting a vulnerability.

Examples include:

* Service scanning.
* Enumeration.
* Protocol interaction.
* Information gathering.
* Authentication testing.

Mental model:

```text
"I need information or a testing action."
                ↓
            AUXILIARY
```

For example:

> Determine what information a service exposes.

This may be an auxiliary-module problem rather than an exploitation problem.

## Exploit Modules

Exploit modules attempt to leverage a vulnerability or weakness.

Mental model:

```text
"I have identified a potentially exploitable condition."
                         ↓
                      EXPLOIT
```

However:

```text
Potential vulnerability
        ≠
Successful exploitation
```

An exploit can fail because of:

* Incorrect target assumptions.
* Incorrect version information.
* Wrong target selection.
* Configuration problems.
* Incompatible payloads.
* Network conditions.
* Target protections.
* Environmental differences.
* Module limitations.

Therefore:

> **Selecting an exploit module does not prove that exploitation is appropriate or successful.**

## Payloads

A payload defines what should happen when code execution is obtained.

Mental model:

```text
EXPLOIT
  ↓
"How do I obtain execution?"
  ↓
PAYLOAD
  ↓
"What should execute?"
  ↓
RESULT / SESSION
```

Payload selection depends on conditions such as:

* Operating system.
* Architecture.
* Module compatibility.
* Communication path.
* Target connectivity.
* Desired session type.

Do not memorize payload names yet.

You will learn how to **select payloads logically** later.

## Sessions

A **session** is an established interaction resulting from successful exploitation or another mechanism that provides access.

Mental model:

```text
EXPLOIT
   ↓
PAYLOAD
   ↓
SESSION
```

For example:

```text
Exploit succeeds
       ↓
Payload executes
       ↓
Connection is established
       ↓
Metasploit receives a session
```

An exploit and a session are not the same thing.

This distinction is important when troubleshooting.

You can have:

```text
Exploit attempt
      ↓
Execution problem
      ↓
No session
```

or:

```text
Exploit succeeds
      ↓
Payload problem
      ↓
No session
```

Therefore:

> **"No session" tells you what happened, not necessarily why it happened.**

## Meterpreter

Meterpreter is a Metasploit payload/session environment that provides an interactive interface for authorized post-exploitation activities.

Think of it as:

```text
SESSION
   ↓
METERPRETER
   ↓
INTERACTIVE CAPABILITIES
```

You will learn Meterpreter by **objective**, not by memorizing a huge command list.

For example:

```text
Objective:
Understand the target system.

Question:
"What information do I need?"

Then:
"Which Meterpreter capability provides that information?"
```

## Jobs

A **job** represents work that Metasploit can continue performing in the background.

Mental model:

```text
TASK
  ↓
BACKGROUND EXECUTION
  ↓
JOB
```

Jobs become useful when a task needs to continue while you perform another operation in the console.

Do not confuse:

```text
JOB
```

with:

```text
SESSION
```

A session represents an established interaction.

A job represents an ongoing background task.

## Options

Modules have configurable values called **options**.

Mental model:

```text
MODULE
  ↓
"What information does this module need?"
  ↓
OPTIONS
```

Common examples include:

```text
RHOSTS
RPORT
LHOST
LPORT
TARGET
```

The exact options depend on the module.

Never assume that every module uses the same options.

## Required and Optional Options

Some options are required.

Others may have defaults or may only affect optional behavior.

Think:

```text
Required
└── Must be supplied or resolved

Optional
└── May have a useful default or may not be necessary
```

When examining a module, ask:

```text
Which options are required?
Which already have useful defaults?
Which options affect the result?
Which options depend on my environment?
```

Do not blindly configure every option you see.

## Targets

Some exploit modules support multiple target implementations.

A target represents a particular set of assumptions about how the exploit should interact with the target.

Mental model:

```text
EXPLOIT MODULE
      ↓
SUPPORTED TARGETS
      ↓
Which target matches the actual system?
```

Therefore:

> **Do not automatically choose the first target in the list.**

Read the module's target information and determine which target matches the actual environment.

## The Complete Exploitation Model

Put the major concepts together:

```text
TARGET
  ↓
VULNERABILITY / CONDITION
  ↓
EXPLOIT MODULE
  ↓
TARGET SELECTION
  ↓
PAYLOAD
  ↓
EXECUTION
  ↓
SESSION
```

This gives you the basic relationship:

```text
EXPLOIT
  ↓
PAYLOAD
  ↓
SESSION
```

while the module provides the mechanism and configuration needed to perform the operation.

## The Operator Workflow

The repository will repeatedly use this workflow:

```text
SCOPE
  ↓
UNDERSTAND TARGET
  ↓
DEFINE OBJECTIVE
  ↓
IDENTIFY MISSING INFORMATION
  ↓
SEARCH
  ↓
READ
  ↓
CHECK REQUIREMENTS
  ↓
CONFIGURE
  ↓
VALIDATE
  ↓
RUN
  ↓
INTERPRET
  ↓
VERIFY
  ↓
NEXT OBJECTIVE
```

If something fails:

```text
FAILURE
  ↓
IDENTIFY FAILURE LAYER
  ↓
CHECK THE RELEVANT ASSUMPTION
  ↓
CHANGE ONE THING
  ↓
RETEST
```

This workflow is more important than memorizing individual commands.

## Start With the Objective

Bad thinking:

```text
"I know an exploit command.
I'll run it."
```

Better thinking:

```text
What am I trying to prove?

What information do I have?

What information am I missing?

What Metasploit capability fits?

What does that capability require?

How will I verify the result?
```

The second approach works when the target and situation are unfamiliar.

## Read Before You Run

When you encounter an unfamiliar module, understand:

```text
What does it target?
What does it require?
What options matter?
What targets are supported?
What payloads are compatible?
Is a check available?
What result should I expect?
What limitations exist?
```

The module's documentation is part of the tool.

Treat it as operational information, not optional reading.

## Execution Is Not Validation

A command completing successfully does not automatically prove a security finding.

Separate these claims:

```text
COMMAND EXECUTED
```

from:

```text
SECURITY CONDITION VALIDATED
```

and:

```text
ACCESS OBTAINED
```

These are different outcomes.

Likewise:

```text
EXPLOIT FAILED
```

does not automatically mean:

```text
TARGET IS NOT VULNERABLE
```

Always interpret the result.

## Change One Assumption at a Time

When something fails, do not immediately change every setting.

For example:

```text
NO SESSION
   ↓
Is the target reachable?
   ↓
Is the service correct?
   ↓
Does the module match?
   ↓
Is the configuration correct?
   ↓
Is the payload compatible?
   ↓
Can the target reach the required listener?
```

Test the most likely failed assumption first.

Then retest.

This creates a useful diagnostic loop:

```text
FAILURE
  ↓
HYPOTHESIS
  ↓
ONE CHANGE
  ↓
RETEST
  ↓
COMPARE
```

## Example: Unknown Web Service

Suppose an authorized lab target exposes:

```text
Target:
10.10.10.20

Known:
TCP/80 open
HTTP service identified

Objective:
Determine whether Metasploit contains relevant testing functionality.
```

Do not immediately search for an exploit.

Think:

```text
What web server/product is running?
        ↓
What version?
        ↓
What behavior or vulnerability am I investigating?
        ↓
Does Metasploit contain relevant functionality?
        ↓
Which modules match?
        ↓
What does each module require?
        ↓
Can I validate safely before exploiting?
```

The exact commands come after the reasoning.

## Example: After Obtaining a Session

Suppose an authorized lab exercise gives you:

```text
Session 2
```

Do not immediately execute random post-exploitation commands.

Ask:

```text
What type of session is this?
        ↓
Who am I?
        ↓
What system am I on?
        ↓
What is my authorized objective?
        ↓
What information do I need?
        ↓
Which capability provides it?
        ↓
What evidence should I record?
        ↓
What should I do next?
```

Again:

> **Objective first. Capability second. Command third.**

## The Portable Mental Model

The entire repository can eventually be reduced to:

```text
OBJECTIVE
    ↓
INFORMATION
    ↓
CAPABILITY
    ↓
MODULE
    ↓
CONFIGURATION
    ↓
VALIDATION
    ↓
EXECUTION
    ↓
RESULT
    ↓
NEXT ACTION
```

For failures:

```text
FAILURE
    ↓
DIAGNOSE
    ↓
CHANGE ONE ASSUMPTION
    ↓
RETEST
```

## Before Moving On

You should be able to answer these questions without looking them up:

### Concepts

1. What is Metasploit?
2. What is a module?
3. What is an auxiliary module?
4. What is an exploit module?
5. What is a payload?
6. What is a session?
7. What is a job?
8. What is a module option?
9. What is a target?
10. What is Meterpreter?

### Decision-making

11. Why should you read an unfamiliar module before running it?
12. Why does selecting an exploit module not prove that exploitation will succeed?
13. Why does payload selection depend on target conditions?
14. Why should you change one important assumption at a time when troubleshooting?
15. Why should an operator start with an objective rather than a command?

### Workflow

You should be able to reconstruct:

```text
OBJECTIVE
→ TARGET INFORMATION
→ SEARCH
→ READ
→ REQUIREMENTS
→ CONFIGURE
→ VALIDATE
→ RUN
→ VERIFY
→ NEXT
```

## Key Takeaways

```text
MODULE
= A reusable Metasploit capability

EXPLOIT
= Attempts to leverage a vulnerability

PAYLOAD
= Defines what executes after exploitation

SESSION
= Established interaction resulting from successful access

JOB
= Background task

OPTIONS
= Module configuration values

TARGET
= A supported exploit target configuration

METERPRETER
= An interactive Metasploit session environment
```

Most importantly:

```text
OBJECTIVE
→ CAPABILITY
→ MODULE
→ CONFIGURATION
→ VALIDATION
→ RESULT
→ NEXT ACTION
```

That is the foundation of Metasploit proficiency.
