# Metasploit Workflow

A practical, decision-oriented workflow for learning and mastering **Metasploit Framework** from absolute beginner to independent penetration-testing operator.

> **Core objective:** Learn how to determine what to do next with Metasploit when given an unfamiliar authorized target and objective — without depending on a step-by-step tutorial.

---

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

---

## Who This Is For

This repository assumes the learner:

* has never used Metasploit;
* may know basic Linux commands;
* understands basic concepts such as IP addresses and network ports;
* does not know Metasploit terminology;
* does not know how Metasploit modules work;
* does not know how to select payloads;
* does not know how sessions work;
* does not know how to troubleshoot Metasploit.

It does **not** require previous Metasploit experience.

---

## Learning Philosophy

The repository follows an approximately:

**80–90% practical / 10–20% essential theory**

approach.

Every important concept is taught through:

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

* operate Metasploit;
* make decisions;
* interpret results;
* troubleshoot failures;
* understand important limitations;
* work safely and methodically.

---

## The Core Mental Models

### 1. The Metasploit Workflow

```text
SEARCH
  ↓
READ
  ↓
CONFIGURE
  ↓
CHECK
  ↓
RUN
  ↓
VERIFY
  ↓
NEXT
```

---

### 2. Failure Is Information

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

Do not respond to failure by randomly changing multiple settings.

---

### 3. No Result Does Not Automatically Mean No Vulnerability

```text
NO RESULT
   ≠
NO VULNERABILITY
```

A failure can originate from:

* incorrect target identification;
* incorrect service assumptions;
* incorrect module selection;
* incorrect configuration;
* incorrect target selection;
* incompatible payload;
* network connectivity;
* handler configuration;
* target protections;
* environmental conditions;
* module limitations.

---

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

---

## Learning Path

The repository progressively removes instructions.

### Stage 1 — Learn

You are given:

```text
Concept
↓
Explanation
↓
Command
↓
Expected result
```

### Stage 2 — Follow

You are given:

```text
Objective
↓
Workflow
↓
Limited command guidance
```

### Stage 3 — Decide

You are given:

```text
Target
+
Known information
+
Objective
```

You determine the appropriate Metasploit workflow.

### Stage 4 — Troubleshoot

You are given a broken or unexpected situation.

You determine:

```text
What failed?
↓
Why?
↓
What should I test?
↓
What should I change?
```

### Stage 5 — Operate

You are given:

```text
Scope
+
Target
+
Objective
+
Rules
```

No command sequence is provided.

You build the workflow yourself.

---

## Curriculum

### 01 — Foundations

Learn:

* What Metasploit is.
* Where it fits in penetration testing.
* Core terminology.
* Framework architecture.
* Installation and verification.

---

### 02 — Interface

Learn to operate `msfconsole` comfortably.

Focus on:

* navigation;
* help;
* searching;
* module selection;
* information;
* options;
* configuration;
* execution;
* sessions;
* jobs.

---

### 03 — Modules

Learn how to approach an unfamiliar module.

Focus on:

```text
SEARCH
→ IDENTIFY
→ INFO
→ REQUIREMENTS
→ CONFIGURE
→ VALIDATE
→ RUN
```

---

### 04 — Payloads

Build a practical understanding of:

* payloads;
* staged and stageless payloads;
* operating systems;
* architectures;
* transports;
* reverse connections;
* bind connections;
* LHOST/LPORT;
* handlers;
* payload compatibility.

The objective is to select payloads based on conditions rather than memorized names.

---

### 05 — Exploitation

Learn controlled exploitation:

```text
IDENTIFY
→ VALIDATE
→ SELECT
→ CONFIGURE
→ CHECK
→ EXECUTE
→ VERIFY
```

Failure becomes part of the learning process.

---

### 06 — Sessions & Meterpreter

Learn to:

* identify sessions;
* interact with sessions;
* background sessions;
* manage multiple sessions;
* understand session stability;
* use Meterpreter by objective rather than memorizing commands.

---

### 07 — Post-Exploitation

Learn how to reason after obtaining access.

Focus on:

