# Module Validation

## Objective

Learn how to validate a selected Metasploit module and its assumptions before execution.

By the end of this file, you should be able to:

* Explain what module validation is.
* Distinguish module applicability from exploit success.
* Use `check` when a module supports it.
* Interpret positive, negative, and inconclusive validation results.
* Validate target, service, version, configuration, and connectivity assumptions.
* Identify when validation is sufficient to proceed.
* Recognize when more information is required.
* Avoid treating failed exploitation as the first validation method.
* Document the evidence behind an execution decision.

## Why Validation Matters

The previous section taught:

```text id="n3c7v2"
SEARCH
  ↓
READ
  ↓
COMPARE
  ↓
SELECT
```

That does not mean:

```text id="r8m2k5"
SELECT
  ↓
RUN
```

There is an important stage between selection and execution:

```text id="w4q9p1"
SELECT
  ↓
VALIDATE
  ↓
EXECUTE IF JUSTIFIED
```

Validation reduces uncertainty.

It helps answer:

```text id="k6x1s8"
Does the target appear to satisfy the conditions
this module expects?
```

## Validation Is Not a Guarantee

This distinction is essential.

A successful validation does not mean:

```text id="f7p3m9"
The exploit will definitely succeed.
```

A failed validation does not always mean:

```text id="c2n8v4"
The vulnerability definitely does not exist.
```

Instead:

```text id="y5r1q7"
Validation result
=
additional evidence
```

Use that evidence together with everything else you know about the target.

## The Validation Model

Use:

```text id="t8m4c1"
MODULE
  ↓
UNDERSTAND REQUIREMENTS
  ↓
VERIFY TARGET
  ↓
VERIFY SERVICE
  ↓
VERIFY VERSION / CONDITION
  ↓
VERIFY CONFIGURATION
  ↓
VERIFY CONNECTIVITY
  ↓
CHECK WHEN SUPPORTED
  ↓
INTERPRET EVIDENCE
  ↓
DECIDE
```

The `check` command is one part of this process.

It is not the entire process.

## What Are We Validating?

There are several different questions.

### Target Validation

```text id="p3x7m9"
Am I testing the correct system?
```

### Service Validation

```text id="j6q2v8"
Is the expected service actually available?
```

### Version Validation

```text id="d4n9k1"
Is the product/version consistent with the module's requirements?
```

### Configuration Validation

```text id="s7m2c5"
Is the required feature or configuration present?
```

### Network Validation

```text id="v1q8r4"
Can my testing system communicate with the required target endpoint?
```

### Module Validation

```text id="x5c9p2"
Does the module itself report that the target appears applicable?
```

### Execution Validation

```text id="n8r3m6"
Did the actual operation produce the expected result?
```

These are different questions.

Do not collapse them into one.

## The Validation Ladder

A useful model is:

```text id="z4m7q1"
Target known
   ↓
Service known
   ↓
Version known
   ↓
Condition known
   ↓
Module requirements satisfied
   ↓
Connectivity verified
   ↓
Module check, if supported
   ↓
Execution
   ↓
Result verification
```

Each step reduces uncertainty.

## Validation Before `check`

Before using:

```text id="y3p8c6"
check
```

ask:

```text id="m1q7v4"
Have I configured the target correctly?
```

Then:

```text id="s6k2n9"
Have I configured the correct port?
```

Then:

```text id="h8r5c3"
Do I understand the service?
```

Then:

```text id="w2v9m7"
Does the module actually apply to this target?
```

If these questions are unanswered, `check` may not provide a useful answer.

## Using `check`

When supported and appropriate:

```text id="k4p1s8"
check
```

The module may return information indicating whether the target appears:

* Vulnerable
* Not vulnerable
* Suitable
* Unsuitable
* Inconclusive
* Unable to be checked

The exact output depends on the module.

Always read the actual result.

## Positive Check Result

A positive result can strengthen your confidence.

Conceptually:

```text id="n7c2x5"
Existing evidence
+
Positive module check
=
stronger case for applicability
```

But do not convert this into:

```text id="r4m8q1"
Guaranteed exploit success
```

There can still be:

* Mitigations
* Environmental differences
* Race conditions
* Authentication requirements
* Network problems
* Payload incompatibility
* Module limitations
* Target-specific behavior

## Negative Check Result

A negative result should cause you to investigate.

Possible explanations include:

```text id="x6p3v9"
The vulnerability is absent.
```

```text id="q1m8c4"
The target is not supported.
```

```text id="t5r2n7"
The module cannot detect the condition.
```

```text id="j9k4s1"
A prerequisite is missing.
```

```text id="d8v6p2"
The configuration is different.
```

Do not immediately conclude:

```text id="f3m7q9"
Target is definitely secure.
```

## Inconclusive Results

An inconclusive result is valuable information.

It means:

```text id="z2c8m5"
The current evidence is insufficient to establish the condition.
```

The correct next step may be:

```text id="n6q1v4"
Gather more information.
```

rather than:

```text id="b9r5x7"
Run the exploit anyway.
```

## No `check` Support

Not every module provides the same validation capabilities.

If:

```text id="c7m2p8"
check
```

is unavailable or not meaningful for the module, use other evidence.

For example:

```text id="v4n9k1"
Service enumeration
+
Version identification
+
Configuration evidence
+
Vendor advisory
+
Manual validation
```

The absence of a built-in check does not eliminate the need for validation.

It simply means validation must come from other sources.

## Validation Through External Evidence

Metasploit should not be the only source of truth.

Depending on the engagement, you may use:

* Service enumeration
* Product documentation
* Vendor advisories
* CVE information
* Security research
* Vulnerability scanner results
* Manual protocol interaction
* Application behavior
* Configuration inspection

The principle is:

```text id="s8x3m6"
Independent evidence
+
Metasploit evidence
=
stronger validation
```

## Validation vs Exploitation

These are separate stages.

### Validation

Question:

```text id="p5q2v7"
Does the target appear to satisfy the vulnerability conditions?
```

### Exploitation

Question:

```text id="k1m9c4"
Can the vulnerability be triggered to produce the intended effect?
```

The second is more intrusive.

Therefore:

```text id="x7r3n8"
Prefer sufficient validation before unnecessary exploitation.
```

## Minimum Necessary Action

A professional operator asks:

```text id="w4c6p1"
What is the minimum action required to satisfy the objective?
```

Suppose the objective is:

```text id="m8q2v5"
Determine whether a vulnerability exists.
```

If you have enough evidence to establish that condition without obtaining a shell, do not automatically escalate to exploitation.

Suppose instead the objective is:

```text id="n3r7k9"
Demonstrate controlled code execution in an authorized lab.
```

Then exploitation may be explicitly required.

The objective determines the appropriate level of action.

## Validation as Risk Reduction

Validation can reduce unnecessary intrusive activity.

Conceptually:

```text id="q6m1v8"
No validation
    ↓
Higher uncertainty
    ↓
More trial and error
    ↓
More unnecessary attempts
```

versus:

```text id="j9p4c2"
Strong validation
    ↓
Lower uncertainty
    ↓
More deliberate execution
    ↓
Better evidence
```

This is especially important in professional engagements.

## Target Validation

Before anything intrusive, verify the target.

Check:

```text id="f2x8n5"
IP address
Hostname
Network segment
Port
Service
```

Where appropriate, verify that the target belongs to the authorized scope.

Do not rely on memory.

A simple IP typo can turn a correct technique into an unauthorized action against the wrong system.

## Scope Validation

Technical validation is not enough.

You must also validate:

```text id="y7k3m9"
Is this target in scope?
```

A technically vulnerable system can still be outside the engagement scope.

Therefore:

```text id="c1n6r4"
Vulnerable
≠
Authorized to exploit
```

This distinction should remain part of every Metasploit workflow.

## Service Validation

Suppose your module expects:

```text id="v5m2q8"
TCP/445
```

Verify that the target actually exposes the expected service.

Do not assume:

```text id="r8x4p1"
port number
=
service identity
```

Service enumeration should support your conclusion.

## Version Validation

If the module depends on a version, verify it.

Potential sources include:

```text id="n3c7m2"
Service banners
Application responses
Authenticated enumeration
Vendor information
Authorized scanning
```

Be careful with uncertain version fingerprints.

A scanner may report:

```text id="q9p5v1"
likely version
```

rather than:

```text id="m6r2x8"
confirmed version
```

Treat those differently.

## Configuration Validation

Some vulnerabilities depend on configuration.

Ask:

```text id="d7k4n9"
What exact condition does the module require?
```

Then:

```text id="p2x8c5"
Do I have evidence that condition exists?
```

If not:

```text id="f1m6q3"
Investigate before execution.
```

## Network Validation

Even a perfectly selected module can fail if required connectivity does not exist.

Consider:

```text id="z8r3v7"
Operator
   ↓
Network path
   ↓
Target
```

and for reverse connections:

```text id="s4n9k2"
Target
   ↓
Network path
   ↓
Operator listener
```

A vulnerability can exist while the required connection path does not.

Separate:

```text id="j6p1c8"
Vulnerability applicability
```

from:

```text id="w3m7q5"
Network reachability
```

## Callback Validation

For reverse payloads, the callback configuration introduces another validation problem.

Conceptually:

```text id="c8v2n6"
Target executes payload
        ↓
Target initiates connection
        ↓
Configured callback address
        ↓
Configured listener
        ↓
Session
```

Every stage can fail.

Therefore:

```text id="r1q7m4"
No session
≠
Exploit failed
```

It could mean the exploitation succeeded but the callback path failed.

This distinction becomes important in the payload section.

## Execution Validation

After execution, validate the result.

Do not assume:

```text id="x5n8p2"
No error
=
Success
```

Instead ask:

```text id="m3q9v6"
Did the intended effect occur?
```

For example:

```text id="a7k2c4"
Expected:
Session

Observed:
No session
```

You now have a discrepancy to investigate.

## Result Verification

Verification depends on the objective.

Examples:

```text id="j8r4n1"
Objective:
Obtain a session

Verification:
Session exists and is interactive.
```

```text id="p6v2m9"
Objective:
Validate a vulnerability

Verification:
Target behavior provides evidence consistent with the vulnerability.
```

```text id="c3x7q5"
Objective:
Collect authorized configuration evidence

Verification:
Expected information was successfully retrieved.
```

Always define the expected result before execution when possible.

## Expected Result vs Actual Result

Before execution, write:

```text id="d9m4s1"
Expected:
What should happen if my hypothesis is correct?
```

After execution:

```text id="q2v8k6"
Actual:
What actually happened?
```

Then compare:

```text id="h7p3n9"
Expected
   vs
Actual
```

This is an extremely effective troubleshooting technique.

## Validation Confidence

You can think about validation in terms of evidence strength.

### Weak

```text id="f8c2m5"
Only a port is open.
```

### Moderate

```text id="n4r7v1"
Product and version appear to match.
```

### Stronger

```text id="k6x9p3"
Product/version match
+
required feature confirmed
+
vulnerability evidence
+
module check supports applicability
```

This is not a numerical scoring system.

It is a reminder that decisions should reflect evidence quality.

## Avoid False Certainty

Security testing often involves incomplete information.

Avoid statements such as:

```text id="v1m8q4"
"This must be vulnerable."
```

when your evidence only shows:

```text id="s5c2n7"
"The product appears to match a potentially affected version."
```

Use precise language.

For example:

```text id="r7p4x1"
"Current evidence supports investigating this module."
```

or:

```text id="j3q8m6"
"The module check was inconclusive; additional validation is required."
```

This improves both technical decisions and reporting quality.

## Practical Exercise 1 — Validation Ladder

### Objective

Practice validating a candidate before execution.

### Task

Choose an appropriate module in an authorized lab.

Document:

```text id="c9v2m7"
Target:
...

Objective:
...

Product:
...

Version:
...

Configuration:
...

Module:
...

Module prerequisites:
...

Network requirements:
...

Check supported?
...

Check result:
...

Other evidence:
...

Decision:
Proceed / Gather information / Stop

Reason:
...
```

Do not execute merely because the module is available.

## Practical Exercise 2 — Interpret `check`

### Objective

