# Module Selection

## Objective

Learn how to select the appropriate Metasploit module from multiple candidates using evidence, compatibility, prerequisites, and engagement objectives.

By the end of this file, you should be able to:

* Turn target evidence into a module-selection hypothesis.
* Compare multiple candidate modules.
* Distinguish relevance from applicability.
* Identify hidden prerequisites.
* Evaluate product, version, platform, architecture, and configuration compatibility.
* Recognize weak assumptions before exploitation.
* Reject modules deliberately.
* Know when more enumeration is required.
* Select a module for a defensible reason rather than because it appeared first.

## Why Module Selection Is a Core Skill

Finding modules is easy.

Selecting the right one is harder.

A search may return:

```text id="h7k2m4"
Module A
Module B
Module C
Module D
Module E
```

The operator's job is not:

```text id="f1n8q3"
Which one looks coolest?
```

It is:

```text id="y4p6w2"
Which candidate best matches the evidence,
objective, prerequisites, and target?
```

This is a decision problem.

## The Selection Model

Use this sequence:

```text id="p8d3s1"
TARGET EVIDENCE
      ↓
OBJECTIVE
      ↓
SEARCH
      ↓
CANDIDATES
      ↓
APPLICABILITY
      ↓
PREREQUISITES
      ↓
COMPATIBILITY
      ↓
VALIDATION
      ↓
SELECTION
```

Do not reverse the process:

```text id="q2v7r9"
MODULE
  ↓
Find a reason to use it
```

Instead:

```text id="m5x1c8"
EVIDENCE
  ↓
Determine what capability is needed
  ↓
Find modules
```

## Start With the Objective

Before comparing modules, define the actual objective.

Examples:

```text id="r4j6k0"
Objective:
Enumerate a service.
```

```text id="n8w3y5"
Objective:
Validate a suspected vulnerability.
```

```text id="t1c9v6"
Objective:
Demonstrate controlled impact.
```

```text id="b7q2m8"
Objective:
Gather authorized evidence after access.
```

These objectives can require completely different module categories.

## Objective vs Technique

Do not confuse the objective with the technique.

For example:

```text id="k3m8p1"
Objective:
Determine whether a target is vulnerable.
```

is not the same as:

```text id="d9x5q2"
Technique:
Exploit the target.
```

The objective may be satisfied through:

```text id="f6w1r8"
Passive evidence
+
Enumeration
+
Manual validation
+
Metasploit check
```

without requiring full exploitation.

## Build an Evidence Record

Before selecting a module, record what you know.

Example:

```text id="a3v7n2"
Target:
192.168.56.101

Port:
445/TCP

Service:
SMB

Product:
Known

Version:
Known

Observed behavior:
Known

Suspected issue:
Known / suspected

Objective:
Controlled validation
```

This prevents your memory from becoming the source of truth.

## Evidence Quality

Classify your information.

### Strong Evidence

Examples include:

```text id="j4r8t2"
Confirmed service version
Vendor documentation
Reliable vulnerability reference
Directly observed configuration
Successful manual validation
```

### Moderate Evidence

Examples:

```text id="q6n1x5"
Service fingerprint
Banner information
Scanner identification
Behavior consistent with a known version
```

### Weak Evidence

Examples:

```text id="v8m3c7"
Port alone
Generic product name
Old scan result
Guess based on naming
Assumption based on default configuration
```

The weaker the evidence, the more validation you should perform before selecting an intrusive technique.

## The Compatibility Question

For every candidate module, ask:

```text id="s2k9p4"
Does this module's expected target condition
match what I actually know about the target?
```

Break this into several dimensions.

```text id="w5h7d1"
Product
Version
Platform
Architecture
Protocol
Configuration
Feature
Vulnerability
Objective
```

Not every module depends on every dimension.

The point is to identify the dimensions that matter for the specific module.

## Product Compatibility

Suppose your evidence says:

```text id="x3c8n5"
Product A
```

and the module targets:

```text id="m7q1z6"
Product B
```

Reject it.

