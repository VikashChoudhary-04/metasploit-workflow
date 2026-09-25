# Metasploit Installation and First Run

This lesson gets Metasploit from **not installed** to a verified, usable environment.

The goal is not to memorize installation commands.

The goal is to understand:

```text
INSTALL
  ↓
VERIFY
  ↓
START
  ↓
CHECK ENVIRONMENT
  ↓
READY FOR WORK
```

## Before You Begin

Use a system or virtual machine dedicated to authorized security testing where possible.

Recommended environments include:

* Kali Linux.
* Another supported Linux distribution.
* Windows.
* macOS.

For practical learning, a Linux security-testing environment is generally the most convenient.

Only use Metasploit against systems you own or are explicitly authorized to test.

## What You Need

At minimum, you need:

* A supported operating system.
* Network access for installation and updates.
* Sufficient disk space.
* Administrative privileges when required by the installation method.
* A controlled lab target for practical exercises.

You do **not** need a vulnerable target just to verify that Metasploit itself is working.

## Installation Strategy

The exact installation process depends on the operating system and distribution.

The important workflow is:

```text
CHOOSE SUPPORTED INSTALLATION METHOD
        ↓
INSTALL METASPLOIT
        ↓
START METASPLOIT
        ↓
VERIFY VERSION
        ↓
VERIFY DATABASE
        ↓
RUN BASIC FUNCTIONAL CHECKS
```

Prefer current installation instructions from the official Metasploit documentation rather than copying commands from old tutorials.

Official documentation:

* https://docs.metasploit.com/
* https://docs.metasploit.com/docs/using-metasploit/getting-started/nightly-installers.html

## Kali Linux

If you are using Kali Linux, Metasploit is normally available through the distribution's package ecosystem.

First determine whether it is already installed:

```bash
msfconsole --version
```

If the command is available, you may not need to install Metasploit separately.

If it is not available, use the current Kali package-management process rather than an unrelated third-party installer.

The important point is:

> **Do not install a second copy just because a tutorial uses a different installation method.**

First determine what is already installed.

## Verify the Installation

After installation, verify that the executable is available:

```bash
msfconsole --version
```

You should receive version information rather than a command-not-found error.

You can also locate the executable:

```bash
which msfconsole
```

On systems where `which` is unavailable, use:

```bash
command -v msfconsole
```

### What This Proves

These checks establish:

```text
msfconsole exists
        ↓
The shell can locate it
        ↓
Metasploit can report its version
```

They do **not** prove that every Metasploit component or database feature is working.

That requires additional checks.

## Start msfconsole

Launch the console:

```bash
msfconsole
```

You should eventually reach a prompt similar to:

```text
msf >
```

The exact startup output and banner may differ between framework versions.

Do not worry about memorizing the banner.

The important thing is the interactive prompt:

```text
msf >
```

## First Orientation

At the prompt, start with:

```text
help
```

You are not trying to memorize the output.

You are learning that Metasploit provides built-in help.

This gives you an important operational principle:

> **When you forget a command, first ask Metasploit.**

You can also use command completion and history while working.

## Verify the Framework Version

Inside `msfconsole`, check the framework version:

```text
version
```

Depending on the framework version and environment, version information may also be available through the startup information or other built-in commands.

The purpose is to establish:

```text
"What version am I actually operating?"
```

This becomes important when troubleshooting differences between tutorials, systems, and module behavior.

## Check the Database

Metasploit can use a database to organize engagement information.

Inside `msfconsole`, check its status:

```text
db_status
```

A healthy environment should report an available database connection.

The exact wording can vary by framework version and installation.

### Why the Database Matters

The database can help organize information such as:

```text
HOSTS
SERVICES
VULNERABILITIES
CREDENTIALS
LOOT
SESSIONS
ROUTES
```

You do not need to understand all of these yet.

For now, learn this distinction:

```text
Metasploit Framework
        +
Database
        =
Framework + engagement organization
```

## If the Database Is Not Connected

Do not immediately reinstall Metasploit.

First determine what is actually wrong.

Use the information provided by:

```text
db_status
```

Then consider:

```text
Is the database service available?
        ↓
Is the required database configuration present?
        ↓
Is Metasploit configured to use it?
        ↓
Is this installation method expected to provide it automatically?
```

The exact repair process depends on the operating system and installation method.

Follow current official documentation for your environment.

The important troubleshooting lesson is:

> **Diagnose the missing component before reinstalling the entire framework.**

## Check Basic Module Availability

A working Metasploit installation should be able to search its module database.

For example:

```text
search type:auxiliary
```

You should receive module results.

Do not worry about understanding all of them.

The purpose of this exercise is to verify that:

```text
msfconsole
    ↓
module database
    ↓
search
    ↓
results
```

is functioning.

## Inspect a Module

Select a harmless module for inspection rather than execution.

For example, search for auxiliary modules:

```text
search type:auxiliary
```

Then choose an appropriate module from the results.

The exact module you choose is less important at this stage than learning the workflow:

```text
SEARCH
  ↓
SELECT
  ↓
INFO
```

Inside a module context, inspect its information:

```text
info
```

Then inspect its options:

```text
show options
```

Do **not** run the module merely because you selected it.

At this stage, we are practicing inspection.

## Understand the Difference Between Inspection and Execution

These are different activities:

```text
SEARCH
```

means:

> Find possible functionality.

