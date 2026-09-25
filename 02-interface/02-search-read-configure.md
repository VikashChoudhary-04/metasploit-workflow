# Search, Read, Configure

## Objective

Learn how to turn target evidence into a defensible Metasploit module choice and a verified configuration.

The core workflow is:

```text
TARGET EVIDENCE
      ↓
SEARCH
      ↓
CANDIDATE MODULES
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
```

The goal is not to find *a* module.

The goal is to determine whether a module is appropriate for the specific target, objective, and evidence available.

## Why This Matters Professionally

A large Metasploit installation can contain many modules related to the same product, service, vulnerability, or protocol.

That creates a common failure pattern:

```text
Search result
    ↓
Looks relevant
    ↓
use
    ↓
set
    ↓
run
    ↓
failure
```

A stronger operator asks:

```text
Why does this module fit?

What evidence supports that decision?

What does the module actually require?

What assumptions am I making?

How can I validate those assumptions before execution?
```

This distinction separates module discovery from module selection.

## The Evidence-First Model

Start with what you know.

For example:

```text
Target:
192.168.56.101

Observed:
TCP/80 open

Service:
HTTP

Product:
Known or suspected

Version:
Known / unknown

Objective:
Validate a suspected vulnerability
```

Do not immediately jump to:

```text
search exploit
```

First determine what information is actually available.

The stronger your evidence, the narrower your search can become.

## Evidence Hierarchy

Not all target information has equal value.

A useful progression is:

```text
IP address
   ↓
Open port
   ↓
Protocol/service
   ↓
Product
   ↓
Version
   ↓
Specific feature/configuration
   ↓
Known vulnerability
   ↓
Confirmed applicability
```

For example:

```text
Port 80 open
```

is useful.

But:

```text
Apache HTTP Server version X.Y.Z
```

is significantly more useful for module selection.

And:

```text
Apache X.Y.Z with a specific vulnerable feature enabled
```

may provide enough evidence for a much more precise decision.

## Search Strategy

Use the strongest reliable evidence you have.

### Broad Search

When little is known:

```text
search <product>
```

Example:

```text
search apache
```

This helps discover the available functionality.

### Service-Oriented Search

If the service is known:

```text
search <service>
```

Example:

```text
search smb
```

### Vulnerability-Oriented Search

If a vulnerability identifier or strong vulnerability information is known, search for it.

For example:

```text
search <identifier>
```

The exact identifier depends on the vulnerability being investigated.

### Module-Type Search

You can narrow by module type.

For example:

```text
search type:auxiliary
```

This is useful when you already know the type of functionality you need.

The important point is:

```text
Search syntax is a tool.
Evidence is the strategy.
```

## Search With a Purpose

Before searching, complete this sentence:

```text
I am searching because I need to ______.
```

Good examples:

```text
I need a scanner for a discovered service.
```

```text
I need a module related to a known vulnerability.
```

```text
I need functionality for validating a suspected condition.
```

Weak example:

```text
I want to see what exploits exist.
```

The second approach encourages random experimentation.

## Search Results Are Candidates

Suppose:

```text
search apache
```

returns several modules.

Do not immediately select the first result.

Create a candidate set:

```text
Candidate A
Candidate B
Candidate C
Candidate D
```

Then compare them against your evidence.

Ask:

```text
Does it target the right product?

Does it target the right version?

Does it target the right protocol?

Does it require a specific condition?

Does it support the target platform?

Does it match my objective?

Does it perform the action I actually need?
```

## Search Result Interpretation

A search result may contain information such as:

```text
Type
Name
Disclosure date
Rank
Description
```

Do not treat the rank alone as your decision mechanism.

A high-ranked module can still be wrong for your target.

A lower-ranked module can still be relevant if its requirements match the evidence.

The fundamental question is:

```text
Does this module fit the target and objective?
```

## Search → Inspect → Reject

A professional workflow includes rejection.

For each candidate:

```text
SEARCH
  ↓
INSPECT
  ↓
REJECT or KEEP
```

Do not feel compelled to use every candidate you discover.

For example:

```text
Candidate A
Wrong version
→ Reject

Candidate B
Correct product but requires unavailable condition
→ Reject

Candidate C
Correct product, version, and objective
→ Keep
```

This is much more useful than:

```text
First result → run
```

## Reading Module Information

After selecting a candidate:

```text
use <module>
```

Then:

```text
info
```

Read the module as if you were reviewing a tool before using it during an engagement.

Look for:

* Description
* Vulnerability or feature being targeted
* References
* Targets
* Requirements
* Options
* Payload information where relevant
* Compatibility information
* Notes and warnings

