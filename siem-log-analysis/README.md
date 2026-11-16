# Project 07: SIEM & Log Analysis

## Overview

This project focused on Security Information and Event Management (SIEM), Windows Security log analysis, authentication telemetry, event correlation, Kusto Query Language (KQL) fundamentals, detection logic, alerts, incidents, and SOC investigation methodology.

The project built directly on the Windows Security skills developed in Project 06.

Instead of manually scrolling through thousands of Windows events, the goal was to learn how security telemetry can be queried, filtered, summarized, correlated, and transformed into useful investigative evidence.

## Project Status

**Windows Security log querying:** Completed ✅  
**Authentication analysis:** Completed ✅  
**Event correlation:** Completed ✅  
**KQL fundamentals:** Completed ✅  
**SIEM architecture:** Completed ✅  
**Detection logic:** Completed ✅  
**Guided SIEM investigation:** Completed ✅  
**SOC documentation:** Completed ✅

## Objectives

- Query Windows Security logs using PowerShell
- Filter events by Event ID
- Query multiple Event IDs simultaneously
- Filter events by time range
- Select relevant event fields
- Group and count security events
- Investigate successful and failed authentication
- Analyze Windows authentication failures
- Correlate related security events
- Understand SIEM architecture
- Understand data connectors and ingestion
- Understand SIEM tables and schemas
- Learn foundational KQL syntax
- Understand detection thresholds
- Understand analytics and detection rules
- Distinguish events, alerts, and incidents
- Understand basic detection tuning
- Practice SOC-style alert triage
- Make evidence-based dispositions
- Document investigations professionally

## Query-Driven Log Analysis

Windows Security events were queried using PowerShell rather than relying only on the graphical Event Viewer interface.

The initial query used:

```powershell
Get-WinEvent -LogName Security -MaxEvents 10
```

This introduced important event properties such as:

```text
TimeCreated
Id
LevelDisplayName
Message
```

The exercise demonstrated the transition from:

```text
Scroll through thousands of events
            ↓
Find something manually
```

to:

```text
Ask a security question
        ↓
Construct a query
        ↓
Return relevant evidence
```

## Filtering by Event ID

PowerShell's `FilterHashtable` functionality was used to target specific Windows Security Event IDs.

Example:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624} -MaxEvents 5
```

This filtered the Security log for successful-logon events.

The investigation demonstrated that an analyst should retrieve only telemetry relevant to the current question whenever possible.

## Selecting Relevant Fields

Large event records were reduced to useful fields using PowerShell pipelines.

Example:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624} -MaxEvents 5 |
Select-Object TimeCreated, Id, RecordId
```

This introduced the workflow:

```text
Collect
   ↓
Filter
   ↓
Select
   ↓
Investigate
```

Rather than examining every property of every event, the analyst can focus on the fields relevant to the investigation.

## Multiple Event IDs

Multiple Windows Security Event IDs were queried simultaneously.

Example:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624,4672} -MaxEvents 10 |
Select-Object TimeCreated, Id, RecordId
```

This placed successful-logon and privileged-logon events into the same timeline.

The resulting records frequently appeared close together in time.

However, the project reinforced that:

```text
Similar timestamp
does not automatically prove
two events are related.
```

Additional fields must be correlated.

## Time-Window Analysis

A PowerShell variable was created to represent a specific investigation window.

Example:

```powershell
$start = (Get-Date).AddHours(-1)
```

The variable was then incorporated into log queries:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName='Security'
    Id=4624,4672
    StartTime=$start
} |
Select-Object TimeCreated, Id, RecordId
```

This introduced an essential investigation concept:

```text
All events
     ↓
Relevant Event IDs
     ↓
Relevant timeframe
     ↓
Smaller investigative dataset
```

Time-window filtering is especially important when investigating alerts tied to a known period of activity.

## Grouping and Counting Events

PowerShell was used to summarize authentication telemetry.

