# Independent Metasploit Assessment

## Objective

Demonstrate that you can use Metasploit as an operator rather than as a command executor.

At this stage, the repository stops telling you:

```text
Which module to use
Which payload to choose
Which command to run
Which attack path to follow
```

You receive only:

```text
SCOPE
OBJECTIVE
RULES
```

You must build the workflow yourself.

The assessment measures **decision quality**, not the number of commands executed.

## Assessment Philosophy

The final practical stage should answer one question:

```text id="k1w5bh"
Can I independently move from an unfamiliar authorized target
to a defensible security conclusion?
```

The expected reasoning is:

```text id="l3q6jx"
SCOPE
  ↓
TARGET MODEL
  ↓
OBJECTIVE
  ↓
INFORMATION GAPS
  ↓
RECONNAISSANCE
  ↓
HYPOTHESIS
  ↓
CAPABILITY
  ↓
TOOL
  ↓
MODULE
  ↓
REQUIREMENTS
  ↓
CONFIGURATION
  ↓
VALIDATION
  ↓
EXECUTION
  ↓
VERIFICATION
  ↓
SESSION
  ↓
POST-EXPLOITATION
  ↓
EVIDENCE
  ↓
CLEANUP
  ↓
CONCLUSION
```

You may use the repository as a reference.

You may **not** turn the repository into a predetermined command sequence.

## Assessment Rules

Perform these assessments only against intentionally vulnerable systems or environments you are explicitly authorized to test.

### Required Rules

```text id="qf8k8e"
1. Stay inside the defined scope.

2. Do not attack systems outside the lab.

3. Do not perform unnecessary destructive actions.

4. Do not assume exploitation is automatically required.

5. Verify important conclusions.

6. Collect only evidence necessary for the objective.

7. Stop when the objective is satisfied.

8. Clean up authorized test artifacts.

9. Document your reasoning.

10. If the current tool is inappropriate, change tools.
```

## What You Are Allowed to Use

You may use:

* Metasploit Framework
* Nmap
* Burp Suite
* Wireshark
* vulnerability scanners
* appropriate enumeration tools
* official documentation
* your own notes
* this repository as a reference

The assessment is about **tool selection and reasoning**, so you should not artificially restrict yourself to Metasploit.

## What You Should Not Receive

For the final assessment, do not receive:

```text id="d3tr4n"
Module names
Payload names
Exploit names
Command sequences
Step-by-step attack paths
Expected session types
Expected answers
```

If another person is administering the assessment, they should provide only the information explicitly specified in each exercise.

## Assessment 1 — Single-Target Independent Assessment

### Scope

```text id="8x7qgk"
One intentionally vulnerable lab machine.
```

### Objective

```text id="1c5m8w"
Identify one meaningful security weakness and determine whether
its impact can be demonstrated safely within the lab.
```

### Rules

```text id="j1gq4p"
Only the specified machine is in scope.

Do not perform unnecessary destructive actions.

Document your reasoning.

Collect sufficient evidence.

Clean up afterward.
```

### Starting Information

You receive only:

```text id="w7x2sd"
Target:
<lab IP or hostname>
```

Everything else must be discovered.

### Expected Independent Process

You should determine for yourself:

```text id="f3c5by"
What is exposed?
What services are running?
Which findings are relevant?
What should be investigated first?
Is Metasploit appropriate?
Which capability is required?
How should the finding be validated?
What constitutes proof?
```

### Deliverable

Produce an assessment report using the template below.

### Success Criteria

You successfully:

* remain within scope
* identify useful target information
* define a concrete finding
* select an appropriate validation method
* use Metasploit appropriately where applicable
* verify the result
* collect evidence
* stop at the appropriate point
* clean up

## Assessment 2 — Multiple-Service Target

### Scope

```text id="8m1c7h"
One intentionally vulnerable lab machine.
```

### Objective

```text id="px3t4d"
Identify the most relevant security-testing path for the stated
assessment objective and demonstrate one authorized impact.
```

### Starting Information

Only the target is provided.

The machine may expose several services.

### Challenge

You may encounter:

```text id="k1y5h9"
Multiple open ports
Multiple applications
Multiple potential findings
Different technologies
Different possible attack paths
```

### Your Task

Build a target model before selecting an exploitation path.

Your notes should answer:

```text id="j8r6g0"
What is exposed?

What appears interesting?

What evidence supports each hypothesis?

Which path directly relates to the objective?

What information is still missing?

Which validation method is appropriate?
```

### Success Criteria

You do not simply select the first apparent vulnerability.

You demonstrate deliberate path selection based on evidence and objective relevance.

## Assessment 3 — Metasploit-Required Decision

### Scope

```text id="g7n4d2"
One intentionally vulnerable service in an isolated lab.
```

### Objective

```text id="q8v3f1"
Determine whether the identified vulnerability can be safely
validated using an appropriate exploitation framework.
```

### Starting Information

You receive:

```text id="m3p9z7"
Target
Service
Version
Known vulnerability reference
```

### Your Task

Determine:

```text id="b5k2r8"
Whether Metasploit supports the required capability.

Which module is relevant.

What requirements must be satisfied.

Which configuration is necessary.

How success will be verified.
```

The assessment administrator should not provide the module name.

### Success Criteria

You demonstrate the complete:

```text id="5a9w4v"
SEARCH
→ READ
→ REQUIREMENTS
→ CONFIGURE
→ VALIDATE
→ EXECUTE
→ VERIFY
```

workflow independently.

## Assessment 4 — Metasploit Is Not the Answer

### Scope

```text id="s8d2k1"
One intentionally vulnerable web application in an isolated lab.
```

### Objective

```text id="f4v7r2"
Determine whether a specific application behavior creates
a security weakness and provide evidence.
```

### Starting Information

You receive:

```text id="y3q9c6"
Application URL
Authorized test account
Objective
```

No Metasploit module is provided.

### Your Task

Decide:

```text id="d8w2m4"
What capability is required?

What information must be collected?

Which tool provides the necessary visibility?

Is exploitation necessary?

What evidence establishes the conclusion?
```

### Success Criteria

You do not force the task into Metasploit.

The correct decision may be to use another tool throughout the exercise.

## Assessment 5 — Failed Exploitation

### Scope

```text id="q7v4m8"
One intentionally vulnerable lab target.
```

### Objective

```text id="k2f8d3"
Validate a known vulnerability and demonstrate its authorized impact.
```

### Starting Information

You receive enough information to identify a likely vulnerable service.

During testing, the expected result does not occur.

### Your Task

Troubleshoot independently.

You must document:

```text id="n6x1r5"
Observed failure
    ↓
Failure classification
    ↓
Hypothesis
    ↓
Evidence
    ↓
One change
    ↓
Retest
    ↓
Observation
    ↓
Conclusion
```

### Constraint

Do not randomly change:

```text id="w8q3c1"
Module
Target
Payload
Callback settings
Ports
Network configuration
```

without a hypothesis.

### Success Criteria

Your report demonstrates how your understanding changed after each test.

A successful final exploitation is useful, but the quality of the troubleshooting process matters more.

## Assessment 6 — Session and Post-Exploitation

### Scope

```text id="p4n7y2"
An authorized vulnerable lab target.
```

### Objective

```text id="c8v5k3"
Demonstrate the security impact of the identified vulnerability
and determine the context under which code execution occurs.
```

### Starting Information

You are given a target and an objective.

### Your Task

After obtaining a session, determine:

```text id="m7q2s4"
Which host?
Which user/context?
Which privilege level?
Is the session relevant to the objective?
What evidence proves the impact?
```

Then stop when the objective is satisfied.

### Success Criteria

You do not perform unrelated post-exploitation.

## Assessment 7 — Evidence-First Assessment

### Scope

```text id="r5k8d1"
One authorized lab target.
```

### Objective

```text id="t2m6p9"
Produce sufficient evidence to demonstrate one security finding
and its authorized impact.
```

### Special Rule

Before testing, define what evidence would constitute success.

Write:

```text id="j4w7s3"
Finding:
<what I am trying to establish>

Required evidence:
<evidence needed>

Verification method:
<how I will confirm it>

Stopping condition:
<when I will stop>
```