## Module Description

Start with the description.

Ask:

```text
What exactly does this module attempt to do?
```

Do not rely only on the module's filename.

A module name may appear relevant while its actual behavior or requirements do not match your objective.

The description provides context.

## References

References can help connect the module to external technical evidence.

Depending on the module, references may include:

* Vulnerability identifiers
* Vendor advisories
* Security research
* Relevant documentation

Use references to answer:

```text
Why does this module exist?
What condition is it targeting?
What technical evidence should I compare against my target?
```

A reference is not proof that your target is vulnerable.

It is evidence about the vulnerability or technique the module addresses.

## Targets

Some exploit modules support multiple target configurations.

When relevant, inspect:

```text
show targets
```

Then determine:

```text
Which target configuration corresponds to my target?
```

Do not select a target because its number looks familiar.

Understand what the target entry represents.

The important distinction is:

```text
Metasploit target
≠
Network target
```

The network target is the system you are testing.

The Metasploit target entry describes how the exploit expects to interact with a particular target configuration.

## Options

Inspect:

```text
show options
```

Then classify the options.

A useful classification is:

```text
TARGET
PAYLOAD
CONNECTION
AUTHENTICATION
EXPLOIT-SPECIFIC
LOCAL/CALLBACK
OTHER
```

The exact categories depend on the module.

The purpose of classification is to understand what each value controls.

## Do Not Configure From Memory

Avoid habits such as:

```text
Every module needs RHOST.
Every reverse shell needs the same settings.
Every exploit uses the same target.
```

Metasploit modules differ.

Read the current module's options.

For example:

```text
show options
```

is more reliable than assuming:

```text
"I remember what this module needs."
```

## Required Options

Start with required options.

Example:

```text
Required
--------
RHOSTS     yes
RPORT      yes
```

You need to determine those values.

Then inspect optional settings.

Do not automatically change every option.

## Defaults

A default value is not automatically wrong.

Ask:

```text
Why does this default exist?

Does it fit my target?

Does it fit my lab?

Does it fit my objective?
```

If yes, leave it alone.

If no, change it deliberately.

This avoids unnecessary configuration.

## Configuration as a Hypothesis

Every configuration value represents an assumption.

For example:

```text
set RHOSTS 192.168.56.101
```

means:

```text
I believe 192.168.56.101 is the correct target.
```

Likewise:

```text
set RPORT 445
```

means:

```text
I believe the relevant service is reachable on TCP/445.
```

Thinking this way makes troubleshooting much easier.

When the module fails, you can ask:

```text
Which assumption was wrong?
```

## Configuration Workflow

Use:

```text
show options
      ↓
identify required values
      ↓
collect evidence
      ↓
set one value
      ↓
show options
      ↓
verify
      ↓
set next value
      ↓
show options
      ↓
verify
```

This may feel slower than entering everything at once.

It creates a much cleaner troubleshooting trail.

## Example Configuration

Suppose your authorized lab target is:

```text
192.168.56.101
```

You select a scanner module requiring a target.

You might configure:

```text
set RHOSTS 192.168.56.101
```

Then immediately verify:

```text
show options
```

Do not continue until the value is correct.

The example demonstrates the workflow, not a universal requirement that every module uses `RHOSTS`.

## RHOSTS vs RHOST

Metasploit modules may use different target-related option names.

For example:

```text
RHOST
```

and:

```text
RHOSTS
```

are not interchangeable concepts.

A module may expect:

* One target.
* Multiple targets.
* A range.
* A network.
* A file containing targets.

Therefore:

```text
Read the option description.
```

Do not assume the option name from another module.

## Local vs Remote Configuration

One of the most important distinctions in Metasploit is:

```text
REMOTE TARGET
```

versus:

```text
LOCAL OPERATOR SYSTEM
```

For a reverse connection, for example, you may need to configure a local callback address.

Conceptually:

```text
Target
  ↓
connects back
  ↓
operator-controlled listener
```

This means you must distinguish:

```text
Where is the target?
```

from:

```text
Where should the target connect back to?
```

Confusing these values is a common cause of failed sessions.

## Configuration Verification

After configuration, review everything.

Ask:

```text
Is the target correct?

Is the target reachable?

Is the service/port correct?

Are required options configured?

Are optional values appropriate?

Are inherited/global values affecting this module?

Is the payload compatible?

Is the callback address correct where applicable?
```

Then:

```text
show options
```

Again.

## Clearing Configuration

If a value is wrong:

```text
unset OPTION
```

Then set the correct value.