Example:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName='Security'
    Id=4624,4672
    StartTime=$start
} |
Group-Object Id |
Select-Object Name, Count
```

This transformed individual records into a simple summary grouped by Event ID.

The exercise reinforced the difference between:

```text
Raw events
```

and:

```text
Event patterns
```

An analyst may begin with aggregated behavior and then drill into the underlying records.

## Event ID 4624

Windows Security Event ID:

```text
4624
```

was used to investigate successful logon sessions.

Individual 4624 records were examined for evidence including:

- Account name
- Logon type
- Process information
- Source network information
- Logon ID

The project reinforced that Event ID 4624 alone does not mean that a suspicious login occurred.

The event must be interpreted in context.

## Event ID 4672

Windows Security Event ID:

```text
4672
```

was used to identify logon sessions that received sensitive privileges.

The project examined 4672 events alongside 4624 events rather than analyzing them in isolation.

This introduced the idea that multiple security events can describe different parts of the same activity.

## Logon ID Correlation

One of the most important exercises involved correlating Event 4624 and Event 4672 using a shared Logon ID.

Conceptually:

```text
Event 4624
Successful logon
      │
      │ Shared Logon ID
      ▼
Event 4672
Sensitive privileges assigned
```

This was stronger evidence than simply observing that the events occurred at similar times.

The general principle was:

```text
Shared fields
     ↓
Stronger correlation
     ↓
Better investigative confidence
```

## Event ID 4625

Windows Security Event ID:

```text
4625
```

was used to investigate failed authentication.

A real failed-login event from the lab was examined.

Fields investigated included:

- Account name
- Failure reason
- Status
- Sub-status
- Logon type
- Caller process
- Source network address

The selected event involved an incorrect-password condition.

The exercise reinforced that:

```text
One failed login
does not automatically mean
a brute-force attack.
```

Security significance depends on pattern, frequency, source, account, context, and surrounding activity.

## Authentication Pattern Analysis

Failed authentication events were counted over a 24-hour period.

The query found only a limited number of matching failed-logon events within the selected timeframe.

Successful and failed authentication events were then compared using:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName='Security'
    Id=4624,4625
    StartTime=$day
} |
Group-Object Id |
Select-Object Name, Count
```

This demonstrated how analysts move from:

```text
Individual authentication event
```

to:

```text
Authentication pattern
```

before making a security conclusion.

## Event Message Extraction

Full Windows event messages were retrieved programmatically.

Example:

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624} -MaxEvents 1 |
Format-List TimeCreated, Id, Message
```

Specific investigation fields were then extracted from the event message.

This reinforced an important principle:

```text
Do not read everything
when the investigation only
requires specific evidence.
```

## SIEM Architecture

The project introduced the overall architecture of a Security Information and Event Management platform.

Conceptually:

```text
Windows ────────┐
Linux ──────────┤
Firewalls ──────┤
Identity ───────┼──► Data Ingestion
Cloud ──────────┤          ↓
DNS ────────────┤        SIEM
Endpoint Tools ─┘          ↓
                          Search
                            ↓
                         Detection
                            ↓
                           Alert
                            ↓
                         Incident
                            ↓
                       Investigation
```

A SIEM allows analysts to work across centralized telemetry rather than investigating each system independently.

## Data Connectors and Ingestion

Data connectors were introduced as the mechanism that allows telemetry from different security sources to enter the SIEM environment.

Conceptually:

```text
Data Source
    ↓
Connector / Ingestion
    ↓
SIEM
    ↓
Table
    ↓
Query
```

Potential sources include:

- Windows endpoints
- Linux systems
- Identity platforms
- Firewalls
- DNS
- Cloud services
- Endpoint-security products

## Tables and Schemas

SIEM telemetry is organized into tables.

Example table concepts included:

```text
SecurityEvent
SigninLogs
DeviceProcessEvents
DeviceNetworkEvents
SecurityAlert
SecurityIncident
```

A simplified table can be visualized as:

```text
SecurityEvent
│
├── TimeGenerated
├── Computer
├── Account
├── EventID
└── additional fields
```

The table determines which dataset is queried.

The schema determines which fields are available within that dataset.

## KQL Fundamentals

Kusto Query Language (KQL) was introduced as a query language used to analyze security telemetry.

The basic pipeline concept was:

```text
Get Data
   |
Filter
   |
Summarize
   |
Select
```

This connected naturally with previous Linux and PowerShell experience.

```text
Linux
command | filter

PowerShell
Get-Object | Select-Object

KQL
Table | operator | operator
```

## KQL `where`

The `where` operator was introduced for filtering records.

Example:

```kusto
SecurityEvent
| where EventID == 4625
```

Conceptually:

```text
SecurityEvent
     ↓
