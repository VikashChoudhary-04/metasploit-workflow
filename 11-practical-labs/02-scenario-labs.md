# Scenario-Based Metasploit Labs

## Objective

Move from guided execution to independent decision-making.

The previous labs told you what workflow to follow.

These scenarios give you:

```text
TARGET
OBJECTIVE
RULES
```

You must determine:

```text
WHAT TO INVESTIGATE
      ↓
WHAT INFORMATION IS MISSING
      ↓
WHICH TOOL TO USE
      ↓
WHICH CAPABILITY IS RELEVANT
      ↓
WHICH MODULE TO CONSIDER
      ↓
HOW TO VALIDATE
      ↓
HOW TO EXECUTE
      ↓
HOW TO VERIFY
      ↓
WHAT EVIDENCE TO COLLECT
      ↓
WHEN TO STOP
```

The purpose is to make the workflow transferable to unfamiliar targets.

## Scenario Rules

All scenarios should be performed only against intentionally vulnerable systems or environments you are explicitly authorized to test.

For every scenario:

```text
[ ] Confirm scope
[ ] Define the objective
[ ] Record assumptions
[ ] Gather evidence
[ ] Avoid unnecessary actions
[ ] Verify results
[ ] Document reasoning
[ ] Clean up
```

Do not search for the "expected answer" before forming your own hypothesis.

## Scenario Difficulty Model

The scenarios progressively remove information.

```text
Scenario 1
Known service + clear objective
        ↓
Scenario 2
Known service + ambiguous objective
        ↓
Scenario 3
Unknown service + clear objective
        ↓
Scenario 4
Multiple possible attack paths
        ↓
Scenario 5
Failure during exploitation
        ↓
Scenario 6
Session obtained + unclear next step
        ↓
Scenario 7
Metasploit may not be the right tool
        ↓
Scenario 8
Multi-tool workflow
        ↓
Scenario 9
Conflicting evidence
        ↓
Scenario 10
Independent assessment
```

## Scenario 1 — Known Service, Clear Objective

### Situation

You are given an authorized lab target.

You already know:

```text
Host:
<lab target>

Service:
<known service>

Version:
<known version>

Vulnerability:
<known lab vulnerability>
```

Objective:

```text
Determine whether the vulnerability can be exploited
and demonstrate the resulting authorized impact.
```

### Your Task

Determine the workflow yourself.

You should decide:

```text
Which Metasploit capability is relevant?
Which module should be investigated?
What does the module require?
Which payload is appropriate?
How will you validate the result?
What evidence is sufficient?
```

Do not begin by copying a complete command sequence.

### Expected Reasoning

Your reasoning should resemble:

```text
Known vulnerability
      ↓
Search for capability
      ↓
Read candidate module
      ↓
Confirm compatibility
      ↓
Configure
      ↓
Validate
      ↓
Execute
      ↓
Verify
      ↓
Evidence
      ↓
Cleanup
```

### Success Criteria

You can explain every major decision.

## Scenario 2 — Known Service, Ambiguous Objective

### Situation

You are told:

```text
Target:
<authorized lab target>

Service:
<known service>

Version:
<known version>

Instruction:
"Test this service."
```

### Problem

The instruction is incomplete.

### Your Task

Do not immediately exploit the service.

Determine what must be clarified.

Ask:

```text
What exactly should be tested?
Is vulnerability discovery required?
Is exploitation authorized?
What impact should be demonstrated?
What evidence is required?
Are there restrictions on service disruption?
```

### Success Criteria

You recognize that technical capability cannot compensate for an undefined objective.

## Scenario 3 — Unknown Service, Clear Objective

### Situation

You receive:

```text
Target:
<authorized lab target>

Objective:
Identify whether the host exposes a remotely exploitable service
and validate one finding safely.
```

No service information is provided.

### Your Task

Determine the minimum information required before exploitation.

Your first questions should include:

```text
Which ports are open?
Which services are running?
What versions are present?
Which service is relevant?
What evidence suggests a vulnerability?
```

