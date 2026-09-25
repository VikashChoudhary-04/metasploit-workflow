# MSFConsole Workflow

## Objective

Learn to operate `msfconsole` as an investigation and execution interface rather than as a collection of commands.

By the end of this module, you should be able to:

* Navigate the Metasploit console confidently.
* Discover commands and modules without memorizing everything.
* Move between console and module contexts.
* Inspect a module before using it.
* Configure module options deliberately.
* Validate configuration before execution.
* Run a module and interpret the result.
* Manage sessions and jobs at a basic operational level.
* Recover from common console mistakes.
* Know what information is still missing before taking the next action.

## Why This Matters Professionally

A penetration tester rarely remembers every Metasploit command or every module.

The important skill is being able to answer:

```text
What am I trying to accomplish?
        ↓
What functionality do I need?
        ↓
How do I find it?
        ↓
What does the selected module require?
        ↓
How do I configure it?
        ↓
How do I validate the configuration?
        ↓
What happened?
        ↓
What should I do next?
```

The console is the environment in which this reasoning happens.

Do not treat `msfconsole` as:

```text
command → exploit → shell
```

Treat it as:

```text
investigate → select → inspect → configure → validate → execute → interpret
```

## The MSFConsole Mental Model

When you start Metasploit:

```bash
msfconsole
```

you enter an interactive console.

The prompt generally looks similar to:

```text
msf6 >
```

The prompt tells you that you are currently operating at the main Metasploit console level.

When you select a module, the context changes.

For example:

```text
msf6 > use auxiliary/scanner/...
msf6 auxiliary(...) >
```

The exact prompt depends on the selected module.

This context matters because some commands behave differently depending on where you are.

## The Core Console Workflow

Use this workflow throughout the repository:

```text
START
  ↓
UNDERSTAND OBJECTIVE
  ↓
DISCOVER COMMAND OR MODULE
  ↓
SELECT MODULE
  ↓
READ MODULE INFORMATION
  ↓
INSPECT OPTIONS
  ↓
CONFIGURE
  ↓
VALIDATE
  ↓
RUN
  ↓
INTERPRET RESULT
  ↓
NEXT ACTION
```

If something fails:

```text
FAILURE
   ↓
READ THE ERROR
   ↓
IDENTIFY THE ASSUMPTION THAT FAILED
   ↓
CHANGE ONE THING
   ↓
RETEST
```

Do not randomly change multiple settings.

## Getting Help

The first skill to develop is discovering functionality from inside the console.

Start with:

```text
help
```

This displays available command categories and commonly used commands.

You can also ask for help about a specific command:

```text
help search
```

or:

```text
help use
```

The exact output can vary between Metasploit versions.

### Practical Rule

Do not memorize every command.

Instead, remember:

```text
I can ask Metasploit.
```

If you forget how something works, investigate it.

## Command Discovery

Before running an unfamiliar command, determine what it does.

Useful discovery techniques include:

```text
help
```

and:

```text
help <command>
```

You can also use command completion where supported by the console.

For example, typing part of a command and using the terminal's completion mechanism can help discover available commands.

History is also useful when repeating or reviewing previous actions.

The objective is to reduce unnecessary memorization.

## Searching for Modules

One of the most important console skills is module discovery.

Use:

```text
search <term>
```

For example:

```text
search apache
```

or:

```text
search type:auxiliary
```

You can refine searches based on information you already know.

Possible search inputs include things such as:

* Product names
* Service names
* Vulnerability identifiers
* Module types
* Keywords
* Platform information

Do not search randomly.

Start with the strongest piece of evidence you have.

For example:

```text
Known:
Apache version
        ↓
Search:
Apache + version/vulnerability information
        ↓
Inspect:
Candidate modules
        ↓
Validate:
Target and module compatibility
```

## Search Is Discovery, Not Validation

A search result does not prove that a module will work.

For example:

```text
search apache
```

may return many modules.

That does not mean:

```text
Apache detected
=
Apache exploitable
=
This module will work
```

The search result only tells you:

```text
Metasploit contains functionality related to this keyword.
```

You still need to investigate.

## Selecting a Module

Once you have identified a potentially relevant module:

```text
use <module>
```

For example:

```text
use auxiliary/scanner/...
```

or:

```text
use exploit/...
```

Do not execute immediately after selecting a module.

The next step is inspection.

## Returning to the Previous Context

To leave the current module context:

```text
back
```

This returns you to the main console.

For example:

```text
msf6 exploit(...) >
back

msf6 >
```

This is useful when:

* You selected the wrong module.
* You finished investigating a module.
* You want to search for another module.
* You want to reset your thinking before trying another approach.

### Important Habit

Do not stay inside the wrong module simply because you already selected it.

Changing direction is normal.

## Inspecting a Module

After selecting a module, inspect it.

Use:

```text
info
```

This is one of the most important habits in Metasploit.

You want to understand:

* What the module is designed to do.
* What vulnerability or functionality it targets.
* Supported targets.
* References.
* Available options.
* Required settings.
* Payload information where applicable.
* Any important limitations.

Think of `info` as reading the operating instructions before using the tool.

## Inspecting Module Options

Use:

```text
show options
```

This displays the module's configurable options.

Typical information includes:

```text
Name
Current Setting
Required
Description
```

Your task is not to blindly fill every field.

Instead ask:

```text
Which options are required?
Which values do I actually know?
Which values must be discovered?
Which values depend on my lab setup?
```

## Required vs Optional

An option marked as required means the module needs a value before it can operate correctly.

Do not assume every visible option needs to be changed.

For example:

```text
Required: yes
Current Setting: blank
```

means:

```text
I need to determine this value.
```

Whereas:

```text
Required: no
Current Setting: some-default
```

may mean:

```text
The default may be appropriate unless my objective requires something else.
```

Always read the description before changing an option.

## Setting an Option

The general syntax is:

```text
set OPTION VALUE
```

For example:

```text
set RHOSTS 192.168.56.101
```

The exact options depend on the module.

After setting an option, inspect the configuration again:

```text
show options
```

This creates a simple verification loop:

```text
SET
 ↓
SHOW
 ↓
VERIFY
```

Do not assume that because you typed a value, your configuration is correct.

## Unsetting an Option

If you need to remove a configured value:

```text
unset OPTION
```

For example:

```text
unset RHOSTS
```

This is useful when:

* You selected the wrong target.
* You are switching between lab targets.
* You want to deliberately reconfigure a module.
* You want to avoid carrying an old value into another test.

## Global Configuration

Metasploit also supports global option configuration.

Global settings can be useful when the same value is needed across multiple modules.

However, global settings can also create hidden state.

This creates an operational risk:

```text
Previous configuration
        ↓
Global setting remains active
        ↓
New module inherits value
        ↓
Operator assumes configuration is fresh
        ↓
Unexpected behavior
```

Therefore:

> Use global configuration deliberately, and always inspect the effective configuration before execution.

When working through this repository, prioritize explicit module configuration until you understand inherited settings well.

## Showing More Than Basic Options

Depending on the module, additional information may be available through commands such as:

```text
show advanced
```

```text
show targets
```

and, where relevant:

```text
show payloads
```

These are not commands to run mechanically.

Use them when the module's behavior or your objective requires additional investigation.

For example:

```text
Architecture mismatch suspected
        ↓
Inspect supported targets/payloads
        ↓
Determine compatibility
```

## Validation Before Execution

Some modules support:

```text
check
```

A module may use this to determine whether the target appears vulnerable or otherwise suitable for the module.

When supported, a useful workflow is:

```text
use module
      ↓
info
      ↓
show options
      ↓
set required values
      ↓
show options
      ↓
check
      ↓
interpret result
      ↓
run/exploit if appropriate
```

However:

```text
check = safe proof of vulnerability
```

is an unsafe assumption.

A check may be:

* Inconclusive.
* Unsupported.
* Detection-dependent.
* Less reliable than expected.
* Different from actual exploitation behavior.

Treat the result as evidence, not absolute truth.

## Running a Module

Metasploit commonly uses:

```text
run
```

for modules that support normal execution.

Exploit modules can commonly be launched with:

```text
exploit
```

Some modules also support:

```text
run
```

depending on their implementation.

The important principle is:

```text
Do not choose the command based only on memory.
```

Inspect the module and use the execution method appropriate to that module.

## The Difference Between Configuration and Execution

These are separate stages.

Configuration:

```text
What will Metasploit do?
Against what target?
With what settings?
Using what payload, if applicable?
```

Execution:

```text
Actually perform the operation.
```

Do not mentally combine them.

A professional workflow deliberately pauses between them:

```text
CONFIGURED
    ↓
REVIEW
    ↓
AUTHORIZED?
    ↓
TARGET CORRECT?
    ↓
OPTIONS CORRECT?
    ↓
EXECUTE
```

## Sessions

Some successful operations result in a session.

You can inspect sessions with:

```text
sessions
```

This lets you determine whether Metasploit currently has active sessions.

To interact with a particular session:

```text
sessions -i <session_id>
```

For example:

```text
sessions -i 1
```

The exact session ID depends on the environment.

### Session Mental Model

Do not think:

```text
Exploit succeeded → finished
```

Think:

```text
Exploit succeeded
       ↓
Session created?
       ↓
Identify session
       ↓
Interact
       ↓
Verify access
       ↓
Perform authorized objective
```

A session is an operational state that must be managed deliberately.

## Backgrounding a Session

When interacting with a session, you may need to return to Metasploit without terminating the session.

Meterpreter commonly supports:

```text
background
```

The session remains available while you return to the Metasploit console.

You can then inspect available sessions again:

```text
sessions
```

This becomes important when multiple sessions exist.

## Multiple Sessions

In realistic engagements, more than one session may exist.

For example:

```text
Session 1 → Target A
Session 2 → Target B
Session 3 → Target C
```

Do not assume:

```text
latest session = correct session
```

Always identify which session corresponds to which target.

A useful habit is:

```text
sessions
      ↓
identify target
      ↓
interact
      ↓
verify context
```

## Jobs

Some Metasploit operations can run as background jobs.

You can inspect jobs with:

```text
jobs
```

Jobs are different from sessions.

A useful mental distinction is:

```text
Job
=
background operation

Session
=
interactive connection/state
```

Do not confuse:

```text
job exists
```

with:

```text
session exists
```

A background operation may or may not eventually produce a session.

## Backgrounding vs Stopping

There is an important difference between:

```text
background
```

and terminating an operation.

Backgrounding generally means:

```text
Keep the current session/operation available
but return control to the console.
```

Stopping or terminating means:

```text
End the relevant operation or state.
```

Always understand which one you intend before acting.

## A Complete Console Investigation

Suppose your lab contains a target with an identified service.

You should not immediately search for an exploit and run it.

Use a workflow such as:

```text
1. Identify the objective.
2. Record what is already known.
3. Search for relevant Metasploit functionality.
4. Select a candidate module.
5. Read info.
6. Inspect options.
7. Determine missing information.
8. Configure required values.
9. Review the configuration.
10. Use check if supported and appropriate.
11. Interpret the result.
12. Execute only when authorized and justified.
13. Determine whether a session or other result was produced.
14. Verify the result.
15. Decide the next objective.
```

This is the behavior the rest of this repository builds upon.

## Example: Investigating a Scanner Module

The following is a safe lab-oriented example of the reasoning process.

Start:

```text
msf6 > search type:auxiliary
```

Suppose you identify a scanner module relevant to your lab.

Select it:

```text
use auxiliary/...
```

Inspect it:

```text
info
```

Then:

```text
show options
```

Ask:

```text
What target information is required?
What port is relevant?
What protocol is expected?
Does the target actually expose the service?
```