Then perform the assessment.

### Success Criteria

Your evidence plan existed before exploitation.

## Assessment 8 — Multi-Tool Independent Assessment

### Scope

```text id="v9c3x7"
Authorized isolated lab environment.
```

### Objective

```text id="n6h4k2"
Identify one meaningful vulnerability, validate it, and demonstrate
its authorized impact.
```

### Available Tools

You may use:

```text id="m3y7q1"
Nmap
Metasploit
Burp Suite
Wireshark
Vulnerability scanners
Web enumeration tools
Other appropriate authorized tools
```

### Task

You decide the workflow.

You must explain:

```text id="b8r4c2"
Why each tool was used.
What question each tool answered.
What evidence each tool produced.
Why you switched tools.
Why you stopped.
```

### Success Criteria

The final report demonstrates a coherent toolchain rather than a collection of unrelated scans.

## Assessment 9 — Stale and Conflicting Data

### Scope

```text id="h5m2z8"
Authorized lab environment.
```

### Objective

```text id="c7r3p6"
Determine whether a suspected vulnerability is currently relevant.
```

### Starting Information

You receive:

```text id="y8k4m1"
An old scan
A current scan
A Metasploit database entry
A service banner
```

Some information intentionally conflicts.

### Your Task

Determine:

```text id="q6v9b2"
Which evidence is current?

Which evidence is authoritative for the specific question?

What assumptions are invalid?

What should be re-enumerated?

What should be trusted only after validation?
```

### Success Criteria

You do not blindly use stale database information.

## Assessment 10 — Final Independent Assessment

### Scope

```text id="r8c5n2"
An unfamiliar intentionally vulnerable lab environment.
```

### Objective

```text id="x4m7k1"
Identify one meaningful security weakness and demonstrate its
authorized impact while producing sufficient evidence for
another operator to reproduce your reasoning.
```

### Rules

```text id="p3q8d5"
No module hints.

No payload hints.

No command hints.

No predetermined attack path.

No unnecessary destructive actions.

No testing outside scope.

Document important decisions.

Verify your conclusion.

Clean up afterward.
```

### Starting Information

Only provide:

```text id="f9w2c6"
Scope
Objective
Rules
```

Nothing else.

### Expected Operator Behavior

The operator should independently determine:

```text id="k3r7v5"
What to enumerate
What information matters
Which findings deserve attention
Which hypotheses are reasonable
Which tools are appropriate
Whether Metasploit is appropriate
Which module to investigate
What requirements exist
How to configure the workflow
How to validate assumptions
How to execute safely
How to verify success
What post-exploitation is necessary
What evidence is sufficient
When to stop
How to clean up
```

## Final Assessment Report

Use this structure for every independent assessment.

```text id="1u6zq8"
# Independent Assessment Report

## 1. Scope

<authorized target, network, and boundaries>

## 2. Objective

<exact objective>

## 3. Rules and Constraints

<allowed and prohibited actions>

## 4. Initial Knowledge

<information provided before testing>

## 5. Reconnaissance

<important discoveries>

## 6. Target Model

<services, technologies, versions, and relevant observations>

## 7. Information Gaps

<what remained unknown>

## 8. Hypotheses

<security hypotheses and supporting evidence>

## 9. Tool Selection

<tools selected and why>

## 10. Metasploit Decision

<why Metasploit was or was not appropriate>

## 11. Module / Capability Selection

<module or alternative capability and reasoning>

## 12. Requirements

<requirements and supporting evidence>

## 13. Configuration

<important configuration and reasons>

## 14. Validation

<validation performed and interpretation>

## 15. Execution

<controlled actions performed>

## 16. Result

<what actually happened>

## 17. Verification

<evidence confirming or disproving the result>

## 18. Session

<session details relevant to the objective, if applicable>

## 19. Post-Exploitation

<objective-specific actions only>

## 20. Evidence

<evidence supporting the conclusion>

## 21. Troubleshooting

<failures, hypotheses, changes, and observations>

## 22. Cleanup

<authorized test artifacts removed or restored>

## 23. Conclusion

<defensible conclusion based on evidence>

## 24. Lessons Learned

<what changed in my understanding>

## 25. Improvements

<what I would do differently next time>
```

