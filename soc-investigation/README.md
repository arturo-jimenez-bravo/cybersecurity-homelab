# Project 08: SOC Investigation

## Overview

This project focused on Security Operations Center (SOC) alert triage and investigation.

The goal was to move beyond recognizing individual security events and begin thinking like a Tier-1 SOC analyst:

```text
Alert
  ↓
Validate
  ↓
Ask questions
  ↓
Collect evidence
  ↓
Build timeline
  ↓
Correlate activity
  ↓
Determine scope
  ↓
Assess severity
  ↓
Make disposition
  ↓
Document findings
```

Unlike earlier projects, this project progressively reduced step-by-step guidance and required more independent decisions about which evidence should be investigated next.

All investigation scenarios documented in this project used synthetic training data and fictional identities, devices, IP addresses, and activity.

## Project Status

**Alert triage:** Completed ✅  
**Authentication investigation:** Completed ✅  
**Identity analysis:** Completed ✅  
**Endpoint investigation:** Completed ✅  
**Process analysis:** Completed ✅  
**Network analysis:** Completed ✅  
**Timeline construction:** Completed ✅  
**Scope analysis:** Completed ✅  
**Disposition decisions:** Completed ✅  
**Tier-1 documentation:** Completed ✅

## Objectives

- Practice Tier-1 SOC alert triage
- Validate security detections before reaching conclusions
- Analyze authentication failures
- Correlate failed and successful authentication
- Investigate privileged sessions
- Analyze endpoint process activity
- Examine parent/child process relationships
- Pivot from endpoint activity into network telemetry
- Investigate suspicious files and delivery context
- Build incident timelines
- Compare activity against user and device baselines
- Validate business context
- Determine incident scope
- Distinguish exposure from confirmed execution
- Recognize false positives and configuration problems
- Reassess alert severity as evidence changes
- Make evidence-based disposition decisions
- Practice Tier-1 escalation
- Document findings without overstating conclusions

## Investigation Methodology

The project used the following investigation model:

```text
WHO?
 ↓
Which account or identity?

WHAT?
 ↓
What activity occurred?

WHEN?
 ↓
What is the timeline?

WHERE?
 ↓
Which device, source, or destination?

HOW?
 ↓
What authentication, process, or network activity occurred?

WHAT HAPPENED NEXT?
 ↓
What additional activity followed?

IS THIS NORMAL?
 ↓
Compare against historical and business context.

HOW FAR?
 ↓
Determine scope.

WHAT DOES THE EVIDENCE SUPPORT?
 ↓
Disposition.
```

A major principle throughout the project was:

```text
Alert
≠
Confirmed attack
```

Alerts indicate activity requiring investigation.

The analyst must determine what the evidence actually supports.

## SOC Investigation 01: Repeated Authentication Failures

The first synthetic scenario began with a medium-severity alert involving repeated authentication failures for a user account.

Initial evidence included:

- Multiple Event ID 4625 failures
- One account
- One workstation
- One external source
- A short timeframe

The first investigative decision was to determine whether the failures were followed by a successful authentication.

Additional evidence showed:

```text
Repeated 4625 failures
        ↓
4624 successful logon
        ↓
Same account
        ↓
Same source
```

This increased the significance of the authentication pattern.

## Privileged Session Correlation

The successful authentication was followed by:

```text
Event ID 4672
```

The successful Event 4624 and Event 4672 shared the same Logon ID.

Conceptually:

```text
4624
Successful authentication
       │
       │ Shared Logon ID
       ▼
4672
Sensitive privileges assigned
```

The shared identifier provided stronger evidence that the privileged activity belonged to the same logon session.

## Endpoint Investigation

After authentication and privilege activity were correlated, the investigation pivoted to endpoint telemetry.

The process chain included:

```text
explorer.exe
     ↓
powershell.exe
     ↓
cmd.exe
```

PowerShell alone was not treated as malicious.

Instead, the investigation focused on:

- Account context
- Device
- Timestamp
- Parent process
- Child processes
- Command context
- Network behavior
- Historical usage

