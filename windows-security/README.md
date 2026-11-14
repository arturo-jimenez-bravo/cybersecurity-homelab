# Project 06: Windows Security

## Overview

This project focused on building and securing a Windows 11 Pro ARM64 virtual machine for cybersecurity and SOC training.

The project expanded the homelab beyond Linux and introduced Windows-specific concepts including user identities, Security Identifiers (SIDs), groups, privileges, integrity levels, processes, services, PowerShell, NTFS permissions, Windows Event Logs, Microsoft Defender, Windows Firewall, and basic security-event correlation.

A major part of the project also involved troubleshooting a missing virtual network adapter and recovering the Windows VM after a temporary system freeze.

## Project Status

**Windows 11 Pro ARM64 deployment:** Completed ✅  
**Windows system baseline:** Completed ✅  
**Identity and privilege fundamentals:** Completed ✅  
**PowerShell fundamentals:** Completed ✅  
**NTFS permission analysis:** Completed ✅  
**Windows Event Log analysis:** Completed ✅  
**Defender and firewall verification:** Completed ✅

## Objectives

- Deploy Windows 11 Pro ARM64 in UTM
- Establish a Windows system baseline
- Troubleshoot missing virtual hardware drivers
- Verify network configuration and connectivity
- Understand Windows users and Security Identifiers
- Inspect local group memberships
- Understand security tokens and integrity levels
- Inspect Windows privileges
- Identify running processes
- Inspect Windows services
- Learn foundational PowerShell commands
- Inspect NTFS Access Control Lists
- Navigate Windows Event Viewer
- Analyze Windows Security events
- Understand successful logon events
- Correlate related security events
- Verify Microsoft Defender protection status
- Verify Windows Firewall profiles
- Apply Windows telemetry to a guided SOC-style investigation

## Windows 11 Pro ARM64 Deployment

Windows 11 Pro ARM64 was deployed as a virtual machine using UTM on an Apple Silicon host.

The VM was configured with:

```text
Architecture: ARM64
Memory: 4 GB
Virtual disk: 64 GB
Operating system: Windows 11 Pro
```

The ARM64 architecture was selected to match the Apple Silicon virtualization environment.

The Windows installation included the normal setup process for:

- Language and keyboard configuration
- Windows edition selection
- Virtual disk installation
- Local account creation
- Initial Windows configuration

Sensitive account credentials and host identifiers are intentionally excluded from this public documentation.

## UTM Guest Tools and Driver Troubleshooting

After the initial Windows installation, Windows did not report a usable network adapter.

Initial evidence included:

```text
systeminfo
→ Network Card(s): N/A
```

and:

```text
ipconfig
→ No network adapter information
```

Instead of randomly changing networking settings, the problem was investigated systematically.

The troubleshooting process was:

```text
Observe missing network adapter
        ↓
Confirm missing configuration with ipconfig
        ↓
Form hypothesis:
virtual network driver may be missing
        ↓
Locate mounted UTM Guest Tools
        ↓
Install guest drivers
        ↓
Restart Windows
        ↓
Verify network configuration again
```

During the first restart after the Guest Tools installation, the Windows VM temporarily became unresponsive.

UTM itself eventually required a forced restart.

After UTM was reopened and the Windows VM was started again, Windows recovered successfully and returned to the desktop.

The networking configuration was then verified again.

This exercise reinforced the importance of:

```text
Observe
↓
Form a hypothesis
↓
Make one controlled change
↓
Recover safely if necessary
↓
Collect fresh evidence
↓
Verify the result
```

## Windows System Baseline

Windows system information was inspected using:

```cmd
whoami
hostname
systeminfo
```

The baseline confirmed:

```text
Operating system: Windows 11 Pro
Architecture: ARM64-based PC
Memory: Approximately 4 GB allocated to the VM
Environment: Virtual machine
```

Machine-specific identifiers are intentionally excluded from this public documentation.

## Windows Networking

After the virtual network driver was repaired, networking was inspected using:

```cmd
ipconfig
```

This confirmed the presence of an Ethernet adapter and valid:

- IPv4 configuration
- Subnet mask
- Default gateway

Connectivity was then tested progressively.

### Local gateway connectivity

```cmd
ping 
```

The gateway responded successfully.

### External IP connectivity

```cmd
ping 1.1.1.1
```

External reachability was verified successfully.

### DNS resolution

```cmd
nslookup example.com
```

The DNS query successfully returned address information.

This recreated the structured troubleshooting methodology previously practiced on Linux:

```text
Network adapter
      ↓
IP configuration
      ↓
Default gateway
      ↓
Local connectivity
      ↓
External connectivity
      ↓
DNS resolution
```

## Windows Identity

The active security identity was inspected using:

```cmd
whoami
```

Additional identity information was retrieved using:

```cmd
whoami /user
```

This introduced the Windows Security Identifier, or SID.

Conceptually:

```text
Account name
     ↓
Human-readable identity

SID
     ↓
Windows security identifier
```

The full SID is intentionally excluded from this repository.

## Groups and Security Tokens

Group membership was inspected using:

```cmd
whoami /groups
```

The output included Windows security groups such as:

```text
BUILTIN\Administrators
BUILTIN\Users
NT AUTHORITY\Authenticated Users
```

The command also exposed the process integrity level.

The elevated Command Prompt used during the lab reported:

```text
Mandatory Label\High Mandatory Level
```

This introduced the distinction between:

```text
User account
      ↓
Group memberships
      ↓
Access token
      ↓
Integrity level
      ↓
Effective security context
```

## Integrity Levels and Privilege Elevation

Windows integrity levels were introduced as part of the Windows security model.

The elevated administrative Command Prompt operated with High integrity.

This reinforced an important access-control concept:

```text
An administrator-capable account
is not the same thing as every process
automatically operating with elevated authority.
```

This concept provides a foundation for understanding User Account Control and least privilege.

## Windows Privileges

Privileges associated with the active access token were inspected using:

```cmd
whoami /priv
```

The output showed Windows privileges with states such as:

```text
Enabled
Disabled
```

Examples observed during the lab included privileges related to:

- System notifications
- Debugging
- Shutdown
- Restore operations
- Impersonation
- System environment operations

The goal was not to memorize every Windows privilege but to understand that access tokens can contain special system capabilities in addition to group membership.

## Process Investigation

Running Windows processes were inspected using:

```cmd
tasklist
```

The command exposed information including:

```text
Image Name
PID
Session
Memory Usage
```

This established a direct comparison with Linux process investigation:

```text
Linux              Windows
ps                 tasklist
PID                PID
Process            Process
```

The exercises reinforced the principle that an unfamiliar process name should not automatically be considered malicious.

Process context should be investigated before reaching a conclusion.

## Windows Services

Windows services were inspected using:

```cmd
sc query
```

A specific Windows security-related service was then queried directly.

This demonstrated the difference between:

```text
Running processes
        ↓
tasklist

Managed Windows services
        ↓
sc query
```

A targeted query was used to verify that the Windows Firewall-related service reported a running state.

This reinforced a useful investigation pattern:

```text
Question
   ↓
Targeted query
   ↓
Evidence
   ↓
Interpretation
```

## PowerShell Fundamentals

PowerShell was introduced as a Windows administration, automation, and investigation environment.

The PowerShell prompt was entered from Command Prompt using:

```cmd
powershell
```

Running processes were then retrieved with:

```powershell
Get-Process
```

A specific process was queried by name:

```powershell
Get-Process WindowsTerminal
```

PowerShell pipelines and property selection were introduced using:

```powershell
Get-Process WindowsTerminal | Select-Object Id, ProcessName, CPU
```

This demonstrated the workflow:

```text
Collect objects
     ↓
Pass through pipeline
     ↓
Select relevant properties
     ↓
Focus investigation
```

PowerShell's structured object model will be expanded in future security and automation projects.

## NTFS Permissions and ACLs

A dedicated practice directory was created for permission analysis.

Its Access Control List was inspected using:

```powershell
Get-Acl .\security-practice
```

The access entries were then examined using:

```powershell
(Get-Acl .\security-practice).Access
```

This introduced the Windows ACL model:

```text
Resource
   ↓
Security Descriptor
   ↓
Access Control List
   ↓
Access Control Entries
   ↓
Identity
   ↓
Allow / Deny
   ↓
Permissions
   ↓
Inheritance
```

The lab observed Access Control Entries for identities including:

```text
SYSTEM
Administrators
Local account
```

Visible entries included:

```text
FullControl
Allow
IsInherited: True
```

This established the distinction between the Linux permission model and Windows' more granular ACL-based access control.

## Event Viewer

Windows Event Viewer was opened to begin examining endpoint telemetry.

The primary log hierarchy introduced was:

```text
Event Viewer
     ↓
Windows Logs
     ↓
Application
Security
Setup
System
```

The Security log became the main focus because it records security-auditing events generated according to Windows audit configuration.

Each event contains information such as:

- Timestamp
- Source
- Event ID
- Task category
- Security identity
- Event-specific fields

This introduced Event Viewer as a historical evidence source rather than only examining the system's current state.

## Event ID 4624

A Windows Security event with:

```text
Event ID 4624
```

was examined.

The event represented a successful Windows logon session.

The event's fields were inspected rather than treating the Event ID alone as sufficient evidence.

The selected event included:

```text
Logon Type: 5
Account: SYSTEM
Account Domain: NT AUTHORITY
Elevated Token: Yes
```

Logon Type 5 identified the session as a service-related logon.

This demonstrated that a successful logon event should be interpreted using fields such as:

```text
Account
Logon Type
Logon ID
Authentication context
Process
Network information
```

rather than automatically treating every successful logon as suspicious.

## Process Correlation

The same Event 4624 was examined further.

Its process information showed a Windows system service-management process associated with the service logon.

The event did not list a remote source network address or source port.

This allowed the event to be interpreted using multiple pieces of evidence:

```text
Successful logon
       ↓
Service logon type
       ↓
SYSTEM identity
       ↓
Elevated context
       ↓
Local service-management process
       ↓
No listed remote network source
```