## Independent Assessment Rules

### Rule 1 — Do Not Optimize for Exploitation

A technically successful exploit is not automatically a successful assessment.

You are being evaluated on:

```text id="q1w8e5"
Reasoning
Accuracy
Control
Verification
Evidence
Discipline
```

### Rule 2 — Separate Facts From Assumptions

Maintain this distinction:

```text id="a5d9r3"
FACT
Something directly observed or reliably established.

ASSUMPTION
Something believed but not yet verified.

HYPOTHESIS
A testable explanation for an observation.

CONCLUSION
A statement supported by sufficient evidence.
```

### Rule 3 — Do Not Treat Tool Output as Absolute Truth

Examples:

```text id="j6r2m8"
Scanner finding
    ≠
Confirmed vulnerability

Metasploit check
    ≠
Guaranteed exploitability

Exploit output
    ≠
Verified code execution

Session
    ≠
Automatically satisfied objective
```

Each result must be interpreted in context.

### Rule 4 — Change One Thing at a Time

During troubleshooting:

```text id="p7m3c9"
OBSERVE
  ↓
HYPOTHESIS
  ↓
ONE CHANGE
  ↓
RETEST
```

If you change everything at once, you lose causal information.

### Rule 5 — Stop When the Objective Is Satisfied

Use:

```text id="x8v2n6"
OBJECTIVE SATISFIED?
      │
      ├── YES
      │    ↓
      │  VERIFY
      │    ↓
      │  DOCUMENT
      │    ↓
      │  CLEANUP
      │    ↓
      │  STOP
      │
      └── NO
           ↓
        DEFINE NEXT OBJECTIVE
```

Do not continue because access remains available.

## Independent Assessment Maturity Levels

### Level 1 — Procedural

You can follow instructions.

```text id="m4n8q2"
Instruction → Command → Result
```

This is not enough.

### Level 2 — Guided Operator

You understand the workflow but still depend on prompts.

```text id="v5c7d1"
Objective → Guided Decision → Action → Result
```

### Level 3 — Scenario Operator

You receive a scenario and choose the workflow.

```text id="n9k3r6"
Objective → Investigation → Decision → Action → Evidence
```

### Level 4 — Independent Operator

You receive only:

```text id="c2m7x4"
Scope
Objective
Rules
```

You build the entire workflow.

### Level 5 — Adaptive Operator

You can independently:

```text id="j8p4w5"
Change tools
Change hypotheses
Troubleshoot failures
Reduce unnecessary impact
Recognize insufficient evidence
Stop when appropriate
Explain every major decision
```

The final challenge should target Level 4–5 behavior.

## Self-Assessment Matrix

Rate each capability using:

```text id="y6r1k8"
0 = Cannot perform independently
1 = Can perform with substantial guidance
2 = Can perform with occasional reference
3 = Can perform independently
4 = Can explain and teach the reasoning
```

| Capability                     | Score |
| ------------------------------ | ----: |
| Scope interpretation           |       |
| Target identification          |       |
| Reconnaissance                 |       |
| Objective definition           |       |
| Information-gap analysis       |       |
| Hypothesis formation           |       |
| Tool selection                 |       |
| Metasploit search              |       |
| Module selection               |       |
| Module reading                 |       |
| Requirement analysis           |       |
| Configuration                  |       |
| Payload selection              |       |
| Validation                     |       |
| Controlled exploitation        |       |
| Result interpretation          |       |
| Verification                   |       |
| Session management             |       |
| Meterpreter usage by objective |       |
| Post-exploitation discipline   |       |
| Evidence collection            |       |
| Troubleshooting                |       |
| Database usage                 |       |
| Automation judgment            |       |
| Tool switching                 |       |
| Cleanup                        |       |
| Documentation                  |       |

The goal is not to maximize the number of commands you know.

The goal is to reach independent reasoning across the workflow.

## Failure Review

If an assessment fails, do not simply record:

```text id="r7f2q4"
"Exploit failed."
```

Perform a structured review.