Learn to interpret validation output without overclaiming.

Use an authorized lab module that supports checking.

Record the result.

Then answer:

```text id="x4n8p2"
What does the result directly establish?

What does it not establish?

What additional evidence supports the conclusion?

What would change your decision?
```

The purpose is to separate:

```text id="q1m7c5"
observation
```

from:

```text id="z8r3v6"
interpretation
```

## Practical Exercise 3 — Negative Validation

### Scenario

A module reports that the target does not appear vulnerable.

### Task

Create three hypotheses:

```text id="b5k9m2"
H1:
The vulnerability is absent.

H2:
The module cannot detect the condition correctly.

H3:
My target assumptions are incorrect.
```

Then determine what evidence could distinguish them.

This prevents the simplistic conclusion:

```text id="n3p7x1"
negative check
=
definitely safe
```

## Practical Exercise 4 — No Check Available

### Scenario

The module does not provide useful `check` functionality.

### Task

Build an alternative validation path:

```text id="m6q2r8"
Target identity
      ↓
Service validation
      ↓
Version validation
      ↓
Configuration validation
      ↓
Vulnerability evidence
      ↓
Module requirements
      ↓
Decision
```

Document what evidence you would collect at each stage.

## Practical Exercise 5 — Expected vs Actual

### Objective

Learn result-based validation.

Before execution, write:

```text id="w9c4p6"
Expected:
...
```

After execution:

```text id="f2n8m3"
Actual:
...
```

Then:

```text id="r5q7v1"
Difference:
...

Likely explanation:
...

Next test:
...
```

Do not immediately rerun the same operation.

## Common Validation Mistakes

### Mistake 1 — Treating `check` as Absolute Proof

Correction:

```text id="k8m3q5"
Treat the result as evidence.
```

### Mistake 2 — Skipping Target Validation

Correction:

```text id="p1r7v9"
Verify the target before execution.
```

### Mistake 3 — Assuming Product Match Is Enough

Correction:

```text id="x4c9n2"
Check version, condition, configuration, and prerequisites.
```

### Mistake 4 — Exploiting to Find Out Whether Exploitation Was Needed

Correction:

```text id="m7q2d8"
Determine the minimum validation required by the objective.
```

### Mistake 5 — Treating Negative Checks as Absolute

Correction:

```text id="v3n6p1"
Investigate what the check actually tests.
```

### Mistake 6 — Ignoring Network Conditions

Correction:

```text id="s9r4k7"
Separate vulnerability applicability from connectivity.
```

### Mistake 7 — Assuming No Session Means No Exploit

Correction:

```text id="j5x8m2"
Investigate exploit execution and payload/session behavior separately.
```

### Mistake 8 — Ignoring Scope

Correction:

```text id="q6c1v9"
Technical applicability never overrides engagement authorization.
```

## The Validation Decision Tree

Use this before intrusive execution:

```text id="z7m3q1"
Is the target authorized?
        │
        ├── NO → STOP
        │
        └── YES
              ↓
        Is the target correct?
              │
              ├── NO → STOP / CORRECT TARGET
              │
              └── YES
                    ↓
             Is the service correct?
                    │
                    ├── NO → GATHER INFORMATION
                    │
                    └── YES
                          ↓
                   Is the version/condition known?
                          │
                    ┌─────┴─────┐
                   NO          YES
                   ↓            ↓
             GATHER INFO    Check requirements
                                 ↓
                         Requirements satisfied?
                                 │
                           ┌─────┴─────┐
                          NO          YES
                          ↓            ↓
                    GATHER INFO    Validate
                                      ↓
                              Check supported?
                               │           │
                              YES          NO
                               ↓            ↓
                             CHECK     Other evidence
                               │           │
                               └─────┬─────┘
                                     ↓
                                INTERPRET
                                     ↓
                         Enough evidence to proceed?
                              │             │
                             NO            YES
                              ↓             ↓
                         GATHER INFO    EXECUTE IF
                                       AUTHORIZED
```

## When to Proceed

Proceed only when you can explain:

```text id="c8r1m6"
Why this module?
Why this target?
Why this configuration?
Why this action?
What result do I expect?
How will I verify it?
```