## Event ID 4672

Nearby events with:

```text
Event ID 4672
```

were also observed.

These events represented sensitive privileges being assigned to new logon sessions.

Rather than interpreting the event in isolation, it was considered alongside:

```text
4624
Successful logon
        ↓
SYSTEM account
        ↓
Service logon
        ↓
4672
Sensitive privileges assigned
```

This introduced basic Windows security-event correlation.

The available evidence was consistent with expected privileged Windows service activity and did not independently establish malicious behavior.

## Windows Event Analysis Methodology

The investigation established a basic workflow:

```text
Event ID
   ↓
Timestamp
   ↓
Account
   ↓
Logon Type
   ↓
Logon ID
   ↓
Privileges
   ↓
Process
   ↓
Network source
   ↓
Related events
   ↓
Context
   ↓
Supported conclusion
```

The project reinforced an important SOC principle:

```text
A security event
is evidence of activity,
not automatically evidence
of a security incident.
```

## Microsoft Defender Verification

Microsoft Defender status was queried through PowerShell.

The following properties were verified:

```text
AntivirusEnabled
RealTimeProtectionEnabled
AntispywareEnabled
```

All three reported:

```text
True
```

This provided evidence that core Defender protection features were enabled on the Windows VM.

## Windows Firewall Verification

The Windows Firewall service was first inspected through the Windows service manager.

Firewall profiles were then queried using PowerShell:

```powershell
Get-NetFirewallProfile | Select-Object Name, Enabled
```

The following profiles were inspected:

```text
Domain
Private
Public
```

All three reported:

```text
Enabled = True
```

This demonstrated the distinction between:

```text
Firewall service running
```

and:

```text
Firewall profiles enabled
```

Both service and configuration evidence were collected.

## Troubleshooting Lessons

Several troubleshooting lessons occurred throughout the project.

Examples included:

- Missing Windows virtual network adapter
- Guest driver installation
- Temporary VM unresponsiveness after driver changes
- Recovering the Windows VM after UTM required a forced restart
- Verifying networking after recovery
- Command syntax errors
- Confusing the number `1` and lowercase letter `l`
- PowerShell path mistakes
- Correcting property separators in PowerShell commands
- Rechecking evidence after making configuration changes

The repeated workflow was:

```text
Observe
↓
Read the error or unexpected result
↓
Collect supporting evidence
↓
Form a hypothesis
↓
Make one controlled change
↓
Recover safely if necessary
↓
Collect fresh evidence
↓
Verify the result
```

## SOC Relevance

The skills practiced in this project are foundational to Windows endpoint investigations.

A SOC or cybersecurity analyst may need to determine:

- Which Windows user is active
- Which SID represents an account
- Which groups the account belongs to
- Whether a process is elevated
- Which privileges exist in an access token
- Which processes are running
- Which services are active
- Who has access to a file or directory
- Whether permissions were inherited
- Which security events occurred
- Which account created a logon session
- What type of logon occurred
- Whether sensitive privileges were assigned
- Which process was associated with the event
- Whether a remote network source was involved
- Whether endpoint protection is enabled
- Whether firewall protection is enabled

The project demonstrated the importance of correlating identity, process, privilege, network, and event-log evidence before drawing conclusions.

## What I Learned

After completing this project, I can:

- Deploy a Windows 11 Pro ARM64 security lab
- Troubleshoot missing virtual hardware drivers
- Recover a Windows VM after a temporary failure
- Establish a Windows system baseline
- Inspect Windows network configuration
- Troubleshoot gateway, Internet, and DNS connectivity
- Identify the current Windows security identity
- Retrieve a user's SID
- Inspect Windows group memberships
- Recognize an elevated High-integrity process
- Inspect Windows privileges
- Enumerate running processes
- Inspect Windows services
- Use foundational PowerShell commands
- Use PowerShell pipelines
- Select specific object properties
- Inspect NTFS Access Control Lists
- Identify ACL inheritance
- Navigate Event Viewer
- Locate the Windows Security log
- Interpret a successful logon event
- Identify a Windows logon type
- Correlate logon activity with privileges and processes
- Recognize that event context matters more than an Event ID alone
- Verify Microsoft Defender status
- Verify Windows Firewall profile status
- Apply a structured investigation methodology to Windows telemetry

## Result

Project 06 established a practical foundation in Windows endpoint security and security-event analysis.

The project expanded the homelab beyond Linux and demonstrated how Windows identity, privileges, processes, services, permissions, networking, PowerShell, security logs, Defender, and firewall telemetry can be investigated together.

It also introduced the process of correlating multiple Windows security events to form a supported conclusion rather than treating individual events as isolated indicators of malicious activity.

## Next Project

**Project 07: SIEM & Log Analysis**

The next project will build on Windows Event Logs and other telemetry by introducing centralized security monitoring.

Future exercises will focus on:

- Centralized log ingestion
- Searching security telemetry
- Filtering events
- Building queries
- Correlating events across data sources
- Identifying suspicious patterns
- Alert triage
- Security monitoring workflows
- SOC-style log investigation
