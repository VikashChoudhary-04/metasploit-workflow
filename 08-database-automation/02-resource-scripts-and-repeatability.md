# Resource Scripts and Repeatability

## Objective

Learn how to use Metasploit resource scripts to make repetitive workflows consistent, reviewable, and reproducible without turning automation into blind execution.

By the end of this file, you should be able to:

* Understand what resource scripts are.
* Identify tasks suitable for automation.
* Separate preparation from execution.
* Build small, readable resource scripts.
* Use variables and configuration deliberately.
* Review a script before running it.
* Make automation repeatable.
* Avoid hard-coding unsafe assumptions.
* Troubleshoot resource-script failures.
* Know when manual control is preferable.

## Why Automation Matters

A professional tester should not have to manually repeat the same harmless setup dozens of times.

For example:

```text id="p7m4cx"
Start Metasploit
  ↓
Check database
  ↓
Select workspace
  ↓
Set common configuration
  ↓
Search / load known functionality
  ↓
Run repeatable preparation
```

If this process is performed repeatedly, automation can improve:

```text id="n2q8vs"
Consistency
Speed
Repeatability
Documentation
Reduced typing errors
```

But automation introduces a new risk:

```text id="x5k1mr"
AUTOMATION
+
WRONG ASSUMPTION
=
REPEATABLE MISTAKE
```

Therefore:

```text id="c8v3qa"
AUTOMATE REPETITION,
NOT JUDGMENT.
```

## What Is a Resource Script?

A Metasploit resource script is a file containing Metasploit console commands that can be executed as a sequence.

Conceptually:

```text id="m6r1xp"
COMMAND
COMMAND
COMMAND
COMMAND
      ↓
RESOURCE SCRIPT
      ↓
REPEATABLE WORKFLOW
```

The goal is to turn a known sequence into a reusable procedure.

## Resource Scripts vs Programs

A resource script is not a replacement for a complete programming language.

It is better understood as:

```text id="w4k9nc"
A REPEATABLE METASPLOIT CONSOLE WORKFLOW
```

Use it when the task is already understood.

Do not use it to hide uncertainty.

## Good Automation Candidates

Good candidates include:

```text id="q8m3yd"
Workspace setup
Database checks
Consistent configuration
Known reconnaissance preparation
Repeated lab setup
Standard evidence preparation
Repeatable module configuration
Controlled test sequences
```

Poor candidates include:

```text id="v5x2pk"
Unknown target selection
Unvalidated exploitation
Automatic destructive actions
Blind payload selection
Unreviewed post-exploitation
Unbounded enumeration
```

The difference is:

```text id="r7n4ma"
KNOWN WORKFLOW
→
AUTOMATE

UNKNOWN DECISION
→
INVESTIGATE MANUALLY
```

## The Automation Decision Model

Before creating a resource script, ask:

```text id="j2m8xc"
1. Is the workflow repetitive?
2. Is the workflow understood?
3. Is it safe to repeat?
4. Are the inputs known?
5. Can the output be reviewed?
6. Is manual intervention required?
```

If several answers are "no", automation may be premature.

## Start Small

Do not begin with:

```text id="u6q1vr"
50-command automation script
```

Start with:

```text id="k9m3ws"
3–5 predictable steps
```

Then:

```text id="f4x8pn"
TEST
  ↓
VERIFY
  ↓
EXPAND
```

Small scripts are easier to understand and troubleshoot.

## Example: Workspace Preparation

Suppose an authorized lab repeatedly starts with:

```text id="s8v2mq"
Check database
Select workspace
Display workspace
```

A resource script can encode that known sequence.

Conceptually:

```text id="y5r7kc"
db_status
workspace lab-web
workspace
```

The exact commands and workspace names should be adapted to the local environment.

The important lesson is:

```text id="m3q8va"
REPEATABLE PREPARATION
→
RESOURCE SCRIPT
```

