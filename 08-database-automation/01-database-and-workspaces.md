# Database and Workspaces

## Objective

Learn how to use Metasploit's database and workspace concepts to organize reconnaissance, services, credentials, vulnerabilities, sessions, and engagement data without mixing unrelated assessments.

By the end of this file, you should be able to:

* Understand why Metasploit uses a database.
* Understand what a workspace represents.
* Create and select an engagement-specific workspace.
* Keep assessment data separated.
* Understand hosts, services, credentials, vulnerabilities, and loot as distinct data types.
* Use database information to improve decision-making.
* Connect external reconnaissance with Metasploit.
* Recognize when database information is stale or incomplete.
* Avoid treating database records as proof of current target state.
* Build a repeatable engagement workflow.

## Why the Database Matters

Without structured data, an assessment can become:

```text id="m7x3qc"
Scan
  ↓
Output
  ↓
More output
  ↓
More output
  ↓
"What did I find?"
```

A database provides a structured place to retain useful engagement information.

The workflow becomes:

```text id="q4n8ws"
DISCOVER
  ↓
IMPORT / RECORD
  ↓
ORGANIZE
  ↓
QUERY
  ↓
VALIDATE
  ↓
ACT
  ↓
UPDATE
```

The database is therefore not just storage.

It can support better decisions.

## What the Metasploit Database Stores

Depending on the tools and workflow being used, Metasploit can track information such as:

```text id="v2k6rp"
Hosts
Services
Credentials
Vulnerabilities
Loot
Sessions
Routes
Other engagement-related information
```

Think of these as different categories of assessment state.

## The Database Mental Model

Use:

```text id="a8m4yx"
TARGET
  ↓
HOST
  ↓
SERVICES
  ↓
VULNERABILITIES
  ↓
CREDENTIALS / LOOT
  ↓
SESSIONS
  ↓
VALIDATED RESULTS
```

Not every target will contain every category.

The model simply helps organize what you know.

## Database Information Is Not Automatically Truth

This is one of the most important concepts.

A database record may represent:

```text id="n5q8vc"
What was discovered earlier.
```

It does not necessarily represent:

```text id="r3m7kx"
What is true right now.
```

For example:

```text id="j9w2pf"
Database:
Port 8080 open.
```

Later:

```text id="s4x6qn"
Service stopped.
```

The database may still contain the historical observation.

Therefore:

```text id="c7m1va"
DATABASE RECORD
≠
CURRENT TARGET STATE
```

Validate important findings before relying on them.

## What Is a Workspace?

A workspace is a way to logically separate engagement data within Metasploit's database-backed workflow.

Think:

```text id="p6v9ks"
ENGAGEMENT A
   ↓
WORKSPACE A

ENGAGEMENT B
   ↓
WORKSPACE B
```

This reduces the risk of mixing:

```text id="x2r7mc"
Targets
Services
Credentials
Vulnerabilities
Loot
```

from different assessments.

## Why Workspace Separation Matters

Imagine:

```text id="d8q4zn"
Client A:
10.10.10.0/24

Client B:
10.20.20.0/24
```

If both are mixed into one dataset:

```text id="m1v7xs"
Which host belongs to which engagement?
```

A workspace boundary provides a useful organizational layer.

The principle is:

```text id="w5k9qp"
ONE ENGAGEMENT
→
ONE LOGICAL DATASET
```

The exact structure may vary depending on the environment.

## Workspace Lifecycle

Use:

```text id="f3n8vc"
CREATE / SELECT
      ↓
IMPORT / DISCOVER
      ↓
QUERY
      ↓
VALIDATE
      ↓
UPDATE
      ↓
REPORT
      ↓
ARCHIVE / CLEAN UP
```

A workspace should follow the assessment lifecycle.

## Check the Database State First

Before relying on database functionality, determine whether the database is available.

Metasploit provides commands for inspecting database connectivity and status.

A commonly used command is:

```text id="u7m2qa"
db_status
```

The goal is to answer:

```text id="h4x8nc"
Is Metasploit connected to its database?
```

If the database is unavailable, do not assume database-backed commands will behave normally.

## Database Availability vs Metasploit Availability

These are different:

```text id="j6q3vr"
Metasploit starts
```

versus:

```text id="r9m4kp"
Database-backed functionality is available
```

A working `msfconsole` does not automatically mean the database is ready.

Check the actual state.

## Workspace Inspection

Metasploit provides workspace functionality for viewing and managing workspaces.

A commonly used command is:

```text id="k8v2my"
workspace
```

The exact output depends on the current environment.

The important question is:

```text id="p5x7qa"
Which workspace am I currently using?
```

## Creating a Workspace

For an authorized lab or engagement, create a workspace that clearly identifies the assessment.

For example:

```text id="c3n9wf"
workspace -a lab-web-01
```

The name should make the purpose obvious.

Avoid ambiguous names such as:

```text id="q7m4zs"
test
new
temp
final
```

Prefer names that communicate context:

```text id="v1k8pd"
lab-web-01
internal-assessment
api-lab
client-a-2026
```

Use engagement naming conventions appropriate to the environment.

## Switching Workspaces

When moving between engagements, explicitly select the intended workspace.

For example:

```text id="a6r3xm"
workspace lab-web-01
```

The exact syntax can vary by Metasploit version, so verify with the local help if necessary.

The important habit is:

```text id="y9q2kc"
BEFORE QUERYING
      ↓
CONFIRM WORKSPACE
```

## Never Assume the Current Workspace

A common operational mistake is:

```text id="m8x4vq"
Open Metasploit
  ↓
Assume correct workspace
  ↓
Run queries
```

Instead:

```text id="s2k7pn"
Open Metasploit
  ↓
Check database
  ↓
Check workspace
  ↓
Then work
```

This simple habit prevents data confusion.

## Hosts

Hosts represent systems discovered or recorded during the engagement.

Useful host information may include:

```text id="r4m8yc"
Address
Hostname
Operating system information
MAC information where available
Host notes
```

Hosts answer:

```text id="q1v7ks"
What systems do we know about?
```

## Services

Services describe network-accessible services associated with hosts.

Examples conceptually include:

```text id="j6p3mx"
HTTP
HTTPS
SSH
SMB
FTP
Database services
```

Services answer:

```text id="w8n4qa"
What is exposed by this host?
```

A useful relationship is:

```text id="t5x2vz"
HOST
  ↓
SERVICE
  ↓
POSSIBLE ATTACK SURFACE
```

But remember:

```text id="k9m3pw"
SERVICE PRESENT
≠
VULNERABLE
```

## Vulnerabilities

Vulnerability records represent identified or imported vulnerability information.

Treat them as hypotheses until appropriately validated.

For example:

```text id="e4r7nc"
Scanner:
Potential vulnerability.

Database:
Recorded vulnerability.

Tester:
Validate against current target state.
```

Use:

```text id="s7m1qx"
DISCOVERED
  ↓
RECORDED
  ↓
VALIDATED
```

not:

```text id="f2v8ka"
RECORDED
=
PROVEN
```

## Credentials

Credential information may be stored in the database when collected or imported through authorized workflows.

Treat credentials as highly sensitive assessment data.

Ask:

```text id="n6q3yr"
Is this credential information necessary?
Is it authorized?
How should it be protected?
Does it belong to this workspace?
```

Do not casually mix credential records between engagements.

## Loot

Loot can represent collected assessment artifacts.

Examples conceptually include:

```text id="p8m4xc"
Files
Captured configuration
Evidence artifacts
Other collected data
```

The same principle applies:

```text id="h2v7qs"
COLLECT
  ↓
CLASSIFY
  ↓
PROTECT
  ↓
USE FOR OBJECTIVE
  ↓
CLEAN UP / RETAIN ACCORDING TO REQUIREMENTS
```

## Sessions and Database Data

Sessions can be associated with the broader engagement context.

This allows you to reason about:

```text id="u3k8mw"
Host
  ↓
Service
  ↓
Vulnerability
  ↓
Exploit attempt
  ↓
Session
```

This creates a more complete assessment picture.

## The Engagement Graph

Think of the database as representing relationships:

```text id="z6m1cp"
HOST
 │
 ├── SERVICE
 │      │
 │      └── VULNERABILITY
 │
 ├── CREDENTIAL
 │
 ├── LOOT
 │
 └── SESSION
```

This is not necessarily the exact internal database schema.

It is a useful operator mental model.

## Importing External Reconnaissance

Metasploit does not need to be the first tool used in every assessment.

You may begin with another authorized tool such as:

```text id="r7p3xn"
Nmap
Nessus
OpenVAS
Other approved scanners
```

Relevant information can then be brought into the Metasploit workflow where appropriate.

The important sequence is:

```text id="c5m9va"
EXTERNAL DISCOVERY
      ↓
IMPORT
      ↓
ORGANIZE
      ↓
VALIDATE
      ↓
ACT
```

Importing data does not make it automatically correct.

## Example: Nmap to Metasploit

A common workflow in an authorized lab is:

```text id="k2w8qd"
Nmap
  ↓
Service discovery
  ↓
Relevant results
  ↓
Import into Metasploit
  ↓
Review hosts/services
  ↓
Identify candidate modules
  ↓
Validate
```

The important skill is tool integration.

Metasploit should not replace reconnaissance tools unnecessarily.

## Discovery vs Validation

A scanner may report:

```text id="m8x3rp"
Potential vulnerability.
```