Configure only what is necessary:

```text
set RHOSTS <LAB_TARGET>
```

Review:

```text
show options
```

Then execute if appropriate:

```text
run
```

Finally:

```text
Interpret the result.
```

Do not treat the command sequence as a recipe for every scanner.

The transferable skill is the reasoning pattern.

## Example: When a Module Does Not Work

Suppose execution produces an error.

Bad workflow:

```text
Try another payload
Try another port
Try another module
Change multiple settings
Run again
```

Better workflow:

```text
Read error
    ↓
Identify failure category
    ↓
Check target reachability
    ↓
Check required options
    ↓
Check module compatibility
    ↓
Check target/service assumptions
    ↓
Change one variable
    ↓
Retest
```

The error is evidence.

Use it.

## Common Failure Categories

### Wrong Target

Possible symptoms:

```text
Connection failed
Host unreachable
Timeout
No response
```

Questions:

```text
Is the target IP correct?
Is the target running?
Is the target reachable from my machine?
Is the relevant port accessible?
```

### Wrong Service Assumption

Possible symptoms:

```text
Connection succeeds
but module does not behave as expected
```

Questions:

```text
Is this actually the expected service?
Is the service version known?
Is the protocol correct?
Did enumeration identify the service correctly?
```

### Missing Required Option

Possible symptoms:

```text
Module refuses to run
or reports a required setting is missing
```

Questions:

```text
Did I inspect show options?
Did I configure every required value?
Did I accidentally unset something?
```

### Module Mismatch

Possible symptoms:

```text
Module runs
but target is unaffected
```

Questions:

```text
Does the module actually apply to this target?
Is the target version compatible?
Is the required vulnerability present?
Are there target-specific requirements?
```

### Payload or Session Failure

Possible symptoms:

```text
Exploit appears successful
but no session is created
```

Questions:

```text
Was the payload compatible?
Was the callback address reachable?
Was the callback port reachable?
Did the target execute the payload?
Did the session die immediately?
```

Do not jump straight to:

```text
"Metasploit is broken."
```

Investigate the assumptions first.

## Console State Awareness

One of the most common beginner mistakes is forgetting what state the console is currently in.

You may have:

```text
Global settings
        +
Module settings
        +
Selected module
        +
Running jobs
        +
Existing sessions
```

These states can affect your next action.

Before doing something important, ask:

```text
Where am I?
What module is selected?
What options are configured?
What sessions exist?
What jobs exist?
What assumptions am I carrying from the previous step?
```

## Resetting Your Thinking

If your console state becomes confusing, do not continue blindly.

A useful recovery pattern is:

```text
back
```

Then reassess.

You can search again:

```text
search <term>
```

Select the candidate:

```text
use <module>
```

Inspect again:

```text
info
show options
```

This may be faster than debugging a configuration you no longer understand.

## Practical Exercise 1 — Console Navigation

### Objective

Become comfortable moving between the main console and module contexts.

### Task

In an authorized lab:

```text
1. Start msfconsole.
2. Display help.
3. Search for an auxiliary module.
4. Select a relevant module.
5. Read its information.
6. Display its options.
7. Return to the main console.
8. Confirm that you can repeat the process.
```

### Success Criteria

You should be able to explain:

```text
What module did I select?
Why did I select it?
What does it do?
What options does it require?
What information am I still missing?
```

## Practical Exercise 2 — Configure and Verify

### Objective

Practice configuration without immediately executing anything.

### Task

Choose a suitable scanner module in your authorized lab.

Then:

```text
1. Select the module.
2. Run info.
3. Run show options.
4. Identify required options.
5. Configure the required target information.
6. Run show options again.
7. Verify every configured value.
```

### Rule

Do not execute the module until you can explain every value you configured.

### Success Criteria

You should be able to answer:

```text
What does each configured value control?
Why is this value correct?
Which values remain at defaults?
Why are those defaults acceptable?
```