## Review Before Execution

Never execute an unfamiliar resource script simply because someone gave it to you.

Review:

```text id="c7n2px"
Every command
Every target
Every variable
Every module
Every configuration change
Every action
```

Ask:

```text id="h4m9wd"
What will this script do?
Against what?
With what configuration?
What happens if an assumption is wrong?
```

A resource script is executable operational logic.

Treat it accordingly.

## The Script Review Workflow

Use:

```text id="p6x3mr"
READ
  ↓
UNDERSTAND
  ↓
IDENTIFY TARGETS
  ↓
IDENTIFY CONFIGURATION
  ↓
IDENTIFY SIDE EFFECTS
  ↓
VALIDATE ASSUMPTIONS
  ↓
EXECUTE
  ↓
VERIFY
```

Do not skip the review phase.

## Configuration Before Execution

Avoid embedding assumptions without reviewing them.

For example:

```text id="w8q4kn"
set RHOSTS 10.10.10.10
```

may be appropriate in a controlled lab.

But blindly reusing that script against another environment could produce unintended behavior.

Prefer workflows where target-specific values are:

```text id="n1v7xc"
Explicit
Reviewed
Easy to identify
Easy to change
```

## Hard-Coded Values

Hard-coding is not automatically bad.

It becomes dangerous when the value represents an assumption that changes between environments.

Potentially environment-specific values include:

```text id="r5m8qp"
Target address
Target port
Workspace
LHOST
LPORT
Credentials
Module options
Paths
```

Before reuse, review all environment-specific values.

## Repeatability

A good script should produce approximately the same intended workflow when run against the same controlled conditions.

Think:

```text id="q3x9va"
SAME INPUT
   +
SAME ENVIRONMENT
   ↓
SAME WORKFLOW
   ↓
COMPARABLE RESULT
```

This makes troubleshooting and reporting easier.

## Reproducibility vs Repeatability

These terms are related but not identical.

### Repeatability

You can execute the same procedure again.

### Reproducibility

Another operator can understand and recreate the procedure from the documented information.

A good automation workflow aims for both.

```text id="m7k2ws"
SCRIPT
+
DOCUMENTATION
+
KNOWN INPUTS
=
REPRODUCIBLE WORKFLOW
```

## Resource Scripts Should Be Readable

Avoid turning a script into an unreadable wall of commands.

Prefer logical grouping:

```text id="v4p8nc"
# Database
...

# Workspace
...

# Configuration
...

# Validation
...

# Execution
...
```

Clear structure makes review easier.

## Comments

Use comments to explain intent.

For example:

```text id="x2m6qr"
# Select the dedicated lab workspace
workspace lab-web
```

The comment should answer:

```text id="j8v3ka"
Why is this command here?
```

not simply repeat the command.

## Automation and Validation

Automation should not remove validation.

Bad workflow:

```text id="s9q4mx"
Script
  ↓
Exploit
  ↓
Assume success
```

Better:

```text id="n5k2vc"
Script
  ↓
Controlled execution
  ↓
Verify
  ↓
Continue only if expected condition exists
```

Automation should support the workflow:

```text id="f1r7qp"
PREPARE
  ↓
EXECUTE
  ↓
VERIFY
```

not:

```text id="d8m3ya"
PREPARE
  ↓
EXECUTE
  ↓
EXECUTE MORE
  ↓
EXECUTE MORE
```

## Resource Scripts and Exploitation

Automation can be useful for a known lab workflow.

For example:

```text id="q6x1mv"
Load a known module
Set known lab parameters
Run a controlled test
Return to a predictable state
```

But the operator should still understand:

```text id="w7p4kc"
Why this module?
Why this target?
Why this payload?
What proves success?
What happens if it fails?
```

A script should encode a known decision, not replace the decision.

## Resource Scripts and Payloads

Payload configuration is especially sensitive to environment-specific assumptions.

Before automating payload-related settings, verify:

```text id="m4r8xn"
Target OS
Architecture
Payload compatibility
Callback address
Callback port
Network reachability
Handler requirements
```

Never assume that a previously successful payload configuration remains correct in another environment.

## Resource Scripts and Sessions

Session handling should generally remain deliberate.

Avoid creating automation that blindly:

```text id="c3v7qa"
Creates many sessions
Runs broad post-exploitation
Collects sensitive information
Moves laterally
```

Instead, use automation for predictable session preparation or controlled lab workflows.

## Automation and Scope

Scope must remain visible.

A script should make it obvious:

```text id="k9x2pw"
Which target?
Which workspace?
Which module?
Which action?
```

Avoid scripts whose behavior depends on:

```text id="r6m3vd"
"Whatever hosts happen to exist."
```

unless the workflow explicitly requires that behavior and the scope has been safely constrained.

## Automation Safety Gate

Before executing a resource script:

```text id="v8q4mc"
[ ] Authorized environment
[ ] Correct workspace
[ ] Correct target
[ ] Correct module
[ ] Configuration reviewed
[ ] Payload reviewed
[ ] Expected result defined
[ ] Stop condition defined
[ ] Side effects understood
```

Only then execute.

## Resource Script Example — Lab Preparation

A simple preparation script might conceptually contain:

```text id="y3n7kp"
# Verify database state
db_status

# Select the dedicated lab workspace
workspace metasploit-lab

# Display active workspace
workspace
```

This is a good automation candidate because:

```text id="j5x8qa"
The sequence is predictable.
The purpose is clear.
The actions are low complexity.
The result can be reviewed.
```

## Running a Resource Script

Metasploit supports loading resource scripts through the console.

A commonly used form is:

```text id="p2m6wr"
resource path/to/script.rc
```

The important reasoning is:

```text id="x7v3nc"
RESOURCE FILE
      ↓
LOAD
      ↓
EXECUTE COMMAND SEQUENCE
      ↓
OBSERVE OUTPUT
      ↓
VERIFY
```

Do not treat `resource` as:

```text id="s4q9mv"
"Run this file and trust it."
```

## Script Output

When running automation, observe:

```text id="n8k2ya"
Errors
Warnings
Unexpected targets
Unexpected module state
Unexpected sessions
Unexpected configuration
```

Automation can make failures happen faster.

It does not make failures disappear.

## Troubleshooting Resource Scripts

If a script fails, isolate the failing step.

Use:

```text id="m1v7qx"
SCRIPT
  ↓
STEP 1
  ↓
VERIFY
  ↓
STEP 2
  ↓
VERIFY
```

Instead of:

```text id="q5x8kc"
SCRIPT FAILED
→
RERUN ENTIRE SCRIPT
```

The second approach may repeat the same problem and create additional side effects.

## One-Change Rule

When troubleshooting automation:

```text id="c8m4vr"
FAILURE
  ↓
IDENTIFY FAILED STEP
  ↓
CHANGE ONE ASSUMPTION
  ↓
RETEST
```

This preserves causality.

## Idempotence

A useful automation concept is whether running the same preparation repeatedly causes unnecessary changes.

For example:

```text id="f7n3mp"
CHECK DATABASE
```

is generally less problematic to repeat than:

```text id="w2r8qa"
CREATE NEW TEST STATE
```

again and again.

Prefer automation that is:

```text id="k6x1vc"
Predictable
Controlled
Low-side-effect
Safe to repeat
```

## Automation Levels

Use progressive automation:

```text id="u4m9qx"
Level 1
Manual workflow

Level 2
Small helper script

Level 3
Repeatable preparation

Level 4
Controlled execution

Level 5
Parameterized workflow

Level 6
Larger automation
```

Do not jump directly to Level 6.

First prove that the underlying workflow is correct.

## Manual vs Automated Decision

Use:

```text id="r3q7mx"
REPETITIVE + WELL UNDERSTOOD
        ↓
AUTOMATE
```

Use:

```text id="v8k2nc"
UNCERTAIN + HIGH IMPACT
        ↓
MANUAL CONTROL
```

And:

```text id="j6m4pw"
REPETITIVE + HIGH IMPACT
        ↓
AUTOMATE ONLY AFTER
CAREFUL VALIDATION
```

## Practical Exercise 1 — Build a Preparation Script

Create a simple `.rc` file for an authorized lab.

It should:

```text id="q2n8vy"
1. Check database state.
2. Select a dedicated workspace.
3. Display the active workspace.
```

Run it manually.

### Success Criteria

You can explain every command before executing the script.

## Practical Exercise 2 — Review Before Execution

Take an existing resource script from your lab environment.

Before running it, create:

```text id="m5x9pc"
Target:
...

Workspace:
...

Modules:
...

Configuration:
...

Potential side effects:
...

Expected result:
...

Stop condition:
...
```

Then execute it.

### Success Criteria

You understand the script before running it.

## Practical Exercise 3 — Find the Hidden Assumption

Given:

```text id="s7r3kx"
set RHOSTS 10.10.10.20
set RPORT 8080
set LHOST 10.10.10.5
```

Identify:

```text id="p4v8mc"
Which values are environment-specific?
Which assumptions could make the workflow fail?
What must be verified before reuse?
```

### Success Criteria

You can identify configuration dependencies without blindly changing values.

## Practical Exercise 4 — Script Failure Diagnosis

Create or use a controlled lab script that contains a deliberately incorrect non-destructive configuration.

Run it and identify:

```text id="n9q2wa"
Which step failed?
Why?
What assumption was wrong?
What single change should be tested?
```

Then rerun only after understanding the failure.

### Success Criteria

You troubleshoot the workflow instead of repeatedly executing the entire script.

## Practical Exercise 5 — Manual vs Automated

For each activity, decide:

```text id="k8m3vp"
Manual
or
Automated
```

Activities:

```text id="f5x7qn"
1. Selecting a known lab workspace.
2. Choosing an exploit for an unfamiliar target.
3. Setting repeated lab configuration.
4. Validating an unexpected vulnerability.
5. Running a known harmless preparation sequence.
6. Deciding whether to escalate privileges.
7. Collecting all files from a compromised system.
8. Repeating a controlled test against identical lab targets.
```

Explain your reasoning.

### Success Criteria

You can identify where automation helps and where human judgment should remain.

## Practical Exercise 6 — Build a Repeatability Record

For one resource script, document:

```text id="r2m6xc"
Purpose:
...

Inputs:
...

Target assumptions:
...

Workspace:
...

Modules:
...

Expected result:
...

Observed result:
...

Known limitations:
...

Cleanup:
...
```

This turns a script into a reproducible procedure.

## Common Mistakes

### Mistake 1 — Automating an Unknown Workflow

Correction:

```text id="q7v3mx"
Understand the workflow manually first.
```

### Mistake 2 — Automating Blind Exploitation

Correction:

```text id="m4k8pc"
Keep target selection and high-impact decisions deliberate.
```

### Mistake 3 — Hard-Coding Without Reviewing

Correction:

```text id="n6x2wr"
Review every environment-specific value before reuse.
```

### Mistake 4 — Treating a Script as Trusted

Correction:

```text id="c9p5va"
Read and understand every command before execution.
```

### Mistake 5 — Re-running the Entire Script After Failure

Correction:

```text id="j3m7qx"
Identify the failing step and isolate the problem.
```

### Mistake 6 — Removing Validation

Correction:

```text id="v8r4kc"
Automation must still verify results.
```

### Mistake 7 — Over-Automating Post-Exploitation

Correction:

```text id="p2n6yw"
Keep sensitive or high-impact actions deliberate and objective-driven.
```