Metasploit may provide:

```text id="q4v7ns"
A module capable of validating or exploiting that condition.
```

Your workflow becomes:

```text id="j5k9mc"
DISCOVERY
  ↓
HYPOTHESIS
  ↓
MODULE
  ↓
VALIDATION
  ↓
CONTROLLED EXPLOITATION
```

This is much stronger than treating scanner output as final truth.

## Database Queries Should Answer Questions

Do not query the database simply because commands exist.

Ask:

```text id="v3m8qx"
What do I need to know?
```

Examples:

```text id="p7k2wr"
Which hosts have HTTP services?
```

```text id="n5c9ya"
Which services were discovered on this host?
```

```text id="f4x8mq"
Which credentials are associated with this assessment?
```

```text id="s6v1kd"
Which vulnerabilities were recorded?
```

The question should determine the query.

## Hosts Query

A commonly used command is:

```text id="a9m4pc"
hosts
```

Use it to inspect known host records in the current workspace.

Do not interpret it as a live scan.

It answers:

```text id="r2x7vn"
What does the database currently know?
```

not:

```text id="q8k3mc"
What is currently online?
```

## Services Query

A commonly used command is:

```text id="w5n9yd"
services
```

Use it to inspect recorded service information.

Again:

```text id="m7c2qa"
DATABASE SERVICE RECORD
≠
LIVE SERVICE VERIFICATION
```

If a service matters to the current decision, validate it against the target.

## Vulnerability Query

Metasploit can also expose recorded vulnerability information through database-backed commands.

The workflow is:

```text id="u4p8mx"
RECORDED VULNERABILITY
      ↓
REVIEW SOURCE / CONTEXT
      ↓
CHECK CURRENT TARGET
      ↓
VALIDATE
```

Never skip the validation stage simply because the database contains a record.

## Workspace Discipline

At the start of an engagement:

```text id="j1x6vr"
[ ] Database available
[ ] Correct workspace selected
[ ] Scope known
[ ] Initial data imported or discovered
```

During the engagement:

```text id="c8m3qp"
[ ] Update relevant findings
[ ] Keep data associated with correct targets
[ ] Validate important records
[ ] Record sessions and evidence
```

At the end:

```text id="n7v2ka"
[ ] Final findings documented
[ ] Sensitive data handled correctly
[ ] Temporary artifacts addressed
[ ] Workspace retained/archived according to requirements
```

## Practical Exercise 1 — Database Verification

In an authorized lab:

1. Start Metasploit.
2. Check database status.
3. Determine the active workspace.
4. Record the result.

Document:

```text id="x5q8mc"
Database:
Connected / Not connected

Workspace:
...

Why does this matter?
...
```

### Success Criteria

You can determine whether database-backed functionality is available before relying on it.

## Practical Exercise 2 — Workspace Creation

Create a dedicated lab workspace.

For example:

```text id="r3m7vx"
workspace -a metasploit-lab
```

Then verify that it is active.

### Success Criteria

You can explain:

```text id="f6k2qp"
Why a dedicated workspace is safer than mixing data into an unrelated workspace.
```

## Practical Exercise 3 — Host and Service Inventory

Using an authorized lab:

1. Perform or import legitimate reconnaissance.
2. Review the hosts recorded in the workspace.
3. Review recorded services.
4. Select one host.
5. Identify its recorded attack surface.

Document:

```text id="v8n4cy"
Host:
...

Known services:
...

Source of information:
...

What requires validation?
...
```

### Success Criteria

You can distinguish:

```text id="q2x7mp"
recorded information
```

from:

```text id="s9k3va"
verified current information
```

## Practical Exercise 4 — Import and Validate

Use an authorized lab scan.

Workflow:

```text id="m5p8xd"
SCAN
  ↓
IMPORT
  ↓
REVIEW
  ↓
SELECT FINDING
  ↓
VALIDATE
```

Pick one recorded service or vulnerability.

Answer:

```text id="j7c4qn"
What did the imported data claim?
What evidence supports the claim?
What still needs validation?
```

### Success Criteria

You do not treat imported scanner output as unquestionable truth.

## Practical Exercise 5 — Workspace Separation

Create two lab workspaces representing two different environments.

For example:

```text id="a3v9mk"
lab-web
lab-internal
```

Place different lab data in each.

Switch between them and verify that your queries return the expected dataset.

### Success Criteria

You can demonstrate that:

```text id="w6q2px"
WORKSPACE A
```

and:

```text id="n8m4yr"
WORKSPACE B
```

remain logically separated.

## Practical Exercise 6 — Database as a Decision Aid

Scenario:

```text id="k1r7vc"
Database contains:

Host A
 ├── HTTP
 ├── SSH
 └── Potential vulnerability

Host B
 └── SMB
```

