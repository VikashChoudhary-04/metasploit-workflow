# Metasploit Workflow

A practical, decision-oriented workflow for learning and mastering **Metasploit Framework** from absolute beginner to independent penetration-testing operator.

> **Core objective:** Learn how to determine what to do next with Metasploit when given an unfamiliar authorized target and objective — without depending on a step-by-step tutorial.

## What This Repository Is

This repository is **not a Metasploit command dictionary**.

It teaches a repeatable operational workflow:

```text
SCOPE
  ↓
UNDERSTAND TARGET
  ↓
DEFINE OBJECTIVE
  ↓
IDENTIFY MISSING INFORMATION
  ↓
FIND RELEVANT FUNCTIONALITY
  ↓
READ THE MODULE
  ↓
CHECK REQUIREMENTS
  ↓
CONFIGURE
  ↓
VALIDATE
  ↓
RUN
  ↓
INTERPRET RESULT
  ↓
SUCCESS? ── YES → VERIFY → NEXT OBJECTIVE
    │
    NO
    ↓
DIAGNOSE
    ↓
CHANGE ONE ASSUMPTION
    ↓
RETEST / CHANGE APPROACH
  ↓
DOCUMENT
  ↓
CLEAN UP
```

The goal is to make this workflow natural enough that the learner can eventually perform it without opening this repository.

## Target Learner

This repository assumes the learner:

* Has never used Metasploit.
* May know basic Linux commands.
* Understands basic concepts such as IP addresses and network ports.
* Does not know Metasploit terminology.
* Does not know how Metasploit modules work.
* Does not know how to select payloads.
* Does not know how sessions work.
* Does not know how to troubleshoot Metasploit.

It does **not** require previous Metasploit experience.

## Learning Philosophy

The repository follows an approximately:

**80–90% practical / 10–20% essential theory**

approach.

Every important concept should answer:

```text
WHAT?
  ↓
WHY?
  ↓
HOW?
  ↓
VERIFY
  ↓
FAILURE
  ↓
RECOVER
  ↓
NEXT
```

Theory is included only when it improves the learner's ability to:

* Operate Metasploit.
* Make decisions.
* Interpret results.
* Troubleshoot failures.
* Understand important limitations.
* Work safely and methodically.

## Core Mental Models

### Search → Read → Configure → Validate → Run → Verify → Next

```text
SEARCH
  ↓
READ
  ↓
CONFIGURE
  ↓
VALIDATE
  ↓
RUN
  ↓
VERIFY
  ↓
NEXT
```

### Failure → Diagnose → Change One Thing → Retest

```text
FAILURE
  ↓
IDENTIFY WHAT FAILED
  ↓
CHECK THE RELEVANT ASSUMPTION
  ↓
CHANGE ONE THING
  ↓
RETEST
```

### No Result ≠ No Vulnerability

```text
NO RESULT
   ≠
NO VULNERABILITY
```

A failure may result from:

* Incorrect target identification.
* Incorrect service or version assumptions.
* Incorrect module selection.
* Incorrect configuration.
* Incorrect target selection.
* Incompatible payload.
* Network connectivity.
* Handler configuration.
* Target protections.
* Environmental conditions.
* Module limitations.

## Universal Metasploit Workflow

The repository repeatedly uses this workflow:

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
SESSION / RESULT
  ↓
NEXT OBJECTIVE
  ↓
DOCUMENT
  ↓