Do not allow stale configuration to remain simply because it was configured earlier.

This becomes especially important when testing multiple lab targets.

## Global Values and Hidden State

Global configuration can be convenient, but it creates hidden state.

Consider:

```text
Module A
setg RHOSTS target-A
```

Later:

```text
Module B
```

If the global value applies to Module B, you may unintentionally test the wrong target.

Therefore:

```text
Convenience
    ↓
can create hidden state
    ↓
hidden state
    ↓
can create operator error
```

Use global configuration carefully.

## Payload Discovery

For modules that use payloads, you may need to inspect available choices.

A module may support:

```text
show payloads
```

Do not select a payload simply because its name looks familiar.

Evaluate:

```text
Operating system
Architecture
Transport
Connection direction
Staged/stageless behavior
Target compatibility
Network reachability
Expected session type
```

Payload selection receives its own dedicated section later in this repository.

For now, the important rule is:

```text
Payload selection is part of module compatibility.
```

## Check Before Exploit

If the module supports checking:

```text
check
```

consider using it when appropriate.

The workflow becomes:

```text
evidence
   ↓
module
   ↓
configuration
   ↓
check
   ↓
interpret
   ↓
exploit if justified
```

But remember:

```text
check result
≠
guaranteed exploitation result
```

A positive check provides useful evidence.

A negative or inconclusive check does not necessarily prove the vulnerability is absent.

## Module Selection Decision Matrix

Use this conceptual matrix:

| Question                                 | If Yes   | If No              |
| ---------------------------------------- | -------- | ------------------ |
| Does the module match the product?       | Continue | Reject             |
| Does it match the version/configuration? | Continue | Investigate        |
| Does it match the objective?             | Continue | Reject             |
| Are prerequisites present?               | Continue | Investigate        |
| Is the target compatible?                | Continue | Reject             |
| Can required options be configured?      | Continue | Gather information |
| Is the action authorized?                | Continue | Stop               |
| Is execution justified?                  | Continue | Validate further   |

This is not a scoring system.

It is a decision filter.

## When Two Modules Both Look Relevant

Sometimes two or more modules appear applicable.

Do not choose based only on:

```text
first result
```

or:

```text
highest rank
```

Compare:

```text
Evidence match
Requirement match
Target compatibility
Objective match
Validation capability
Expected behavior
Risk
```

Then choose the module whose assumptions best match the evidence.

If uncertainty remains, gather more information before executing.

## When No Module Looks Relevant

This is an important outcome.

Do not force Metasploit to solve the problem.

Possible next actions:

```text
Gather more service information.
```

```text
Confirm the product and version.
```

```text
Review vulnerability references.
```

```text
Search outside Metasploit for technical information.
```

```text
Use another security tool for validation.
```

```text
Document that the current evidence is insufficient.
```

The absence of a suitable Metasploit module does not mean the target is secure.

It means:

```text
Metasploit currently does not provide the functionality you need,
or you do not yet have enough information to identify it.
```

## Practical Exercise 1 — Module Triage

### Objective

Learn to reject unsuitable modules.

### Scenario

Your lab provides:

```text
Target:
192.168.56.101

Service:
Known

Product:
Known

Version:
Known
```

### Task

Search Metasploit for relevant modules.

For at least three candidate modules, record:

```text
Module:
Why it appeared in search:
What it actually does:
Required conditions:
Why it fits or does not fit:
Decision:
KEEP / REJECT
```

Do not execute anything.

### Success Criteria

You should be able to explain why a module was rejected without saying:

```text
"It just looked wrong."
```

Your reasoning should be based on evidence.

## Practical Exercise 2 — Read Before Configure

### Objective

Practice extracting requirements from a module.

### Task

Select a suitable lab module.

Run:

```text
info
```

Then:

```text
show options
```

If relevant:

```text
show targets
```

and:

```text
show payloads
```

Create a small worksheet:

```text
Objective:
Target:
Module:
Why this module:
Required options:
Optional options:
Target assumptions:
Payload requirements:
Missing information:
Validation method:
```

Do not execute the module yet.

## Practical Exercise 3 — Configuration Review

### Objective

Learn to detect configuration mistakes before execution.

### Task

Configure the module for your authorized lab.

Then perform a deliberate review:

```text
Target:
Correct?

Port:
Correct?

Protocol:
Correct?

Required options:
Complete?

Local callback:
Correct, if applicable?

Payload:
Compatible?

Inherited settings:
Reviewed?
```

Then run:

```text
show options
```

Compare the actual configuration against your worksheet.

