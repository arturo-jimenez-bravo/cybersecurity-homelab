# Project 03: Linux Administration & Permissions

## Overview

This project focused on developing practical Linux administration skills using Kali Linux.

After establishing the system baseline in Project 02, this project moved from observing the operating system to actively working with files, directories, permissions, users, processes, services, logs, and package management.

The exercises were completed in a dedicated practice directory to avoid making unnecessary changes to important system files.

## Project Status

**Linux administration fundamentals:** Completed ✅  
**Permissions and access control:** Completed ✅  
**Process and service monitoring:** Completed ✅  
**Log investigation:** Completed ✅  
**Package management fundamentals:** Completed ✅

## Objectives

- Navigate the Linux filesystem from the terminal
- Create, copy, move, rename, and safely remove files and directories
- Read and search text files
- Understand shell output redirection
- Combine commands using pipes
- Identify users, groups, UIDs, and GIDs
- Understand Linux file permissions
- Modify permissions using `chmod`
- Understand normal versus elevated privileges
- Inspect running processes
- Monitor system resources
- Inspect system services
- Query and filter system logs
- Understand basic Kali package management
- Practice verification and troubleshooting after system changes

## Filesystem Navigation and Management

The project began inside a dedicated `linux-practice` directory.

Commands practiced included:

```bash
pwd
cd
ls
mkdir
touch
cp
mv
rm
```

These commands were used to navigate the filesystem and create a safe working environment.

A useful early lesson involved Linux case sensitivity. A directory named `Projects` could not be accessed using `projects`, demonstrating that capitalization matters when working with Linux paths.

I practiced a simple administration workflow throughout the project:

```text
Observe
↓
Make a change
↓
Verify the result
```

This reduced mistakes and encouraged deliberate system administration.

## Creating and Managing Files

A practice file called `notes.txt` was created using:

```bash
touch notes.txt
```

Text was written into the file and inspected from the terminal.

The following commands were practiced:

```bash
cat
cp
mv
rm
```

These exercises demonstrated how to:

- Create files
- Read file contents
- Copy files
- Rename files
- Move files between directories
- Remove files safely

Interactive deletion was practiced with:

```bash
rm -i
```

This reinforced the importance of verifying a target before removing it.

## Output Redirection

Shell output redirection was practiced using:

```text
>
>>
```

The single `>` operator was used to redirect output into a file, replacing existing content.

The `>>` operator was used to append new output without removing existing content.

This introduced an important concept that will later apply to scripts, logs, automation, and command output.

## Text Inspection and Searching

Several tools were used to inspect and search text:

```bash
cat
less
head
tail
grep
find
```

### `cat`

Displayed the complete contents of smaller text files.

### `less`

Provided an interactive way to inspect and search text without dumping the entire file into the terminal.

### `head`

Displayed lines from the beginning of a file.

### `tail`

Displayed lines from the end of a file.

### `grep`

Filtered text and returned lines matching a search pattern.

Case-sensitive and case-insensitive searches were practiced using:

```bash
grep
grep -i
```

### `find`

Located files and directories based on criteria such as filename.

An important distinction was established:

```text
find → locate files or directories

grep → search for information inside text
```

## Pipes

The pipe operator:

```text
|
```

was used to send the output of one command into another command.

For example:

```bash
ps aux | grep zsh
```

combined process information with text filtering.

This demonstrated how small Linux utilities can be combined into more useful investigation workflows.

## Users and Groups

Linux identity information was inspected using:

```bash
id
groups
```

This introduced:

- User IDs (UID)
- Group IDs (GID)
- Primary groups
- Supplementary groups

The exercises demonstrated that Linux access decisions involve both user identity and group membership.

## Linux Permissions

File permissions were inspected using:

```bash
ls -l
```

Permission strings were divided into:

```text
Owner | Group | Others
```

The primary permission types were:

```text
r = read
w = write
x = execute
- = permission not granted
```

For example:

```text
-rw-r--r--
```

represents a regular file where the owner can read and write while the group and others can only read.

## Changing Permissions with chmod

Permissions were modified using both symbolic and numeric notation.

Example symbolic change:

```bash
chmod g-w notes.txt
```

This removed group write permission.

Numeric notation was then practiced:

```bash
chmod 600 notes.txt
```

The numeric model was learned as:

```text
read    = 4
write   = 2
execute = 1
```

Therefore:

```text
6 = read + write
0 = no access
0 = no access
```

A mode of `600` gives the owner read/write access while granting no permissions to the group or others.

This demonstrated the principle of limiting access to what is actually required.

## Execute Permissions

A small shell script was created to demonstrate execute permissions.