CLEAN UP
```

When something fails:

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

## Repository Structure

```text
metasploit-workflow/
│
├── README.md
│
├── 01-foundations/
│   ├── 01-mental-model.md
│   └── 02-installation-and-first-run.md
│
├── 02-interface/
│   ├── 01-msfconsole-workflow.md
│   └── 02-search-read-configure.md
│
├── 03-modules/
│   ├── 01-module-system.md
│   ├── 02-module-selection.md
│   └── 03-module-validation.md
│
├── 04-payloads/
│   ├── 01-payload-mental-model.md
│   └── 02-payload-selection-and-handlers.md
│
├── 05-exploitation/
│   ├── 01-exploitation-workflow.md
│   └── 02-exploitation-validation.md
│
├── 06-sessions-meterpreter/
│   ├── 01-session-management.md
│   └── 02-meterpreter-by-objective.md
│
├── 07-post-exploitation/
│   ├── 01-post-exploitation-workflow.md
│   └── 02-evidence-and-cleanup.md
│
├── 08-database-automation/
│   ├── 01-database-and-workspaces.md
│   └── 02-resource-scripts-and-repeatability.md
│
├── 09-troubleshooting/
│   └── 01-troubleshooting-decision-system.md
│
├── 10-decision-guides/
│   ├── 01-operator-decision-tree.md
│   └── 02-metasploit-or-another-tool.md
│
├── 11-practical-labs/
│   ├── 01-guided-labs.md
│   ├── 02-scenario-labs.md
│   └── 03-independent-assessment.md
│
└── 12-final-challenge/
    └── README.md
```

## Learning Path

The repository progressively removes instructions.

### Stage 1 — Guided Learning

The learner receives:

```text
CONCEPT
  ↓
EXPLANATION
  ↓
COMMAND
  ↓
EXPECTED RESULT
```

The purpose is to establish correct fundamentals.

### Stage 2 — Workflow Learning

The learner receives:

```text
OBJECTIVE
  ↓
WORKFLOW
  ↓
LIMITED GUIDANCE
```

The learner begins making decisions.

### Stage 3 — Scenario Learning

The learner receives:

```text
TARGET
+
KNOWN INFORMATION
+
OBJECTIVE
```

The learner determines the appropriate workflow.

### Stage 4 — Troubleshooting

The learner receives a broken or unexpected situation and determines:

```text
WHAT FAILED?
  ↓
WHY?
  ↓
WHAT SHOULD I TEST?
  ↓
WHAT SHOULD I CHANGE?
```

### Stage 5 — Independent Operation

The learner receives only:

```text
SCOPE
+
TARGET
+
OBJECTIVE
+
RULES
+
EVIDENCE REQUIREMENTS
```

No step-by-step command sequence is provided.

## Curriculum

### 01 — Foundations

Learn:

* What Metasploit is.
* Where it fits in penetration testing.
* Core terminology.
* Framework architecture.
* Installation and verification.

### 02 — Interface

Learn to operate `msfconsole` comfortably.

Focus on:

* Navigation.
* Help.
* Searching.
* Module selection.
* Information.
* Options.
* Configuration.
* Execution.
* Sessions.
* Jobs.

### 03 — Modules

Learn how to approach an unfamiliar module:

```text
SEARCH
  ↓
IDENTIFY
  ↓
INFO
  ↓
REQUIREMENTS
  ↓
CONFIGURE
  ↓
VALIDATE
  ↓
RUN
```

### 04 — Payloads

Build a practical understanding of:

* Payloads.
* Staged and stageless payloads.
* Operating systems.
* Architectures.
* Transports.
* Reverse connections.
* Bind connections.
* LHOST and LPORT.
* Handlers.
* Payload compatibility.

The objective is to select payloads based on target conditions rather than memorized names.

### 05 — Exploitation

Learn controlled exploitation:

```text
IDENTIFY
  ↓
VALIDATE
  ↓
SELECT
  ↓
CONFIGURE
  ↓
CHECK
  ↓
EXECUTE
  ↓
VERIFY
```

Failure is treated as part of the workflow.

### 06 — Sessions and Meterpreter

Learn to:

* Identify sessions.
* Interact with sessions.
* Background sessions.
* Manage multiple sessions.
* Understand session stability.
* Use Meterpreter by objective rather than memorizing commands.

### 07 — Post-Exploitation

Learn how to reason after obtaining authorized access.

Focus on:

* System context.
* User context.
* Process awareness.
* Network awareness.
* Authorized post-exploitation.
* Evidence.
* Documentation.
* Cleanup.

### 08 — Database and Automation

Learn how to organize and repeat work using:

* Databases.
* Workspaces.
* Hosts.
* Services.
* Credentials.
* Vulnerabilities.
* Loot.
* Resource scripts.
* Controlled automation.

### 09 — Troubleshooting

Build a universal diagnostic process for:

* Module failures.
* Configuration problems.
* Target mismatches.
* Payload failures.
* Listener problems.
* Network problems.
* Session failures.
* Environmental restrictions.

### 10 — Decision Guides

Develop the ability to answer:

> **What should I do next?**

and:

> **Should I use Metasploit at all?**

### 11 — Practical Labs

Progress through:

```text
GUIDED
  ↓