This reinforced:

```text
Process name alone
is not a verdict.
```

## Network Pivot

Network telemetry showed an outbound connection associated with PowerShell.

The investigation considered:

- Destination
- Destination port
- Protocol
- Initiating process
- User context
- Whether the destination had previously been observed
- Whether the activity fit expected behavior

Port 443 and TLS were treated as context rather than proof of benign or malicious activity.

## Scope Analysis

The investigation expanded beyond the original account and device.

The authentication source was associated with attempts involving additional accounts during the reviewed timeframe.

This changed the investigation from:

```text
One account
```

to:

```text
One source
     ↓
Multiple accounts
```

Scope analysis reinforced that the original alert may represent only one visible portion of broader activity.

## Severity Reassessment

The original alert severity was not treated as permanent.

As evidence accumulated, the investigation included:

- Repeated authentication failures
- Successful authentication afterward
- Privileged activity
- Endpoint process activity
- External network communication
- Multiple accounts associated with the source

The project reinforced:

```text
Initial alert severity
does not replace
analyst judgment.
```

## Escalation Decision

The accumulated evidence supported escalation for deeper investigation.

The Tier-1 analyst's responsibility was not to independently perform destructive response actions.

Instead:

```text
Triage
   ↓
Investigate
   ↓
Determine scope
   ↓
Assess severity
   ↓
Document
   ↓
Escalate
```

## Investigation 02: Service Account False Positive

The second synthetic scenario involved repeated authentication failures associated with an account that appeared to be used by an automated backup service.

Initial evidence looked concerning because many authentication failures occurred over a short interval.

Further investigation examined:

- Account purpose
- Source system
- Schedule
- Failure reason
- Successful authentication activity
- Privilege activity
- Process activity
- Network activity

The evidence indicated:

```text
Known service account
       ↓
Known backup system
       ↓
Expected scheduled timeframe
       ↓
Incorrect stored credential
       ↓
Repeated authentication failures
```

No suspicious successful authentication, privilege activity, process execution, or unusual network communication was identified in the reviewed evidence.

## False-Positive Reasoning

The service-account scenario demonstrated:

```text
High event volume
≠
Confirmed attack
```

The correct analyst response was to:

```text
Investigate
     ↓
Identify benign cause
     ↓
Document evidence
     ↓
Route configuration issue
for remediation
     ↓
Close according to SOC procedure
     ↓
Evaluate appropriate detection tuning
```

Detection tuning should not be used to hide unresolved operational problems.

## Investigation 03: Suspicious Process Execution

The third synthetic scenario began with:

```text
WINWORD.EXE
     ↓
powershell.exe
```

PowerShell was not automatically treated as malicious.

Instead, the investigation focused on:

- PowerShell command context
- Parent process
- User
- Device
- Process timeline
- Created files
- Network activity

Additional evidence produced a broader execution chain:

```text
Email attachment
      ↓
Document opened
      ↓
WINWORD.EXE
      ↓
PowerShell
      ↓
Temporary file created
      ↓
External network connection
```

The significance came from correlation, not from any individual executable.

## File and Origin Investigation

The document and subsequently created file were investigated.

Evidence considered included:

- File path
- File name
- Hash
- First-seen time
- Prevalence
- Related process activity
- Delivery method

File reputation that was unknown was not treated as proof that a file was malicious.

The investigation instead continued gathering supporting evidence.

## Email-to-Endpoint Correlation

Email telemetry established that the initial document had been delivered as an attachment.

This created the timeline:

```text
Email delivered
      ↓
Attachment received
      ↓
Document opened
      ↓
Word
      ↓
PowerShell
      ↓
File creation
      ↓
External connection
```

This demonstrated how different telemetry sources can contribute to one investigation.

## Exposure vs Execution

The attachment had been delivered to multiple recipients.

However, suspicious execution was only observed on one endpoint during the reviewed evidence.

The project distinguished:

```text
EXPOSURE
Multiple recipients received the file

        vs.

OBSERVED EXECUTION
Suspicious activity occurred
on one endpoint
```

This prevented the investigation from incorrectly labeling every recipient as compromised.

## Investigation 04: Browser Network Activity

Another synthetic alert involved:

```text
chrome.exe
   ↓
External IP
   ↓
TCP 443
   ↓
TLS
```

The investigation began by asking:

> What does the traffic actually look like?

Relevant evidence included:

- Destination
- Protocol
- Connection pattern
- Initiating process
- User
- Historical destination activity
- Nearby process activity
- Authentication anomalies
- Privilege anomalies

The reviewed evidence was consistent with expected browser activity.

The investigation demonstrated:

```text
External IP
+
TCP 443
+
TLS

does not automatically mean
suspicious activity.
```

## Investigation 05: Unusual Privileged Login

A synthetic high-severity scenario involved:

```text
4624
Successful logon
      ↓
4672
Sensitive privileges assigned
```

The login occurred outside the user's normal observed activity window.

Instead of immediately calling the activity malicious, the investigation asked:

- What was the user accessing?
- Was the device normal for the user?
- Was the time unusual?
- Was administrative access expected?
- What was the user's role?
- Was approved maintenance scheduled?
- What happened after login?

Business context established that the identity was an administrator using an expected administrative workstation during an approved maintenance window.

No additional suspicious endpoint or network behavior was identified.

The activity was therefore consistent with authorized administrative work.

## Behavioral Baselines

This scenario introduced a critical concept:

```text
Unusual
≠
Malicious
```

Behavior should be compared against:

- Historical login behavior
- Normal working hours
- Assigned device
- User role
- Expected privileges
- Approved maintenance/change activity
- Typical process behavior
- Typical network behavior

## Final Investigation: Potential Account Compromise

The final synthetic scenario began with:

```text
Repeated Event 4625 failures
        ↓
Event 4624 successful authentication
        ↓
External authentication source
```

The investigation increasingly became analyst-driven rather than multiple-choice.

Questions included:

- Why did the authentication attempts fail?
- How many failures occurred before success?
- Was the device normally used by the account?
- Was the source previously observed?
- Was remote work expected?
- What changed between failure and success?
- What happened immediately after authentication?
- Was PowerShell normal for the user's job?
- Why did PowerShell contact another destination?
- What process initiated the activity?
- Did the user intentionally perform it?

## Authentication Context

Evidence established:

```text
Legitimate employee account
        ↓
Expected assigned device
        ↓
Authorized remote work
        ↓
Repeated incorrect-password attempts
        ↓
Successful authentication
```

This provided plausible context for part of the authentication activity.

However, the investigation continued rather than stopping at a successful user confirmation.

## Post-Authentication Activity

After the successful session, telemetry showed:

```text
Successful authentication
        ↓
PowerShell
        ↓
External connection
        ↓
Command Prompt
```

PowerShell usage was not consistent with the reviewed normal activity for the user's job function.

The user also stated that the PowerShell and Command Prompt activity had not been intentionally performed.

This created an important discrepancy:

```text
Endpoint telemetry:
PowerShell executed under
the user's security context

        vs.

User statement:
The activity was not intentionally performed
```

The project reinforced that process execution under a user's security context does not automatically prove that the human user personally initiated the activity.

## Hypothesis Development

Instead of forcing a premature conclusion, multiple hypotheses were considered:

```text
User intentionally initiated activity
Application or automation initiated activity
Authorized administrative mechanism
Unauthorized activity
Other unidentified cause
```

The purpose of investigation is to collect evidence that supports or eliminates hypotheses.

## Final Assessment

The authentication activity had partial legitimate context:

- Valid employee
- Expected device
- Authorized remote work
- User confirmation of login

However, post-authentication endpoint activity remained unexplained:

- PowerShell execution
- External communication
- Command Prompt execution
- Activity inconsistent with reviewed job behavior
- User denied intentionally performing the shell activity

The strongest Tier-1 conclusion was:

> The remote authentication has legitimate supporting context, but the post-authentication endpoint activity remains unexplained and inconsistent with the user's reported actions and normal job function.

## Final Disposition

The synthetic case was escalated for deeper endpoint investigation and incident-response review.

Recommended follow-up included:

- Determine what initiated PowerShell
- Analyze the external destination
- Review related process activity
- Expand the endpoint timeline
- Search for similar behavior elsewhere
- Determine whether additional identities or devices were affected
- Preserve investigation evidence
- Follow authorized incident-response procedures

## Evidence vs Assumptions

One of the most important lessons from Project 08 was:

```text
Observed evidence
        ≠
Unsupported conclusion
```

Examples:

Good:

```text
PowerShell executed under the user's
security context.
```

Not supported without more evidence:

```text
The user personally typed the command.
```

Good:

```text
Multiple failed logons were followed
by a successful authentication.
```

Not supported without more evidence:

```text
An attacker definitely obtained the password.
```

Analyst conclusions should remain proportional to the available evidence.

## Tier-1 SOC Documentation

Investigations used a structure such as:

```text
Alert
Evidence
Timeline
Scope
Analysis
Disposition
Recommended Action
```

A strong escalation should allow another analyst to understand:

- Why the original alert fired
- What was investigated
- What evidence was found
- What evidence remains unresolved
- Which assets and identities may be affected
- Why the case was closed or escalated
- What should happen next

## SOC Investigation Workflow

The completed Project 08 methodology was:

```text
ALERT
  ↓
VALIDATE
  ↓
ASK QUESTIONS
  ↓
IDENTITY
  ↓
AUTHENTICATION
  ↓
DEVICE
  ↓
PROCESSES
  ↓
FILES
  ↓
NETWORK
  ↓
TIMELINE
  ↓
BASELINE
  ↓
BUSINESS CONTEXT
  ↓
SCOPE
  ↓
SEVERITY
  ↓
DISPOSITION
  ↓
DOCUMENT
  ↓
CLOSE OR ESCALATE
```

## What I Learned

After completing this project, I can:

- Perform initial SOC alert triage
- Avoid treating alerts as confirmed incidents
- Investigate failed authentication patterns
- Correlate failed and successful authentication
- Correlate privileged sessions using shared identifiers
- Analyze identity context
- Determine whether a device is expected for a user
- Use behavioral baselines
- Validate business context
- Investigate process trees
- Evaluate parent and child processes
- Investigate PowerShell activity contextually
- Pivot from endpoint telemetry to network evidence
- Analyze external connections without relying only on ports
- Investigate file origin and prevalence concepts
- Correlate email delivery with endpoint activity
- Build investigation timelines
- Distinguish exposure from observed execution
- Determine investigation scope
- Compare benign and suspicious explanations
- Recognize service-account false positives
- Understand when detection tuning may be appropriate
- Reassess severity as evidence changes
- Form multiple hypotheses
- Eliminate hypotheses using evidence
- Distinguish security context from proof of human action
- Make Tier-1 disposition decisions
- Escalate suspicious activity appropriately
- Write defensible SOC investigation notes
- Separate evidence from assumptions

## Result

Project 08 shifted the homelab from guided log analysis toward practical SOC investigation.

The project required progressively more independent decisions about what evidence should be examined next.

Rather than memorizing Event IDs or process names, the investigation process focused on asking:

```text
What happened?

Why did it happen?

What changed?

What happened afterward?

Is this normal?

How far does it extend?

What does the evidence actually prove?
```

This project established the investigative mindset required before advancing into incident response.

## Next Project

**Project 09: Incident Response**

The next project will shift from:

```text
What happened?
```

toward:

```text
What should the security team do about it?
```

Future exercises will introduce:

- Incident-response lifecycle
- Containment decisions
- Evidence preservation
- Account/session containment
- Endpoint isolation concepts
- Eradication
- Recovery
- Validation
- Post-incident review
- Lessons learned
- Incident documentation