```text
INFO
```

means:

> Understand the selected module.

```text
SHOW OPTIONS
```

means:

> Understand its configuration requirements.

```text
RUN
```

means:

> Actually execute the module.

A professional workflow does not jump directly from:

```text
SEARCH
```

to:

```text
RUN
```

## Leave the Module Context

If you are inside a module and want to return to the main Metasploit prompt:

```text
back
```

You should return to:

```text
msf >
```

This is one of the first navigation commands you should become comfortable with.

## Exit Metasploit

When finished:

```text
exit
```

or:

```text
quit
```

Use the command supported by your current environment.

The important distinction is:

```text
back
```

moves out of the current module context.

```text
exit
```

leaves Metasploit.

## First-Run Verification Checklist

Your initial environment should satisfy:

```text
[ ] Metasploit executable is available
[ ] msfconsole starts
[ ] Framework version can be identified
[ ] msfconsole help works
[ ] Database status can be checked
[ ] Module search works
[ ] A module can be inspected
[ ] Module options can be displayed
[ ] Module context can be exited
[ ] Metasploit can be closed cleanly
```

Do not move forward because you simply managed to launch `msfconsole`.

Verify the environment.

## Common Installation Problems

### `msfconsole: command not found`

Possible causes:

* Metasploit is not installed.
* The executable is not in the expected `PATH`.
* The installation did not complete.
* You are using a different environment than expected.

Start with:

```bash
command -v msfconsole
```

Then verify the installation method and environment.

Do not immediately download random installation scripts.

---

### Metasploit Starts but Database Is Unavailable

Possible causes include:

* Database service not running.
* Database configuration problem.
* Installation-specific setup issue.
* Permission problem.
* Incorrect environment.

Start with:

```text
db_status
```

Then diagnose the database layer separately.

Do not assume that the entire framework is broken.

---

### Search Produces Unexpected Results

Remember:

```text
SEARCH RESULTS
    ≠
CONFIRMED APPLICABILITY
```

A search result only tells you that Metasploit found a matching module.

You still need to:

```text
READ
  ↓
COMPARE
  ↓
VALIDATE
```

---

### A Tutorial Uses a Command That Does Not Work

Do not immediately conclude that Metasploit is broken.

Ask:

```text
Is the tutorial old?
        ↓
Is the command still supported?
        ↓
Did the command behavior change?
        ↓
Is the command distribution-specific?
        ↓
Is there a current documented equivalent?
```

Frameworks change.

Your goal is not to memorize historical commands.

Your goal is to understand the current workflow.

## Updating Metasploit

Metasploit should be maintained using the package or installation mechanism appropriate to your environment.

Before updating, understand:

```text
CURRENT VERSION
      ↓
AVAILABLE UPDATE
      ↓
UPDATE METHOD
      ↓
VERIFY AFTER UPDATE
```

After an update, verify:

```text
msfconsole --version
```

and perform a basic search.

This helps confirm that the framework still starts and can access its module collection.

## Why Version Awareness Matters

Suppose a tutorial says:

```text
"This command produces X."
```

but your installation behaves differently.

There are several possible explanations:

```text
Different framework version
Different operating system
Different package
Different module
Different configuration
Deprecated behavior
```

Therefore, record the environment when troubleshooting.

At minimum:

```text
Metasploit version
Operating system
Target environment
Module
Relevant configuration
Observed result
```

This makes problems reproducible.

## First Operational Exercise

Before continuing to the next lesson, complete this exercise.

### Objective

Verify that your Metasploit installation is operational without attacking a target.

### Starting Information

You have:

```text
A working Metasploit installation
```

### Task

Determine:

1. Whether `msfconsole` is available.
2. Which framework version you are running.
3. Whether the database is available.
4. Whether module search works.
5. Whether you can inspect a module.
6. Whether you can display module options.
7. Whether you can return to the main prompt.
8. Whether you can exit cleanly.

### Expected Workflow

You should independently determine something similar to:

```text
VERIFY INSTALLATION
        ↓
START CONSOLE
        ↓
CHECK VERSION
        ↓
CHECK DATABASE
        ↓
SEARCH
        ↓
INSPECT
        ↓
SHOW OPTIONS
        ↓
BACK
        ↓
EXIT
```

Do not treat this as a command-memorization exercise.

The goal is to understand the sequence.

## Verification

You are ready for the next lesson when you can answer:

### Environment

* How do I know Metasploit is installed?
* How do I start `msfconsole`?
* How do I determine the framework version?
* How do I check database status?

### Navigation

* How do I get help?
* How do I search for modules?
* How do I inspect a module?
* How do I display its options?
* How do I leave a module?
* How do I exit Metasploit?

### Troubleshooting

If `msfconsole` starts but the database is unavailable, do you:

```text
A. Immediately reinstall Metasploit
B. Ignore the problem
C. Diagnose the database layer
D. Delete the installation
```

Correct approach:

```text
C. Diagnose the database layer
```

The reason matters more than the answer.

## Mental Model

Your installation workflow should now be:

```text
INSTALL
  ↓
VERIFY
  ↓
START
  ↓
CHECK VERSION
  ↓
CHECK DATABASE
  ↓
SEARCH
  ↓
INSPECT
  ↓
READY
```

The next lesson moves from:

> **"Can I start Metasploit?"**

to:

> **"Can I operate the Metasploit console without hesitation?"**