### Mistake 8 — Ignoring Cleanup

Correction:

```text id="s5x9mq"
Define cleanup as part of the automated workflow where appropriate.
```

## Professional Workflow

Use:

```text id="u7q3nc"
UNDERSTAND MANUALLY
      ↓
DEFINE REPEATABLE STEPS
      ↓
CREATE SMALL SCRIPT
      ↓
REVIEW SCRIPT
      ↓
TEST IN LAB
      ↓
VERIFY RESULT
      ↓
DOCUMENT
      ↓
REUSE
      ↓
IMPROVE
```

Automation should emerge from understanding.

Not replace it.

## Repeatability Checklist

Before calling a workflow reusable:

```text id="x4m8vp"
[ ] Purpose is clearly defined.
[ ] Inputs are known.
[ ] Target assumptions are documented.
[ ] Workspace is explicit.
[ ] Environment-specific values are identifiable.
[ ] Every command is understood.
[ ] Side effects are known.
[ ] Expected result is defined.
[ ] Validation is included.
[ ] Failure behavior is understood.
[ ] Cleanup is defined.
[ ] Script has been tested in an authorized lab.
[ ] Another operator could understand its purpose.
```

## When Not to Automate

Do not automate merely because you can.

Prefer manual control when:

```text id="q9v2mk"
The target is unfamiliar.
```

```text id="h6x3wp"
The vulnerability is not yet validated.
```

```text id="r8m4nc"
The action may cause significant impact.
```

```text id="j1k7qa"
The workflow depends on changing human judgment.
```

```text id="c5v9xm"
The scope is uncertain.
```

Automation is valuable when it reduces repetitive work.

It is harmful when it removes necessary decision points.

## Know When to Stop

Stop automation when:

```text id="m3x8qp"
The workflow is no longer predictable.
```

or:

```text id="v7k2na"
An assumption becomes invalid.
```

or:

```text id="f4r9mc"
The target leaves the expected scope.
```

or:

```text id="p8q1yx"
The action requires a new security decision.
```

At that point:

```text id="s6m3vk"
PAUSE AUTOMATION
      ↓
REASSESS
      ↓
MAKE DECISION
      ↓
CONTINUE MANUALLY IF JUSTIFIED
```

## Completion Checklist

Before moving to troubleshooting, confirm that you can:

```text id="n4x7qc"
[ ] Explain what a resource script is.
[ ] Identify appropriate automation candidates.
[ ] Identify poor automation candidates.
[ ] Build a small resource script.
[ ] Read a resource script before execution.
[ ] Identify environment-specific assumptions.
[ ] Understand hard-coded configuration.
[ ] Use explicit workspaces.
[ ] Define expected results.
[ ] Preserve validation.
[ ] Troubleshoot individual script steps.
[ ] Apply the one-change rule.
[ ] Understand repeatability.
[ ] Understand reproducibility.
[ ] Prefer low-side-effect automation.
[ ] Keep high-impact decisions deliberate.
[ ] Include cleanup considerations.
[ ] Document reusable workflows.
[ ] Know when to stop automation.
```

## Key Mental Model

Remember:

```text id="w8m2pv"
UNDERSTAND
  ↓
STANDARDIZE
  ↓
AUTOMATE
  ↓
VERIFY
  ↓
REPEAT
```

And:

```text id="k5r9xc"
AUTOMATE REPETITION.
DO NOT AUTOMATE AWAY JUDGMENT.
```

The professional goal is not to create the largest resource script.

It is to create the smallest reliable automation that makes a known workflow:

```text id="a7q3mn"
Faster
Consistent
Reviewable
Reproducible
Controlled
```

## Next Step

The next file is:

```text id="p3v8kx"
09-troubleshooting/01-troubleshooting-decision-system.md
```

There we will build the **systematic troubleshooting framework** for when modules, payloads, handlers, sessions, database features, or resource scripts do not behave as expected.