If you cannot answer those questions, stop and gather information.

## When to Gather More Information

Gather more information when:

```text id="n4q7p2"
Product is uncertain.
```

```text id="v8m3c6"
Version is uncertain.
```

```text id="j1r9k5"
Required configuration is unknown.
```

```text id="s6x2d8"
Module applicability is uncertain.
```

```text id="p5c7n1"
Check result is inconclusive.
```

```text id="q9m4r3"
Network behavior is unexplained.
```

The correct response to uncertainty is often information gathering.

## When to Stop

Stop when:

```text id="w2k8v5"
The target is outside scope.
```

or:

```text id="f7n3m9"
The required condition is demonstrably absent.
```

or:

```text id="c4p1x6"
The module is clearly incompatible.
```

or:

```text id="r8q5m2"
Further attempts would provide no new information.
```

or:

```text id="t3v7k9"
The engagement objective has already been satisfied.
```

Stopping is a professional decision, not a failure.

## Industry Documentation Pattern

Record validation before execution.

Example:

```text id="m8c2q5"
Objective:
Validate suspected vulnerability.

Target:
192.168.56.101

Evidence:
Service and version identified.

Candidate module:
<module>

Applicability:
Product/version match.

Prerequisites:
Confirmed / unknown.

Module check:
<result>

Independent evidence:
<evidence>

Risk/scope:
Authorized lab target.

Decision:
Proceed / Gather information / Stop

Reason:
<reason>
```

This makes the decision auditable.

## The Complete Module Lifecycle

At this point, the complete module workflow should look like:

```text id="x7p4n1"
OBJECTIVE
    ↓
EVIDENCE
    ↓
SEARCH
    ↓
CANDIDATES
    ↓
READ
    ↓
COMPARE
    ↓
SELECT
    ↓
CONFIGURE
    ↓
VERIFY
    ↓
VALIDATE
    ↓
DECIDE
    │
    ├── Gather more information
    │
    ├── Stop
    │
    └── Execute
            ↓
         VERIFY RESULT
            ↓
         DOCUMENT
```

This is the foundation for the exploitation section later.

## Completion Checklist

Before leaving the module section, confirm that you can:

```text id="d2m8q6"
[ ] Explain the purpose of module validation.
[ ] Distinguish validation from exploitation.
[ ] Validate target identity.
[ ] Validate service identity.
[ ] Validate version information.
[ ] Validate configuration assumptions.
[ ] Validate network requirements.
[ ] Use check when supported.
[ ] Interpret positive check results.
[ ] Interpret negative check results.
[ ] Interpret inconclusive results.
[ ] Build an alternative validation path when check is unavailable.
[ ] Define expected results before execution.
[ ] Compare expected and actual results.
[ ] Recognize payload/session failures separately from exploit applicability.
[ ] Know when to gather more information.
[ ] Know when to stop.
[ ] Document the evidence behind an execution decision.
```

## Key Mental Model

Remember:

```text id="p5x9m3"
SELECT
  ↓
VERIFY ASSUMPTIONS
  ↓
VALIDATE
  ↓
INTERPRET EVIDENCE
  ↓
DECIDE
```

And:

```text id="j7c2r8"
CHECK ≠ GUARANTEE
```

Instead:

```text id="n4v6q1"
CHECK
  +
TARGET EVIDENCE
  +
MODULE REQUIREMENTS
  +
CONFIGURATION
  +
NETWORK CONDITIONS
  ↓
INFORMED DECISION
```

The goal of validation is not to make failure impossible.

The goal is to make your decision **evidence-driven**.

## Section Complete

You have now completed the core module workflow:

```text id="r8m1v5"
SEARCH
  ↓
READ
  ↓
SELECT
  ↓
CONFIGURE
  ↓
VALIDATE
```

The next major section moves into payloads.

The next file is:

```text id="z3q7c2"
04-payloads/01-payload-mental-model.md
```

There we will build the mental model for **what a payload actually is, how it relates to an exploit, staged vs stageless payloads, reverse vs bind connections, compatibility, and why payload selection is a separate decision from exploit selection**.