### Constraint

Do not begin with Metasploit exploitation modules.

### Success Criteria

You identify reconnaissance as the first phase.

## Scenario 4 — Multiple Possible Attack Paths

### Situation

Enumeration reveals:

```text
Service A
Service B
Web application
Remote administration service
```

Several possible vulnerabilities appear.

### Objective

Demonstrate one authorized security impact with the least unnecessary activity.

### Your Task

Create an attack-path decision table:

| Candidate      | Evidence | Required assumptions | Validation method | Potential impact | Next action |
| -------------- | -------- | -------------------- | ----------------- | ---------------- | ----------- |
| A              |          |                      |                   |                  |             |
| B              |          |                      |                   |                  |             |
| Web            |          |                      |                   |                  |             |
| Remote service |          |                      |                   |                  |             |

Do not simply choose the path that has the most interesting exploit.

Choose based on:

```text
Evidence
+
Objective relevance
+
Compatibility
+
Validation confidence
+
Operational safety
```

### Success Criteria

You can justify why you investigated one path before another without turning the exercise into a ranking of vulnerabilities.

## Scenario 5 — Exploitation Failure

### Situation

You have:

```text
Correct target
Correct service
Relevant module
Apparently compatible configuration
```

The expected result does not occur.

### Your Task

Do not immediately change the module.

Describe the failure precisely.

For example:

```text
"The module executed but no session was established."
```

is more useful than:

```text
"It doesn't work."
```

### Investigation

Classify the failure:

```text
Module?
Target?
Configuration?
Payload?
Handler?
Network?
Session?
Target state?
```

Then create a hypothesis.

```text
Hypothesis:
The callback path is not reachable from the target.

Change:
<one evidence-based change>

Retest:
<result>
```

### Success Criteria

You can show a logical troubleshooting chain.

## Scenario 6 — Session Obtained, Objective Unclear

### Situation

An authorized exploit produces a session.

The original objective was:

```text
Determine whether the target can be compromised through
the identified vulnerability.
```

### Your Task

Determine what evidence is now necessary.

Ask:

```text
Did the session prove code execution?
Under which account?
With what privilege?
Does this satisfy the original objective?
```

If sufficient evidence exists:

```text
VERIFY
  ↓
DOCUMENT
  ↓
CLEANUP
  ↓
STOP
```

### Important Constraint

Do not continue exploring simply because the session gives you additional capabilities.

### Success Criteria

You demonstrate restraint.

## Scenario 7 — Metasploit May Not Be the Right Tool

### Situation

Your objective is:

```text
Determine whether an authenticated web application
is vulnerable to a specific input-handling flaw.
```

You have:

```text
A browser
A test account
An authorized application
```

### Your Task

Decide what capability is required.

Possible needs:

```text
HTTP interception
Request modification
Response comparison
Authentication/session handling
Application workflow analysis
```

Determine whether Metasploit or another tool provides the most appropriate control.

### Success Criteria

You can justify the tool choice based on capability.

The goal is not to force Metasploit into the exercise.

## Scenario 8 — Multi-Tool Workflow

### Situation

An authorized lab assessment requires:

```text
Network discovery
Service identification
Web application analysis
Vulnerability validation
Controlled exploitation
Evidence collection
```

### Your Task

Design the workflow before executing it.

Your plan should resemble:

```text
DISCOVERY
    ↓
SERVICE ENUMERATION
    ↓
APPLICATION ANALYSIS
    ↓
VULNERABILITY IDENTIFICATION
    ↓
VALIDATION
    ↓
METASPLOIT IF APPROPRIATE
    ↓
CONTROLLED EXPLOITATION
    ↓
VERIFICATION
    ↓
EVIDENCE
    ↓
CLEANUP
```

### Constraint

Do not run every available tool just because it exists.

For each tool, write:

```text
Tool:
<name>

Question it answers:
<question>

Information produced:
<information>

Why it is needed:
<reason>
```