### Ask

```text id="k8m3v6"
Was the target correct?

Was the service correctly identified?

Was the vulnerability assumption supported?

Was the module appropriate?

Were the requirements satisfied?

Was the configuration correct?

Was the payload compatible?

Was the network path valid?

Did execution actually occur?

Was the session expected?

Was the result independently verified?

Was another tool more appropriate?
```

Then identify the earliest incorrect assumption.

That is often more valuable than identifying the final visible error.

## Earliest-Wrong-Assumption Method

Use:

```text id="d5q9x3"
Observed Failure
      ↓
Trace Backward
      ↓
Find First Unsupported Assumption
      ↓
Correct It
      ↓
Rebuild Workflow
```

Example:

```text id="z1c6m8"
No session
   ↓
Callback failed
   ↓
Callback address incorrect
   ↓
Why was that address selected?
   ↓
Network topology was never verified
```

The visible failure was:

```text id="6n4w2j"
No session
```

The actual reasoning failure was:

```text id="e8p3r5"
Unsupported network assumption
```

This method builds transferable troubleshooting skill.

## Evidence Quality Test

Before writing your conclusion, ask:

```text id="v4k8m2"
Can another operator understand what happened?

Can they identify the target?

Can they understand the vulnerability?

Can they understand what was attempted?

Can they distinguish observation from interpretation?

Can they see what proves the conclusion?

Can they reproduce the reasoning?
```

If not, the evidence record is incomplete.

## Final Challenge Standard

You are ready for the final repository challenge when you can complete an unfamiliar authorized lab without being given:

```text id="s6n2r9"
The vulnerability
The module
The payload
The command sequence
The attack path
The troubleshooting solution
```

And you can still:

```text id="h3v7c1"
Understand the target
      ↓
Define the objective
      ↓
Identify missing information
      ↓
Choose appropriate tools
      ↓
Select and validate a capability
      ↓
Execute safely
      ↓
Interpret results
      ↓
Verify impact
      ↓
Collect evidence
      ↓
Stop appropriately
      ↓
Clean up
```

## Final Readiness Checklist

Before moving to the final challenge, confirm:

* [ ] I can work from scope rather than from a command list.
* [ ] I can build a target model independently.
* [ ] I can identify information gaps.
* [ ] I can form testable hypotheses.
* [ ] I can choose between Metasploit and another tool.
* [ ] I can search for modules without knowing their names.
* [ ] I can read and understand module requirements.
* [ ] I can configure modules deliberately.
* [ ] I can reason about payload compatibility.
* [ ] I can validate assumptions.
* [ ] I can perform controlled exploitation.
* [ ] I can interpret failures systematically.
* [ ] I can manage sessions.
* [ ] I can use Meterpreter according to an objective.
* [ ] I can perform focused post-exploitation.
* [ ] I can distinguish evidence from assumptions.
* [ ] I can verify exploitation independently.
* [ ] I can use the database without treating stale data as truth.
* [ ] I can automate repetition without automating judgment.
* [ ] I know when another tool is better.
* [ ] I know when to stop.
* [ ] I can clean up.
* [ ] I can write a defensible assessment report.

## Key Mental Model

```text id="q5m9x2"
THE FINAL SKILL IS NOT:

"Knowing Metasploit."

THE FINAL SKILL IS:

"Knowing what to do when you do not know
which Metasploit feature, module, payload,
command, or tool you need yet."
```

A mature operator can enter an unfamiliar authorized environment and create the workflow from first principles:

```text id="w7c3n8"
SCOPE
  ↓
QUESTION
  ↓
EVIDENCE
  ↓
HYPOTHESIS
  ↓
CAPABILITY
  ↓
TOOL
  ↓
ACTION
  ↓
RESULT
  ↓
VERIFICATION
  ↓
CONCLUSION
```

That is the standard this repository is designed to build.

## Next Step

Continue to:

`12-final-challenge/README.md`

This is the final challenge of the repository: a deliberately instruction-free assessment that tests the complete Metasploit operator workflow from scope and reconnaissance through exploitation, verification, evidence, troubleshooting, tool selection, and cleanup.