Initially, the script did not have execute permission.

Execute permission was added for the owner using:

```bash
chmod u+x hello.sh
```

The script was then executed from the current directory using:

```bash
./hello.sh
```

This demonstrated the purpose of the `x` permission for executable files and scripts.

## sudo and Privilege Elevation

Administrative privileges were explored using:

```bash
sudo -l
```

Normal and elevated execution identities were compared using:

```bash
whoami
sudo whoami
```

This demonstrated the difference between operating as a standard user and running an authorized command with elevated privileges.

The exercise reinforced the principle of least privilege: elevated access should be used only when required.

## Process Investigation

Running processes were inspected using:

```bash
ps
ps aux
top
```

The project introduced the concept of a Process ID (PID), which identifies a running process.

`ps` provided process snapshots, while `top` provided a continuously updating view of processes and system resource usage.

Process information included:

- PID
- Process owner
- CPU usage
- Memory usage
- Command name

Process filtering was practiced using:

```bash
ps aux | grep zsh
```

This demonstrated how process information can be filtered instead of manually reviewing a large process list.

## Service Investigation

System services were inspected using:

```bash
systemctl status NetworkManager
```

This introduced service information such as:

- Loaded state
- Active state
- Main PID
- Tasks
- Memory consumption
- Recent service events

This connected Linux processes with the background services managed by the operating system.

## Log Investigation

System logs were examined using:

```bash
journalctl
```

The following concepts were practiced:

- Filtering by service
- Limiting the number of returned events
- Filtering by the current boot
- Filtering by severity
- Combining logs with `grep`

For example:

```bash
journalctl -u NetworkManager -n 10 --no-pager
```

was used to examine recent NetworkManager events.

Error-level events from the current boot were inspected using:

```bash
journalctl -b -p err -n 10 --no-pager
```

This surfaced repeated clipboard-related events generated by the VM integration agent.

Rather than assuming that an error represented malicious activity, the process generating the message was identified and the surrounding context was investigated.

This reinforced an important security-analysis principle:

```text
An error is evidence of an event, not automatically evidence of an attack.
```

## Package Management

Kali's configured software repository was inspected before performing package-management operations.

The package index was refreshed using:

```bash
sudo apt update
```

Available upgrades were inspected without installing them.

This demonstrated the difference between:

```text
Updating package information
```

and:

```text
Actually upgrading installed software
```

A full system upgrade was intentionally not performed during this project.

## Troubleshooting Lessons

Several mistakes during the exercises became useful troubleshooting opportunities.

Examples included:

- Incorrect capitalization in directory names
- Typographical errors in command paths
- Incorrect command options
- Accidental shell redirection
- Commands waiting for additional input
- Discovering unexpected files and investigating why they appeared
- Identifying the active shell before explaining shell-specific behavior
- Correcting command syntax after reading error messages

These experiences reinforced a consistent troubleshooting approach:

```text
Observe the unexpected result
↓
Read the error or output
↓
Inspect the current state
↓
Identify the cause
↓
Correct the issue
↓
Verify the result
```

## SOC Relevance

The skills practiced in this project form a foundation for Linux-based security investigations.

A security analyst may need to determine:

- Which user is operating on a system
- Which permissions a file has
- Which users or groups can access a resource
- Which processes are running
- Which user owns a process
- Which services are active
- Whether a service is behaving normally
- What events have been recorded in system logs
- Whether an error requires additional investigation
- What software and updates are available on a system

The project also introduced the process of narrowing large amounts of information using filtering tools instead of manually reviewing everything.

## What I Learned

After completing this project, I can:

- Navigate the Linux filesystem from the terminal
- Create and manage files and directories
- Read and search text efficiently
- Use shell redirection
- Combine commands with pipes
- Locate files using `find`
- Search text using `grep`
- Identify user and group information
- Interpret basic Linux permission strings
- Modify permissions using `chmod`
- Understand symbolic and numeric permission notation
- Make a script executable
- Understand normal versus elevated privileges
- Inspect running processes and PIDs
- Monitor processes and system resources
- Inspect service status
- Query and filter Linux system logs
- Investigate an error without immediately assuming malicious activity
- Refresh and inspect package information
- Troubleshoot common command-line mistakes by reading system feedback

## Result

Project 03 established a working foundation in Linux administration, permissions, process monitoring, service management, log investigation, and package management.

The project moved beyond basic system inspection and introduced deliberate system administration and investigative workflows that will support future networking and security operations labs.

## Next Project

**Project 04: Networking Fundamentals**

The next project will expand on the networking concepts introduced during the Kali system baseline and focus on understanding how systems communicate across networks.