## Practical Exercise 3 — Search to Selection

### Objective

Learn to move from evidence to module selection.

### Scenario

Your lab enumeration identifies a service and version.

Your task is:

```text
Known evidence
      ↓
Search Metasploit
      ↓
Identify candidate modules
      ↓
Inspect candidates
      ↓
Reject unsuitable candidates
      ↓
Select the most appropriate candidate
```

Do not execute the module.

### Success Criteria

You should be able to explain why the selected module fits the available evidence and why the other candidates were rejected.

## Practical Exercise 4 — Failure Diagnosis

### Objective

Practice troubleshooting instead of random experimentation.

### Scenario

A module fails to produce the expected result.

Your task:

```text
1. Read the output.
2. Identify the failure category.
3. List the assumptions involved.
4. Select one assumption to test.
5. Change one variable.
6. Retest.
7. Record the result.
```

### Rule

Never change five settings at once.

If you change multiple variables, you lose the ability to determine what actually fixed the problem.

## Operational Checklist

Before selecting a module:

```text
[ ] What is my objective?
[ ] What evidence do I have?
[ ] What functionality do I need?
[ ] What information is still missing?
```

After selecting a module:

```text
[ ] Did I read info?
[ ] Did I inspect the options?
[ ] Do I understand the module?
[ ] Are the target assumptions correct?
```

Before execution:

```text
[ ] Is this target authorized?
[ ] Is the target correct?
[ ] Are required options configured?
[ ] Did I verify the configuration?
[ ] Is the selected approach appropriate?
[ ] Can I explain what the module is expected to do?
```

After execution:

```text
[ ] What happened?
[ ] Did the expected result occur?
[ ] Was a session created?
[ ] Is the result verified?
[ ] What evidence should I record?
[ ] What is the next objective?
```

## Professional Habit: Read Before Run

A strong Metasploit operator does not measure progress by the number of commands executed.

A better measure is:

```text
quality of decisions
```

Compare:

```text
Beginner:

search
use
set
run
try again
try another module
try another payload
```

with:

```text
Operator:

objective
↓
evidence
↓
module selection
↓
module understanding
↓
requirements
↓
configuration
↓
validation
↓
execution
↓
verification
↓
next action
```

The second workflow is slower for the first few minutes and usually much faster when something unexpected happens.

## Know When to Stop

Metasploit is not automatically the correct tool for every task.

Stop and reconsider when:

```text
The module does not match the evidence.
```

or:

```text
The target behavior is not understood.
```

or:

```text
Required information is missing.
```

or:

```text
Another tool is better suited to discovery or validation.
```

or:

```text
Repeated attempts are producing no new information.
```

The correct response to repeated failure is not always another Metasploit command.

Sometimes the next action is:

```text
enumerate
```

or:

```text
verify manually
```

or:

```text
switch tools
```

or:

```text
stop
```

Knowing when not to use Metasploit is part of mastering Metasploit.

## What You Should Be Able to Do Now

Before moving to the next file, you should be comfortable with this complete loop:

```text
START
  ↓
UNDERSTAND OBJECTIVE
  ↓
SEARCH
  ↓
SELECT
  ↓
INFO
  ↓
SHOW OPTIONS
  ↓
CONFIGURE
  ↓
VERIFY
  ↓
CHECK WHEN APPROPRIATE
  ↓
RUN / EXPLOIT
  ↓
INTERPRET
  ↓
SESSION / RESULT
  ↓
VERIFY
  ↓
NEXT ACTION
```

And when something fails:

```text
FAILURE
  ↓
READ
  ↓
DIAGNOSE
  ↓
CHANGE ONE ASSUMPTION
  ↓
RETEST
```

This workflow is more important than memorizing individual Metasploit commands.

## Next Step

The next file is:

```text
02-interface/02-search-read-configure.md
```

It will go deeper into the **SEARCH → READ → CONFIGURE** cycle and teach you how to evaluate candidate modules instead of simply finding one and running it.