Keep only records
where EventID = 4625
```

Memory rule:

```text
where
→ Which rows?
```

## KQL `ago()`

Time filtering was introduced using:

```kusto
SecurityEvent
| where TimeGenerated > ago(1h)
```

Conceptually:

```text
ago(1h)
→ Look back one hour
```

This allowed queries to focus on a relevant investigative timeframe.

## KQL `project`

The `project` operator was introduced to choose output columns.

Example:

```kusto
SecurityEvent
| where EventID == 4625
| project TimeGenerated, Account, EventID
```

Memory rule:

```text
project
→ Which columns?
```

This was compared with PowerShell's:

```powershell
Select-Object
```

## KQL `summarize` and `count()`

KQL aggregation was introduced using:

```kusto
SecurityEvent
| where EventID == 4625
| summarize Failures=count() by Account
```

Conceptually:

```text
Failed logons
     ↓
Group by account
     ↓
Count each group's events
```

This answers:

> How many failed logons did each account generate?

## KQL Detection Thresholds

A threshold was added:

```kusto
SecurityEvent
| where TimeGenerated > ago(1h)
| where EventID == 4625
| summarize Failures=count() by Account
| where Failures >= 10
```

In plain English:

> Show accounts with at least 10 failed-logon events during the last hour.

This introduced the transition from:

```text
Log query
```

to:

```text
Detection logic
```

The threshold identifies behavior considered noteworthy enough to investigate.

It does not automatically prove malicious activity.

## Cross-Table Correlation

The KQL `join` concept was introduced for correlating evidence stored in different tables.

Conceptually:

```text
SigninLogs
      │
      │ Shared identity/device evidence
      ▼
DeviceProcessEvents
```

The investigation question might be:

> Did an account involved in a suspicious sign-in also execute an unusual process?

The important concept was:

```text
join
→ Connect evidence from multiple datasets
```

This is similar to the manual Logon-ID correlation performed between Event 4624 and Event 4672, but applied across different telemetry tables.

## Detection Rules

Detection rules were introduced as logic that continuously or periodically searches collected telemetry for behaviors considered potentially security-relevant.

Conceptually:

```text
Telemetry
   ↓
Detection logic
   ↓
Condition matched?
   ↓
Yes
   ↓
Alert
```

A detection might consider:

- Event type
- Time window
- Account
- Device
- Source
- Frequency
- Threshold

## Alerts

An alert indicates that security activity matched detection logic.

The project reinforced:

```text
Alert
≠
Confirmed attack
```

Instead:

```text
Alert
     ↓
Something deserves investigation
```

The analyst must validate the detection and investigate the surrounding context.

## Incidents

An incident was introduced as an investigation case containing related security evidence and alerts.

The conceptual flow became:

```text
Events
   ↓
Detection
   ↓
Alert
   ↓
Incident
   ↓
SOC investigation
```

Incidents allow analysts to work with related evidence rather than treating each alert independently.

## Detection Tuning

Detection tuning was introduced using the example of a rule that generated excessive benign alerts.

An overly sensitive rule may produce:

```text
Alert
Alert
Alert
Alert
Alert
...
```

without providing useful analyst value.

A useful detection should balance:

```text
Too sensitive
     ↓
Excessive noise

        ↕ Balance

Too insensitive
     ↓
Important activity missed
```

If a known application repeatedly triggers benign authentication failures, the appropriate response is not to disable monitoring entirely.

Instead:

```text
Investigate cause
      ↓
Verify benign pattern
      ↓
Document findings
      ↓
Evaluate detection tuning
      ↓
Preserve visibility into meaningful suspicious activity
```

## Guided SIEM Investigation

A simulated SIEM alert was analyzed.

The synthetic scenario included:

```text
Repeated failed authentication
        ↓
Successful logon
        ↓
Privileged session
        ↓
Unusual process
        ↓
Unusual network activity
```

The investigation followed:

```text
Alert
  ↓
Validate detection
  ↓
Account
  ↓
Timeframe
  ↓
Source
  ↓
Authentication events
  ↓
Privilege activity
  ↓
Endpoint activity
  ↓
Network activity
  ↓
Correlation
  ↓
Disposition
```

The accumulated evidence supported escalation for deeper investigation.

Importantly, the investigation did not prematurely declare a confirmed compromise.

## Benign vs Suspicious Dispositions

Two different outcomes were practiced.

### Suspicious pattern

```text
Repeated failures
      ↓
Successful login
      ↓
Privileged session
      ↓
Unusual process
      ↓
Unusual network activity
      ↓
Escalate
```

### Explainable benign pattern

```text
Repeated failures
      ↓
Known internal application
      ↓
Known configuration problem
      ↓
No suspicious successful login
      ↓