* system context;
* user context;
* process awareness;
* network awareness;
* authorized post-exploitation;
* evidence;
* documentation;
* cleanup.

---

### 08 — Database & Automation

Learn how to organize and repeat work using:

* databases;
* workspaces;
* hosts;
* services;
* credentials;
* vulnerabilities;
* loot;
* resource scripts;
* controlled automation.

---

### 09 — Troubleshooting

Build a universal diagnostic process for:

* module failures;
* configuration problems;
* target mismatches;
* payload failures;
* listener problems;
* network problems;
* session failures;
* environmental restrictions.

---

### 10 — Decision Guides

Develop the ability to answer:

> **What should I do next?**

and:

> **Should I use Metasploit at all?**

---

### 11 — Practical Labs

Progress through:

```text
Guided
  ↓
Semi-guided
  ↓
Scenario-based
  ↓
Independent
```

---

### 12 — Final Challenge

Complete an authorized assessment with:

* no command list;
* no module name;
* no payload name;
* no step-by-step instructions.

The learner must determine the workflow independently.

---

## Universal Operator Decision Tree

When facing an unfamiliar target:

```text
1. IS THE ACTION AUTHORIZED?
        ↓
2. WHAT IS THE OBJECTIVE?
        ↓
3. WHAT DO I KNOW?
        ↓
4. WHAT INFORMATION IS MISSING?
        ↓
5. WHICH TOOL OR CAPABILITY FITS?
        ↓
6. SEARCH
        ↓
7. READ
        ↓
8. VALIDATE THE MODULE
        ↓
9. CONFIGURE
        ↓
10. CHECK WHEN SUPPORTED
        ↓
11. RUN
        ↓
12. INTERPRET
        ↓
13. VERIFY
        ↓
14. SESSION / RESULT
        ↓
15. PERFORM ONLY THE REQUIRED NEXT ACTION
        ↓
16. COLLECT NECESSARY EVIDENCE
        ↓
17. DOCUMENT
        ↓
18. CLEAN UP
        ↓
19. DEFINE THE NEXT OBJECTIVE
```

---

## Troubleshooting Model

When something fails:

```text
FAILURE
  ↓
WHAT EXACTLY FAILED?
  ↓
TARGET?
  ↓
SERVICE?
  ↓
VERSION / CONDITION?
  ↓
MODULE?
  ↓
OPTIONS?
  ↓
TARGET SELECTION?
  ↓
PAYLOAD?
  ↓
NETWORK?
  ↓
HANDLER?
  ↓
SESSION?
  ↓
ENVIRONMENT?
  ↓
ALTERNATIVE MODULE / TOOL?
```

Change **one important assumption at a time** and retest.

---

## Metasploit Is Not the Entire Toolkit

A capable penetration tester knows when to combine Metasploit with other tools.

Examples:

| Objective                     | Possible tool                         |
| ----------------------------- | ------------------------------------- |
| Network/service discovery     | Nmap                                  |
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

It teaches how to decide **when Metasploit belongs in the workflow**.

---

## Lab Environment

Use only intentionally vulnerable or explicitly authorized environments.

Recommended practice targets include:

* Metasploitable;
* OWASP Juice Shop where applicable;
* DVWA where applicable;
* other intentionally vulnerable local environments;
* explicitly authorized penetration-testing environments.

Do **not** use this repository to test systems without authorization.

---

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
* Document exact configurations when relevant.
* Stop when the objective is satisfied.
* Clean up appropriately.
* Know when another tool is more appropriate.

---

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

---

## Final Goal

The goal is not:

> **"I know Metasploit commands."**

The goal is:

> **"I understand Metasploit well enough that the interface feels familiar, the workflow feels natural, and I can independently determine what to do next when faced with an unfamiliar authorized situation."**

---

## Sources

For current framework behavior and terminology, prefer the official Metasploit documentation:

* [Metasploit Documentation](https://docs.metasploit.com/)
* [Metasploit Framework on GitHub](https://github.com/rapid7/metasploit-framework)

Commands and workflows in this repository should be verified against current framework documentation before being treated as authoritative.