Do not assume similar products are interchangeable.

Even products implementing the same protocol may have completely different vulnerabilities.

## Version Compatibility

Version information can be critical.

For example:

```text id="k9p4v2"
Observed:
Product X version 2.1
```

while the module targets:

```text id="y6n3w8"
Product X version 4.x
```

That is a compatibility problem.

Do not assume:

```text id="d2f7m1"
same product
=
same vulnerability
```

## Version Uncertainty

Sometimes you do not know the exact version.

For example:

```text id="r5c8j3"
Product:
Apache HTTP Server

Version:
Unknown
```

You may find several modules that appear relevant.

Do not pretend the version is known.

Instead:

```text id="p1v6q9"
Unknown
  ↓
Gather version information
  ↓
Narrow candidates
  ↓
Validate applicability
```

Uncertainty should change your workflow.

## Platform Compatibility

Some modules depend on the target operating system or platform.

Ask:

```text id="n4x7s2"
Does the module support the target platform?
```

For example:

```text id="g8m1c5"
Windows
Linux
Unix-like
Network appliance
Application platform
```

The exact supported platforms depend on the module.

Never assume that a vulnerability automatically means every platform variant is exploitable in the same way.

## Architecture Compatibility

Architecture can matter particularly when payload execution is involved.

Examples include:

```text id="e5j2r7"
x86
x64
ARM
Other supported architectures
```

A vulnerability may be present while a particular payload or target configuration is incompatible.

Therefore separate:

```text id="c8n3v1"
Vulnerability applicability
```

from:

```text id="m2q6s9"
Payload compatibility
```

These are related but different questions.

## Protocol Compatibility

A module may expect a specific protocol or communication mechanism.

For example:

```text id="h7p1d4"
SMB
HTTP
HTTPS
FTP
SSH
RDP
DNS
```

Do not assume that an open port proves the expected protocol.

For example:

```text id="z9k3f6"
Port 8080 open
```

does not automatically prove:

```text id="a1m8q5"
HTTP service
```

Verify the service.

## Configuration Compatibility

This is often overlooked.

A product and version may match, but the vulnerability may depend on:

* A feature being enabled.
* A particular configuration.
* A specific authentication state.
* A specific deployment mode.
* A specific protocol behavior.
* A particular application endpoint.

Therefore:

```text id="b6r2w9"
Product match
+
Version match
```

does not necessarily mean:

```text id="q4n7c1"
Module applies
```

## Vulnerability Compatibility

A candidate module should correspond to the actual condition you are investigating.

Ask:

```text id="t8m5p2"
What vulnerability or behavior does this module target?
```

Then:

```text id="s3v9k6"
Do I have evidence that the target has that condition?
```

If the answer is:

```text id="u1d7x4"
No
```

you may need additional validation before executing.

## Objective Compatibility

Even if a module applies technically, it may not match the engagement objective.

For example:

```text id="w8c2m5"
Module:
Obtains code execution

Objective:
Enumerate service configuration
```

The module may technically work, but exploitation may be unnecessary.

A better approach may be:

```text id="n5q9r3"
Use enumeration first.
```

Always ask:

```text id="f2m7x8"
What is the minimum action required to satisfy the objective?
```

## Prerequisites

A module can have prerequisites that are not obvious from its name.

Possible prerequisites include:

```text id="j6v1p4"
Specific product
Specific version
Specific configuration
Authentication
A reachable service
A known endpoint
A particular target type
A compatible payload
A particular platform
```

Read the module documentation and options.

Do not discover prerequisites only after a failed exploit attempt.

## The Candidate Comparison Process

When multiple modules look relevant, create a comparison.

Example:

| Candidate | Product | Version  | Condition | Objective | Prerequisites | Decision    |
| --------- | ------- | -------- | --------- | --------- | ------------- | ----------- |
| A         | Match   | Match    | Match     | Match     | Satisfied     | Keep        |
| B         | Match   | Unknown  | Unknown   | Match     | Unknown       | Investigate |
| C         | Match   | Mismatch | Match     | Match     | N/A           | Reject      |