### Success Criteria

Every tool has a defined purpose.

## Scenario 9 — Conflicting Evidence

### Situation

You have three pieces of information:

```text
Scanner:
Vulnerability appears present.

Service enumeration:
Version appears compatible.

Metasploit validation:
Result is inconclusive.
```

### Your Task

Do not declare the vulnerability exploitable.

Instead ask:

```text
Which evidence is strongest for the specific question?
What does each result actually prove?
What assumption remains unresolved?
What additional evidence is needed?
```

### Evidence Model

```text
OBSERVATION
    ↓
INTERPRETATION
    ↓
HYPOTHESIS
    ↓
VALIDATION
    ↓
CONCLUSION
```

Do not skip the validation stage.

### Success Criteria

Your conclusion reflects the evidence rather than the most convenient result.

## Scenario 10 — Stale Information

### Situation

Your Metasploit database contains:

```text
Host:
<lab target>

Service:
<old service information>

Version:
<old version>
```

Current enumeration shows different information.

### Your Task

Determine which information should drive your next decision.

Ask:

```text
Which data is newer?
What changed?
Is the database record still useful?
Should it be updated?
```

### Key Principle

```text
DATABASE RECORD
    ≠
CURRENT TARGET STATE
```

### Success Criteria

You recognize stale information before using it to select an exploit.

## Scenario 11 — Payload Compatibility Problem

### Situation

The exploit appears appropriate, but your chosen payload is incompatible with the target conditions.

### Your Task

Do not randomly rotate payloads.

Determine:

```text
Target OS
Target architecture
Module compatibility
Required session type
Network direction
Callback path
```

Then choose a compatible payload based on evidence.

### Success Criteria

You can explain the payload selection as a compatibility decision.

## Scenario 12 — Exploit Output Without Proof

### Situation

The console reports an apparently successful exploitation attempt.

However:

```text
No session appears.
No independent target evidence exists.
```

### Your Task

Determine whether you can honestly conclude:

```text
"The target was successfully exploited."
```

### Required Reasoning

Separate:

```text
Exploit attempt
    ↓
Observed console output
    ↓
Session / target-side evidence
    ↓
Verification
    ↓
Conclusion
```

### Success Criteria

You do not confuse an optimistic message with verified impact.

## Scenario 13 — Objective Satisfied Before Exploitation

### Situation

Your objective is:

```text
Determine whether the target is vulnerable to a specific issue.
```

You obtain sufficient authorized evidence through non-destructive validation.

### Your Task

Ask:

```text
Is exploitation actually necessary?
```

If the objective is already satisfied:

```text
DOCUMENT
  ↓
CLEANUP
  ↓
STOP
```

### Key Principle

```text
EXPLOITATION IS A MEANS,
NOT AUTOMATICALLY THE OBJECTIVE.
```

## Scenario 14 — Potentially Destructive Path

### Situation

A candidate module may affect target availability.

Your objective can be satisfied through a lower-impact validation method.

### Your Task

Before execution, compare:

```text
Required evidence
      vs.
Potential impact
```

Ask:

```text
Can the objective be satisfied without the higher-impact action?
Is the higher-impact action explicitly authorized?
Is the expected impact understood?
Is there a rollback/cleanup plan?
```

### Success Criteria

You choose the least unnecessary impact consistent with the authorized objective.

## Scenario 15 — Session Instability

### Situation

A session appears and then terminates.

### Your Task

Investigate systematically.

Possible branches:

```text
Payload compatibility
Target process stability
Network interruption
Session transport
Target resource constraints
Privilege/context
```

Record:

```text
Observed behavior:
<what happened>

Hypothesis:
<what might explain it>

Evidence:
<what supports the hypothesis>

Change:
<one change>

Result:
<observation>

Conclusion:
<what changed in your understanding>
```

### Success Criteria

You diagnose rather than repeatedly reconnect without understanding the failure.

## Scenario 16 — Multiple Sessions, One Objective

### Situation

You have multiple authorized sessions:

```text
Session 1 → Host A
Session 2 → Host B
Session 3 → Host A
```

Objective:

```text
Collect evidence from Host A demonstrating the authorized impact.
```

### Your Task

Determine:

```text
Which session is relevant?
Which session provides the required context?
Which sessions can be left untouched?
Which sessions should be closed?
```

### Success Criteria

You manage sessions by objective rather than by availability.

## Scenario 17 — Post-Exploitation Boundary

### Situation

You obtain an authorized privileged session.

The original objective has been satisfied.

You notice several additional actions that could be performed.

### Your Task

For each possible action, classify it:

| Action | Necessary for objective? | Authorized? | Evidence value | Perform? |
| ------ | -----------------------: | ----------: | -------------: | -------: |
|        |                          |             |                |          |
|        |                          |             |                |          |
|        |                          |             |                |          |

The key question is:

```text
"Why am I doing this?"
```

If the answer is:

```text
"Because I can."
```

that is not sufficient.

## Scenario 18 — No Suitable Metasploit Module

### Situation

You identify a vulnerability, but your Metasploit search does not reveal an appropriate module.

### Your Task

Investigate:

```text
Is the vulnerability identification correct?
Is the search sufficiently broad?
Does Metasploit support this vulnerability?
Does another tool provide a better validation method?
```

### Do Not

```text
Force an unrelated module
```

or:

```text
Assume the vulnerability is absent
```

because no module was found.

### Success Criteria

You separate:

```text
No Metasploit module
```

from:

```text
No vulnerability
```

## Scenario 19 — Repeatable Assessment

### Situation

You must perform the same low-risk initial workflow against several authorized lab hosts.

### Task

Identify which steps can be safely automated.

Create:

```text
Automate:
<repetitive steps>

Keep manual:
<judgment-heavy steps>

Reason:
<why>
```

### Principle

```text
AUTOMATE REPETITION.
RETAIN HUMAN JUDGMENT.
```

### Success Criteria

Automation reduces repetition without hiding important decisions.

## Scenario 20 — Full Scenario

### Situation

You receive an unfamiliar authorized lab network.

You are given:

```text
Scope:
Specified lab network only.

Objective:
Identify one meaningful security weakness and demonstrate
its authorized impact with sufficient evidence.

Restrictions:
No unnecessary destructive activity.
No testing outside scope.
Clean up after testing.
Document reasoning.
```

No module name is provided.

No payload is provided.

No command sequence is provided.

### Your Task

Perform the assessment independently.

Your workflow should emerge naturally:

```text
SCOPE
  ↓
DISCOVERY
  ↓
TARGET MODEL
  ↓
OBJECTIVE
  ↓
INFORMATION GAPS
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
```

### Success Criteria

You can complete the scenario without needing:

```text
A module name
A payload name
A command sequence
A vulnerability name
A predetermined attack path
```

You should be able to explain the reasoning behind your decisions.

## Scenario Debrief Template

After every scenario, answer:

```text
# Scenario Debrief

## Objective

<what I needed to accomplish>

## Initial Knowledge

<what I knew>

## Missing Information

<what I needed to discover>

## Initial Hypothesis

<what I believed and why>

## Tools Selected

<tools and reasons>

## Metasploit Decision

<why Metasploit was or was not appropriate>

## Module Decision

<module considered and reasoning>

## Requirements

<important prerequisites>

## Configuration

<important configuration decisions>

## Validation

<how assumptions were tested>

## Execution

<what happened>

## Verification

<how the result was confirmed>

## Troubleshooting

<failures and reasoning>

## Post-Exploitation

<objective-specific actions>

## Evidence

<what proves the conclusion>

## Cleanup

<what was removed or restored>

## What I Would Change

<one or more improvements for a future attempt>
```

## Scenario Scoring Framework

Do not score yourself primarily on whether exploitation succeeded.

Evaluate your process.