SEMI-GUIDED
  ↓
SCENARIO-BASED
  ↓
INDEPENDENT
```

### 12 — Final Challenge

Complete an authorized assessment with:

* No command list.
* No module name.
* No payload name.
* No step-by-step instructions.

The learner must determine the workflow independently.

## Module Categories

The repository teaches module categories according to their operational purpose.

| Category  | Primary purpose                                             |
| --------- | ----------------------------------------------------------- |
| Auxiliary | Useful actions that do not necessarily involve exploitation |
| Exploit   | Attempt to leverage a vulnerability                         |
| Payload   | Define what executes after exploitation                     |
| Post      | Perform authorized actions after obtaining access           |
| Encoder   | Transform payload representations for supported use cases   |
| NOP       | Generate NOP-related payload components                     |
| Evasion   | Support specialized evasion-oriented testing                |

The curriculum prioritizes:

```text
AUXILIARY
EXPLOIT
PAYLOAD
POST
```

because these are most important to the core operator workflow.

## Metasploit and Other Tools

Metasploit is part of a broader penetration-testing toolkit.

| Objective                     | Possible tool                         |
| ----------------------------- | ------------------------------------- |
| Network and service discovery | Nmap                                  |
| Web application testing       | Burp Suite                            |
| Packet analysis               | Wireshark                             |
| Vulnerability assessment      | Nessus / OpenVAS                      |
| Exploit research              | Searchsploit / vendor advisories      |
| DNS reconnaissance            | DNS/recon tools                       |
| SMB-specific enumeration      | SMB tools                             |
| Controlled exploit validation | Metasploit                            |
| Session management            | Metasploit                            |
| Post-exploitation workflow    | Metasploit + objective-specific tools |

This repository does not attempt to replace those tools.

It teaches the learner to determine:

> **When should Metasploit be used?**

and:

> **When should another tool be used instead?**

## Lab Environment

Use only intentionally vulnerable or explicitly authorized environments.

Recommended practice targets include:

* Metasploitable.
* OWASP Juice Shop where applicable.
* DVWA where applicable.
* Other intentionally vulnerable local environments.
* Explicitly authorized penetration-testing environments.

Do **not** use this repository to test systems without authorization.

## Professional Operating Principles

Throughout the repository:

* Stay within scope.
* Confirm authorization.
* Minimize impact.
* Validate assumptions.
* Do not blindly exploit.
* Do not collect unnecessary sensitive information.
* Record what you actually observed.
* Distinguish discovery from validation.
* Treat failed attempts as information.
* Record exact configurations when relevant.
* Stop when the objective is satisfied.
* Clean up appropriately.
* Know when another tool is more appropriate.

## Completion Standard

You have completed this repository when you can receive:

```text
AUTHORIZED TARGET
+
OBJECTIVE
+
RULES OF ENGAGEMENT
```

and independently determine:

```text
What to investigate
        ↓
What to search
        ↓
Which module is relevant
        ↓
What the module requires
        ↓
How to configure it
        ↓
Which payload fits
        ↓
How to validate the result
        ↓
How to troubleshoot failure
        ↓
How to manage the resulting session
        ↓
What authorized post-exploitation action is appropriate
        ↓
What evidence matters
        ↓
How to document the result
        ↓
How to clean up
        ↓
What to do next
```

without following a step-by-step tutorial.

## Final Goal

The goal is not:

> **"I know Metasploit commands."**

The goal is:

> **"I understand Metasploit well enough that the interface feels familiar, the workflow feels natural, and I can independently determine what to do next when faced with an unfamiliar authorized situation."**

## Current Documentation

Use current official documentation when verifying framework behavior, commands, module functionality, or terminology:

* [Metasploit Documentation](https://docs.metasploit.com/)
* [Metasploit Framework](https://github.com/rapid7/metasploit-framework)

Concrete commands and module examples in this repository should be verified against current framework documentation before being treated as authoritative.