This is not a ranking system.

It is an elimination process.

The objective is to remove candidates that cannot be justified.

## Candidate A: Strong Match

A strong candidate might look like:

```text id="s8m4q2"
Product:
Match

Version:
Match

Condition:
Match

Objective:
Match

Prerequisites:
Satisfied
```

This candidate deserves deeper inspection.

It still does not guarantee success.

## Candidate B: Incomplete Evidence

Another candidate may look like:

```text id="p7n2d9"
Product:
Match

Version:
Unknown

Condition:
Unknown

Objective:
Match

Prerequisites:
Unknown
```

Do not automatically reject it.

Instead ask:

```text id="e3x6k1"
What information would allow me to decide?
```

Then gather that information.

## Candidate C: Clear Mismatch

For example:

```text id="r1q5v8"
Product:
Match

Version:
Mismatch

Condition:
Mismatch
```

Reject it.

Do not keep testing it simply because it appears in search results.

## Rejection Is Progress

A useful operator mindset is:

```text id="h9c3w7"
Rejected module
=
new information
```

If you determine:

```text id="b5m8q2"
This module requires Product X version 3,
but the target runs version 4.
```

you have learned something valuable.

You have reduced the search space.

This is better than repeatedly trying unrelated modules.

## Module Selection Confidence

Do not think only in terms of:

```text id="x7k2m5"
Yes / No
```

Instead recognize levels of certainty:

```text id="n4p8r1"
High confidence
Strong target evidence + strong module match

Medium confidence
Good product match but incomplete conditions

Low confidence
Weak evidence or broad assumptions
```

The weaker the confidence, the more you should prioritize information gathering.

## Do Not Confuse Confidence With Success Probability

A high-confidence module selection does not mean:

```text id="z6m1q4"
"The exploit will definitely work."
```

It means:

```text id="c2v9s8"
"The available evidence strongly supports that this module is appropriate to investigate."
```

Execution can still fail because of:

* Network conditions
* Configuration
* Mitigations
* Authentication
* Module limitations
* Payload problems
* Environmental differences
* Incorrect assumptions

## The Information-Gap Method

When you cannot confidently select a module, ask:

```text id="m8q4f1"
What information is preventing the decision?
```

Call this the information gap.

Example:

```text id="k7r2v5"
Question:
Does the target run vulnerable version X?

Information gap:
Exact version unknown.

Next action:
Gather version information.
```

Another example:

```text id="p3n8d6"
Question:
Is feature Y enabled?

Information gap:
Configuration unknown.

Next action:
Perform authorized enumeration.
```

This is more productive than guessing.

## Module Selection as a Decision Tree

Use:

```text id="g1c7m9"
Do I know the objective?
        │
        ├── NO → Define objective
        │
        └── YES
              ↓
        Do I have target evidence?
              │
              ├── NO → Gather evidence
              │
              └── YES
                    ↓
             Search candidates
                    ↓
             Does candidate match?
                    │
             ┌──────┴──────┐
             NO           YES
             ↓             ↓
           Reject      Check prerequisites
                             ↓
                     Are prerequisites known?
                             │
                       ┌─────┴─────┐
                      NO          YES
                      ↓            ↓
                 Gather info   Check compatibility
                                   ↓
                             Fits objective?
                                   │
                              ┌────┴────┐
                             NO        YES
                             ↓          ↓
                           Reject    Investigate
```

## Search Breadth vs Evidence Depth

There is a tradeoff between searching broadly and gathering detailed evidence.

### Broad Search

Useful when:

```text id="q8m2t5"
You know little about the target.
```

### Narrow Search

Useful when:

```text id="v4c7n1"
You have strong product/version/vulnerability evidence.
```

The goal is to progressively narrow uncertainty.

```text id="s9f3k6"
Broad evidence
    ↓
Broad search
    ↓
More evidence
    ↓
Narrower search
    ↓
Strong candidate
```

## Example: Broad Search