No unusual endpoint/network activity
      ↓
Document
      ↓
Close as benign/false positive
```

This reinforced the principle:

```text
Same alert
does not always mean
same disposition.
```

Context matters.

## SOC Investigation Documentation

The project introduced a simple Tier-1 documentation structure:

```text
1. Alert
2. Evidence
3. Analysis
4. Disposition
5. Recommended action
```

A strong analyst note separates observed facts from conclusions.

Example:

```text
Evidence:
Multiple failed authentication events were observed for the
account during the reviewed timeframe.

Analysis:
Available evidence indicates a pattern requiring additional
investigation.

Disposition:
Escalate.

Recommended Action:
Perform deeper identity, endpoint, and network investigation.
```

The project emphasized:

```text
Facts
≠
Assumptions
```

Analysts should avoid claiming compromise or attacker intent unless the evidence supports that conclusion.

## SIEM Investigation Workflow

The complete workflow developed during Project 07 was:

```text
TELEMETRY
     ↓
INGESTION
     ↓
TABLES / SCHEMAS
     ↓
KQL
     ↓
DETECTION
     ↓
ALERT
     ↓
INCIDENT
     ↓
TRIAGE
     ↓
INVESTIGATION
     ↓
CORRELATION
     ↓
DISPOSITION
     ↓
DOCUMENTATION
```

## Troubleshooting Lessons

Several syntax and query mistakes became useful learning exercises.

Examples included:

- Incorrect hash-table syntax
- Confusing `=` with other characters
- Incomplete text-search patterns
- Commands returning more information than expected
- Refining queries to return only relevant fields
- Correcting KQL operator misunderstandings
- Distinguishing row filtering from column selection

The repeated workflow remained:

```text
Run query
   ↓
Read error or unexpected result
   ↓
Inspect syntax
   ↓
Correct
   ↓
Run again
   ↓
Verify output
```

## SOC Relevance

The skills covered in this project directly support foundational SOC workflows.

A Tier-1 analyst may need to:

- Search security telemetry
- Filter large datasets
- Investigate authentication failures
- Compare successful and failed logons
- Identify privileged sessions
- Establish investigation timeframes
- Group and count security events
- Correlate related activity
- Work with multiple telemetry sources
- Understand SIEM tables and schemas
- Interpret detection rules
- Triage alerts
- Investigate incidents
- Distinguish benign activity from suspicious activity
- Escalate when evidence warrants deeper investigation
- Document conclusions clearly and defensibly

## What I Learned

After completing this project, I can:

- Query Windows Security logs with PowerShell
- Use `Get-WinEvent`
- Filter Windows events with `FilterHashtable`
- Filter by one or multiple Event IDs
- Define time-based investigation windows
- Use PowerShell variables during log investigations
- Select useful event properties
- Group and count event records
- Investigate Event 4624 successful logons
- Investigate Event 4625 failed logons
- Investigate Event 4672 privileged sessions
- Interpret authentication failure information
- Correlate events using Logon IDs
- Compare successful and failed authentication patterns
- Explain the purpose of a SIEM
- Explain data ingestion and connectors
- Understand SIEM tables and schemas
- Read basic KQL queries
- Use the concepts behind `where`, `project`, `ago()`, `summarize`, and `count()`
- Understand the purpose of KQL `join`
- Explain how detection thresholds work
- Distinguish events, detections, alerts, and incidents
- Understand basic detection tuning
- Follow a guided SIEM investigation
- Make evidence-based disposition decisions
- Write basic SOC investigation notes
- Separate evidence from unsupported assumptions

## Result

Project 07 established a practical foundation in SIEM and security-log analysis.

The project moved from individual Windows Security events toward structured querying, filtering, aggregation, event correlation, KQL concepts, detection logic, alert triage, incident investigation, and analyst documentation.

It also reinforced that effective security analysis depends on context and correlation rather than interpreting isolated events as proof of malicious activity.

This project prepares the homelab for more independent SOC alert investigation.

## Next Project

**Project 08: SOC Investigation**

The next project will shift the focus from guided SIEM concepts toward hands-on SOC investigation.

Future exercises will involve:

- Receiving security alerts
- Determining which evidence matters
- Choosing investigative steps
- Querying relevant telemetry
- Building timelines
- Correlating identities, endpoints, processes, and network activity
- Determining severity
- Making disposition decisions
- Escalating suspicious activity
- Writing SOC investigation notes
- Working with progressively less guidance