## Practical Exercise 4 — One Assumption at a Time

### Objective

Develop disciplined troubleshooting.

### Scenario

Your module fails.

Create an assumption list:

```text
A1: Target IP is correct.
A2: Service is reachable.
A3: Port is correct.
A4: Product/version matches.
A5: Module requirements are satisfied.
A6: Payload is compatible.
A7: Callback configuration is correct.
```

Do not change everything.

Choose one assumption.

Test it.

Record:

```text
Assumption:
Test:
Result:
Conclusion:
Next assumption:
```

This creates a repeatable troubleshooting process.

## Evidence Log

For practical work, maintain a simple record.

Example:

```text
Target:
192.168.56.101

Objective:
Validate suspected service vulnerability.

Evidence:
TCP/80 open
HTTP service identified
Product/version identified

Candidate module:
<module>

Reason selected:
Matches product/version and objective.

Required options:
<list>

Configuration:
<verified values>

Validation:
<check/output>

Execution:
<performed/not performed>

Result:
<result>

Next action:
<action>
```

The purpose is reproducibility.

Another operator should be able to understand what you did and why.

## Common Mistakes

### Mistake 1 — Choosing the First Search Result

Why it fails:

```text
Search relevance
≠
target applicability
```

Correction:

```text
Search → inspect → compare → select
```

### Mistake 2 — Running Before Reading

Why it fails:

You may misunderstand:

* Requirements
* Targets
* Options
* Payload compatibility
* Expected behavior

Correction:

```text
info
show options
```

before execution.

### Mistake 3 — Changing Every Option

Why it fails:

You introduce unnecessary variables.

Correction:

```text
Change only what the objective and evidence require.
```

### Mistake 4 — Treating Defaults as Automatically Correct

Why it fails:

A default is a convenience, not proof of compatibility.

Correction:

```text
Ask whether the default fits your target.
```

### Mistake 5 — Treating a Search Result as Vulnerability Proof

Why it fails:

A module can exist for a vulnerability without that vulnerability being present on your target.

Correction:

```text
Use target evidence and validation.
```

### Mistake 6 — Assuming Failure Means the Module Is Bad

Why it fails:

The problem may be:

* Target
* Network
* Service
* Version
* Configuration
* Payload
* Callback
* Module prerequisite

Correction:

```text
Diagnose before replacing the module.
```

### Mistake 7 — Repeating the Same Attempt

Why it fails:

Repeated identical attempts produce little new information.

Correction:

```text
Change one assumption.
```

## Professional Workflow

The complete operator pattern is:

```text
1. Define objective.
2. Collect evidence.
3. Search for functionality.
4. Build candidate set.
5. Read candidate modules.
6. Reject mismatches.
7. Select based on evidence.
8. Identify prerequisites.
9. Configure only necessary options.
10. Verify configuration.
11. Validate when appropriate.
12. Execute within authorization.
13. Interpret the result.
14. Record evidence.
15. Decide the next action.
```

If execution fails:

```text
1. Read the error.
2. Categorize the failure.
3. List assumptions.
4. Test one assumption.
5. Change one variable.
6. Retest.
7. Record the result.
8. Escalate or change approach if necessary.
```

## The Core Skill

The important skill is not:

```text
"I know 500 Metasploit commands."
```

It is:

```text
"I can take incomplete target evidence,
find the relevant Metasploit capability,
determine whether it actually applies,
configure it correctly,
validate my assumptions,
interpret the result,
and choose the next action."
```

That is the foundation for module mastery.

## Completion Checklist

Before leaving this section, you should be able to:

```text
[ ] Search using target evidence.
[ ] Distinguish search results from proof.
[ ] Compare multiple candidate modules.
[ ] Read module information.
[ ] Identify module prerequisites.
[ ] Understand required vs optional options.
[ ] Configure options deliberately.
[ ] Verify configuration before execution.
[ ] Recognize hidden/global configuration state.
[ ] Understand the purpose of targets and payloads.
[ ] Use check when appropriate.
[ ] Diagnose configuration failures.
[ ] Reject unsuitable modules.
[ ] Know when more enumeration is required.
[ ] Know when another tool may be more appropriate.
```

## Key Mental Model

Remember:

```text
SEARCH
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
EXECUTE
  ↓
INTERPRET
```

And never forget:

```text
A module existing
does not mean
the module applies.
```

The evidence determines the decision.

## Next Step

The next section begins module mastery:

```text
03-modules/01-module-system.md
```

There we will move beyond console operation and build a clear mental model of **how Metasploit's module system is organized and how the different module types fit into an engagement workflow**.