Objective:

```text id="p4x8mq"
Determine what information you should investigate next.
```

Do not immediately exploit.

Instead identify:

```text id="s5n2yd"
What is known?
What is uncertain?
What should be validated?
Which tool or module is appropriate?
```

## Common Mistakes

### Mistake 1 — Using One Workspace for Everything

Correction:

```text id="m8q3vf"
Separate engagements logically.
```

### Mistake 2 — Assuming Database Records Are Live

Correction:

```text id="c7x1pn"
Validate important information against current target state.
```

### Mistake 3 — Treating Imported Vulnerabilities as Proven

Correction:

```text id="r4m9ks"
Treat them as findings or hypotheses until validated.
```

### Mistake 4 — Forgetting the Active Workspace

Correction:

```text id="y6p2wa"
Check the workspace before running engagement queries.
```

### Mistake 5 — Mixing Sensitive Data

Correction:

```text id="q8v3mc"
Keep credentials and loot associated with the correct authorized engagement.
```

### Mistake 6 — Treating the Database as a Scanner

Correction:

```text id="f5n7xr"
The database records information; it does not automatically prove current reachability.
```

### Mistake 7 — Collecting Data Without a Question

Correction:

```text id="j2m8pk"
Start with the information you need.
```

## Professional Workflow

Use:

```text id="u9x4qn"
START
  ↓
CHECK DATABASE
  ↓
CHECK WORKSPACE
  ↓
CONFIRM SCOPE
  ↓
DISCOVER / IMPORT
  ↓
ORGANIZE
  ↓
QUERY
  ↓
VALIDATE
  ↓
ACT
  ↓
UPDATE
  ↓
DOCUMENT
```

The database becomes useful when it supports decisions.

## Database and External Tools

A mature Metasploit workflow can look like:

```text id="e3k7mw"
Nmap
  ↓
Host / Service Discovery
  ↓
Metasploit Database
  ↓
Workspace
  ↓
Module Selection
  ↓
Validation
  ↓
Exploitation
  ↓
Session
  ↓
Evidence
```

Other tools can participate as well.

For example:

```text id="p8m2vc"
Nessus / OpenVAS
      ↓
Vulnerability Discovery
      ↓
Metasploit
      ↓
Controlled Validation
```

The tools complement one another.

## Data Quality

Ask:

```text id="w4q9ks"
Where did this data come from?
When was it collected?
Is it still relevant?
Has it been validated?
```

A database with poor-quality data can create false confidence.

Therefore:

```text id="c6n1yr"
DATA QUALITY
=
SOURCE
+
CONTEXT
+
FRESHNESS
+
VALIDATION
```

## Know When to Stop

Stop database-driven investigation when:

```text id="m3x8qp"
The required information has been identified.
```

or:

```text id="v7k2nc"
The finding has been sufficiently validated.
```

or:

```text id="r5p9ma"
Additional queries will not change the current decision.
```

The database should reduce uncertainty.

It should not become another source of endless enumeration.

## Completion Checklist

Before moving to resource scripts and repeatability, confirm that you can:

```text id="q4m8vx"
[ ] Explain why Metasploit uses a database.
[ ] Check database connectivity.
[ ] Explain what a workspace represents.
[ ] Create a dedicated workspace.
[ ] Switch between workspaces.
[ ] Confirm the active workspace.
[ ] Understand host records.
[ ] Understand service records.
[ ] Understand vulnerability records.
[ ] Understand credential and loot records.
[ ] Distinguish database records from live state.
[ ] Import authorized reconnaissance data.
[ ] Use database information to guide decisions.
[ ] Validate important imported findings.
[ ] Keep engagements logically separated.
[ ] Handle sensitive data appropriately.
[ ] Recognize stale or incomplete data.
[ ] Know when database investigation is sufficient.
```

## Key Mental Model

Remember:

```text id="n6v2kp"
DATABASE
  ↓
ORGANIZE KNOWLEDGE
  ↓
REDUCE UNCERTAINTY
  ↓
MAKE BETTER DECISIONS
```

And:

```text id="x8m4qa"
RECORDED
≠
CURRENT
≠
VALIDATED
≠
EXPLOITED
```

The professional skill is not simply knowing database commands.

It is knowing:

```text id="j3r7mc"
WHAT DATA EXISTS
      ↓
WHERE IT BELONGS
      ↓
HOW TRUSTWORTHY IT IS
      ↓
WHAT MUST BE VALIDATED
      ↓
WHAT DECISION IT SUPPORTS
```

## Next Step

The next file is:

```text id="z5q1wn"
08-database-automation/02-resource-scripts-and-repeatability.md
```

There we will turn the workflow into **repeatable automation using resource scripts—without replacing operator judgment with blind automation**.