Suppose you only know:

```text id="w2h8p5"
HTTP service
```

You may search broadly.

But if you know:

```text id="j7m3q9"
Product
Version
Known vulnerability identifier
```

your search can become much more precise.

The second situation should produce fewer, more relevant candidates.

## Example: Multiple Plausible Exploits

Imagine your search returns:

```text id="f5n1r8"
Exploit A
Exploit B
Exploit C
```

Do not immediately execute A.

For each candidate, determine:

```text id="m2v7c4"
What condition does it require?

Does my target have that condition?

What evidence supports the conclusion?

What would disprove it?

What is the intended objective?

What additional information is needed?
```

Then eliminate candidates.

## Negative Evidence

Good module selection considers evidence against a hypothesis too.

Suppose a module requires:

```text id="a6k2s9"
Feature X enabled
```

but your enumeration shows:

```text id="z8p4m1"
Feature X disabled
```

That is useful evidence.

Do not ignore contradictory information because you want the module to work.

A strong operator asks:

```text id="q3v7d5"
What evidence would prove my current hypothesis wrong?
```

## Confirmation Bias in Module Selection

One common failure pattern is:

```text id="x1n9c6"
Find module
   ↓
Become attached to module
   ↓
Interpret ambiguous evidence as support
   ↓
Ignore contradictory evidence
```

Avoid this.

Instead use:

```text id="h4m8q2"
Hypothesis:
Module X applies.

Supporting evidence:
...

Contradicting evidence:
...

Unknown:
...

Decision:
...
```

This makes your reasoning explicit.

## Practical Exercise 1 — Candidate Elimination

### Objective

Practice selecting from multiple candidates.

### Scenario

Your authorized lab provides:

```text id="p6c2v8"
Target:
192.168.56.101

Service:
Known

Product:
Known

Version:
Known

Objective:
Validate a specific condition
```

### Task

Search Metasploit and identify at least three plausible candidates.

Create:

```text id="y8r3m1"
Candidate 1:
Evidence match:
Prerequisites:
Compatibility:
Objective match:
Decision:

Candidate 2:
Evidence match:
Prerequisites:
Compatibility:
Objective match:
Decision:

Candidate 3:
Evidence match:
Prerequisites:
Compatibility:
Objective match:
Decision:
```

Your final decision should include:

```text id="w4n7q9"
Why the selected candidate fits.
Why the rejected candidates do not.
```

## Practical Exercise 2 — Information Gap

### Scenario

You find a candidate module that appears relevant.

However:

```text id="d2k8s5"
Exact product version is unknown.
```

### Task

Do not select the module yet.

Write:

```text id="n6p1v3"
Current evidence:
...

Unknown:
...

Why the unknown matters:
...

How I can obtain the information:
...

What I will do after obtaining it:
...
```

The purpose is to practice stopping at the correct point.

## Practical Exercise 3 — Contradictory Evidence

### Scenario

A module appears to match the product and version.

However, the module requires a feature that your enumeration suggests is disabled.

### Task

Answer:

```text id="f9m4x7"
What evidence supports the module?

What evidence contradicts it?

Which evidence is stronger?

What should be investigated next?

Should exploitation happen immediately?
```

The goal is not to force a yes/no answer.

The goal is to reason correctly under uncertainty.

## Practical Exercise 4 — Objective Mismatch

### Scenario

You have a module that may obtain code execution.

Your engagement objective is:

```text id="c5v2n8"
Determine whether the target is vulnerable.
```

### Task

Identify:

```text id="r7m1q4"
What evidence could satisfy the objective without exploitation?

When would exploitation become justified?

What risks or scope considerations should be reviewed first?
```

This teaches the distinction between:

```text id="x3k9p6"
validation
```

and:

```text id="t8w2c5"
exploitation
```

## Practical Exercise 5 — Build a Selection Record

For one real lab module, document:

```text id="v1q7m4"
Objective:
Target:
Evidence:
Search terms:
Candidates:
Rejected candidates:
Selected candidate:
Why selected:
Prerequisites:
Known:
Unknown:
Configuration requirements:
Validation method:
Expected result:
Actual result:
Next action:
```

This becomes the foundation for professional documentation.

## Common Selection Mistakes

### Mistake 1 — Selecting by Name Alone

A module name may contain the correct product but still target the wrong condition.

Use:

```text id="m8f2r6"
name
+
description
+
references
+
requirements
+
target compatibility
```

### Mistake 2 — Selecting by Rank Alone

Rank is useful context.

It is not target-specific proof.

Use:

```text id="p4n7x1"
evidence
+
compatibility
+
requirements
```

to make the decision.

### Mistake 3 — Ignoring Version

Same product does not mean same vulnerability.

Always verify version when the module depends on it.

### Mistake 4 — Ignoring Configuration

A vulnerable version may still require a particular feature or configuration.

Investigate the condition.

### Mistake 5 — Ignoring Architecture

A successful exploit path and a compatible payload are separate questions.

Check both.

### Mistake 6 — Ignoring Objective

Technical applicability does not automatically mean operational necessity.

Ask what the engagement actually requires.

### Mistake 7 — Continuing After Strong Contradictory Evidence

If your evidence shows the prerequisite is absent:

```text id="j6r3v8"
stop
```

or:

```text id="s2n9c5"
gather more information
```

Do not keep forcing the hypothesis.

## Professional Selection Pattern

Use this sequence:

```text id="w5q1m8"
1. Define objective.
2. Record target evidence.
3. Identify information gaps.
4. Search for candidate functionality.
5. Read candidate modules.
6. Compare prerequisites.
7. Compare target compatibility.
8. Compare objective compatibility.
9. Identify supporting evidence.
10. Identify contradictory evidence.
11. Reject unsuitable candidates.
12. Select the candidate with the strongest evidence fit.
13. Configure only after selection.
14. Validate before intrusive execution.
```

## The Most Important Question

Whenever you are about to select a module, ask:

```text id="x8c4n2"
"What do I know that makes me believe this module applies?"
```

If the answer is:

```text id="q5m7v1"
"It appeared in search."
```

you do not yet have a sufficient reason.

A stronger answer looks like:

```text id="n3r9k6"
"The target is running the product and version
this module targets, the required feature is present,
the vulnerability condition matches the available evidence,
and the module's objective matches the authorized test objective."
```

That is defensible module selection.

## Completion Checklist

Before moving to module validation, confirm that you can:

```text id="d7p2m9"
[ ] Define the objective before selecting a module.
[ ] Record target evidence.
[ ] Distinguish strong and weak evidence.
[ ] Identify information gaps.
[ ] Search for multiple candidates.
[ ] Compare candidate modules.
[ ] Check product compatibility.
[ ] Check version compatibility.
[ ] Check platform and architecture where relevant.
[ ] Check protocol and configuration requirements.
[ ] Check module prerequisites.
[ ] Compare the module against the actual objective.
[ ] Identify contradictory evidence.
[ ] Reject unsuitable modules.
[ ] Explain why a selected module fits.
[ ] Know when more enumeration is needed.
[ ] Avoid forcing a module to work.
```

## Key Mental Model

Remember:

```text id="p9x3m7"
EVIDENCE
   ↓
OBJECTIVE
   ↓
CANDIDATES
   ↓
PREREQUISITES
   ↓
COMPATIBILITY
   ↓
SUPPORTING / CONTRADICTORY EVIDENCE
   ↓
SELECTION
```

And:

```text id="k4m8q1"
A plausible module
is not automatically
an appropriate module.
```

The correct module is the one whose assumptions and requirements best align with the evidence and authorized objective.

## Next Step

The next file is:

```text id="6z3p8v"
03-modules/03-module-validation.md
```

That file will complete the module section by focusing on **validation before execution**: how to establish whether your assumptions are sufficiently supported, how `check` fits into the workflow, how to interpret inconclusive results, and how to decide whether to proceed, gather more evidence, or stop.