| Skill              | Question                                        |
| ------------------ | ----------------------------------------------- |
| Scope              | Did I stay within authorization?                |
| Reconnaissance     | Did I gather sufficient information?            |
| Objective          | Did I define what success meant?                |
| Tool selection     | Did I choose tools based on capability?         |
| Module selection   | Did I have evidence for the choice?             |
| Configuration      | Could I explain important options?              |
| Validation         | Did I test assumptions?                         |
| Execution          | Was the action controlled?                      |
| Interpretation     | Did I distinguish output from proof?            |
| Troubleshooting    | Did I use evidence rather than randomness?      |
| Session management | Did I track context correctly?                  |
| Post-exploitation  | Did I remain objective-driven?                  |
| Evidence           | Can the conclusion be independently understood? |
| Cleanup            | Did I leave the lab appropriately?              |
| Decision-making    | Can I explain why I stopped or continued?       |

A failed exploit with excellent reasoning can be more educational than a successful exploit obtained through blind command copying.

## Common Mistakes

### Mistake 1 — Looking for the Answer First

Do not search for a write-up before attempting the scenario.

Your first attempt should reveal what you actually understand.

### Mistake 2 — Treating Every Scenario as an Exploitation Exercise

Some scenarios are deliberately designed to test whether you know when **not** to exploit.

### Mistake 3 — Confusing Evidence With Assumption

Write down:

```text
FACT
ASSUMPTION
HYPOTHESIS
CONCLUSION
```

separately.

### Mistake 4 — Over-Enumerating

More data is not always better.

Gather the information needed to answer the current question.

### Mistake 5 — Continuing After Success

Once the objective is satisfied:

```text
VERIFY
→ DOCUMENT
→ CLEANUP
→ STOP
```

### Mistake 6 — Random Troubleshooting

Use:

```text
OBSERVATION
→ HYPOTHESIS
→ ONE CHANGE
→ RETEST
```

### Mistake 7 — Tool Attachment

Do not force Metasploit into a workflow where another tool provides better capability.

## Increasing Difficulty

Repeat the scenarios while progressively removing guidance.

### Pass 1 — Guided Reasoning

Use the repository decision trees.

### Pass 2 — Reduced Guidance

Write only:

```text
Objective
Known information
Missing information
Next action
```

### Pass 3 — Minimal Notes

Write only:

```text
Objective
Decision
Evidence
Result
```

### Pass 4 — Independent

Start with only:

```text
Scope
Objective
Rules
```

Everything else must come from your own investigation.

## Final Scenario Checklist

Before considering the scenario complete:

* [ ] Scope was confirmed.
* [ ] Target identity was verified.
* [ ] Objective was measurable.
* [ ] Missing information was identified.
* [ ] Reconnaissance was purposeful.
* [ ] Tool selection was justified.
* [ ] Metasploit was used only when appropriate.
* [ ] Module selection was evidence-based.
* [ ] Module requirements were understood.
* [ ] Configuration was deliberate.
* [ ] Validation was performed where appropriate.
* [ ] Execution was controlled.
* [ ] Results were interpreted correctly.
* [ ] Success was independently verified.
* [ ] Sessions were managed deliberately.
* [ ] Post-exploitation remained objective-driven.
* [ ] Evidence was collected.
* [ ] Failures were troubleshot systematically.
* [ ] Cleanup was completed.
* [ ] The final conclusion matches the evidence.

## Key Mental Model

```text
A SCENARIO IS NOT ASKING:

"Can you run Metasploit?"

It is asking:

"Can you reason your way from an objective
to a defensible security conclusion?"
```

The final transition should look like:

```text
GUIDED INSTRUCTIONS
       ↓
SCENARIO
       ↓
DECISION
       ↓
ACTION
       ↓
EVIDENCE
       ↓
CONCLUSION
```

The less information the scenario gives you, the more important your reasoning becomes.

## Next Step

Continue to:

`11-practical-labs/03-independent-assessment.md`

That file removes the scenario scaffolding and establishes independent assessment exercises where you receive only the scope, objective, and rules.
