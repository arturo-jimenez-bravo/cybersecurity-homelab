# Project 02: Kali Linux System Baseline

## Overview

This project establishes a technical baseline of my Kali Linux virtual machine before making additional system changes or beginning more advanced cybersecurity labs.

The goal was to learn how to inspect a Linux system from the command line, understand its basic resources and configuration, and verify that networking, routing, DNS, and external connectivity were functioning correctly.

## Project Status

**System baseline:** Completed ✅  
**Network baseline:** Completed ✅  
**DNS and connectivity testing:** Completed ✅

## Objectives

- Identify the current Linux user and hostname
- Navigate and inspect the Linux filesystem
- Identify hidden files and directories
- Begin understanding Linux ownership and permissions
- Verify the operating system and CPU architecture
- Inspect memory, swap, and filesystem usage
- Identify network interfaces and IP configuration
- Inspect the routing table and default gateway
- Inspect DNS resolver configuration
- Verify external network connectivity
- Verify DNS name resolution
- Practice troubleshooting without making unnecessary system changes

## Commands Learned

| Command | Purpose |
|---|---|
| `whoami` | Display the current user |
| `pwd` | Print the current working directory |
| `ls` | List visible directory contents |
| `ls -a` | List contents including hidden entries |
| `ls -la` | Display detailed information including hidden entries, ownership, and permissions |
| `hostname` | Display the system hostname |
| `uname -m` | Display the system architecture |
| `cat /etc/os-release` | Display operating system identification information |
| `free -h` | Display memory and swap usage in human-readable form |
| `df -h` | Display filesystem storage usage in human-readable form |
| `ip addr` | Inspect network interfaces and assigned addresses |
| `ip route` | Inspect the IPv4 routing table and default route |
| `cat /etc/resolv.conf` | Inspect DNS resolver configuration |
| `ping -c 4 1.1.1.1` | Test external IP connectivity |
| `ping -c 4 kali.org` | Test DNS name resolution and external reachability |

## System Baseline

The system was verified as:

| Component | Baseline |
|---|---|
| User | `Standard non-root user` |
| Home directory | `User home directory` |
| Hostname | `kali` |
| Architecture | `aarch64` / ARM64 |
| Operating system | Kali GNU/Linux Rolling |
| Version | 2026.2 |
| Usable memory | Approximately 2.9 GiB |
| Swap | Approximately 1.0 GiB |
| Root filesystem | `/dev/vda3` |
| Root filesystem size | Approximately 18 GiB |

The filesystem was already using a significant portion of the available virtual disk, so storage usage will continue to be monitored before installing additional tools or storing large datasets.

## Filesystem Exploration

The first commands focused on understanding identity and location within Linux.

`whoami` confirmed the active user account.

`pwd` confirmed that the working directory was:

```text
User home directory
```

`ls` displayed the visible contents of the home directory.

`ls -a` revealed hidden files and directories whose names begin with a period.

Examples included configuration-related directories such as:

```text
.config
.local
.ssh
```

No hidden configuration or authentication files were modified during this exercise.

## Ownership and Permissions

Running:

```bash
ls -la
```

introduced the Linux long-listing format and the concepts of:

- File types
- Owners
- Groups
- Read permissions
- Write permissions
- Execute permissions
- Modification timestamps

This established the foundation for a future project focused specifically on Linux administration, users, groups, and permissions.

## Operating System and Architecture

The architecture was verified with:

```bash
uname -m
```

which returned:

```text
aarch64
```

The operating system was inspected with:

```bash
cat /etc/os-release
```

which identified the system as Kali GNU/Linux Rolling version 2026.2.

This connected the ARM64 virtual-machine configuration used during installation with the architecture reported by the running operating system.

## Memory and Storage

Memory was inspected with:

```bash
free -h
```

This introduced the concepts of:

- Total memory
- Used memory
- Free memory
- Buffer/cache memory
- Available memory
- Swap

Filesystem storage was inspected with:

```bash
df -h
```

The root filesystem was identified as:

```text
/dev/vda3
```

mounted at:

```text
/
```

This connected the virtual disk partitions created during Project 01 with the filesystems visible from the running Kali system.

## Network Baseline

The VM network configuration was inspected using:

```bash
ip addr
```

and:

```bash
ip route
```

These commands introduced:

- Network interfaces
- Loopback
- IPv4 addressing
- IPv6 addressing
- Network prefixes
- Default routes
- Default gateways
- Source addresses
- Routing tables

The active virtual network interface was verified as `eth0`.

Specific network identifiers are intentionally not included in this public documentation because they are not necessary to demonstrate the learning outcome.

## DNS Configuration

The first attempt to inspect DNS used:

```bash
resolvectl status
```

The command was not installed on this Kali system, and the shell suggested installing an additional package.

Instead of installing software unnecessarily, the existing resolver configuration was inspected using:

```bash
cat /etc/resolv.conf
```

This provided the information needed to verify that DNS resolver configuration was present.

This was an important troubleshooting lesson: when one utility is unavailable, first determine whether the required information can be obtained using tools and configuration already available on the system.

## Connectivity Testing

External IP connectivity was tested with:

```bash
ping -c 4 1.1.1.1
```

The test completed successfully with all four packets receiving replies and no observed packet loss in that sample.

DNS name resolution and external reachability were then tested with:

```bash
ping -c 4 kali.org
```

The hostname successfully resolved to an IP address, and all four probes received replies.

Using both tests demonstrated the difference between:

1. Testing reachability directly by IP address
2. Testing hostname resolution before communicating with the destination

This provides a simple troubleshooting method for separating general connectivity problems from DNS-related problems.

## Troubleshooting Lessons

Several small errors during the baseline became useful learning opportunities.

For example, an incorrect path was initially entered while attempting to read `/etc/os-release`.

Linux returned a `No such file or directory` message.

Instead of treating the error as a failure, the path was reviewed, corrected, and the command was run successfully.

The unavailable `resolvectl` command provided another lesson: do not automatically install software simply because the shell suggests doing so. First understand what information is needed and whether an existing tool can provide it.

## SOC Relevance

The skills practiced during this project are fundamental to security operations.

A SOC or cybersecurity analyst may need to quickly determine:

- Which user is active
- Which host is being investigated
- What operating system is running
- What files and configurations are present
- File ownership and permissions
- Available system resources
- Network interfaces and addressing
- Routing configuration
- DNS configuration
- Whether a system can communicate with other hosts

These baseline checks help establish context before deeper investigation begins.

## What I Learned

After completing this project, I can:

- Identify the active Linux user
- Determine my current filesystem location
- Inspect visible and hidden directory contents
- Recognize basic Linux ownership and permission information
- Identify the system hostname
- Verify the operating-system architecture
- Identify the installed Kali Linux release
- Inspect memory and swap usage
- Inspect filesystem storage usage
- Identify network interfaces and addressing
- Read the routing table
- Identify the default route
- Inspect DNS configuration
- Test external IP connectivity
- Test hostname resolution
- Interpret common command-line errors instead of immediately assuming something is broken

## Result

The Kali Linux virtual machine baseline was successfully established.

The operating system, architecture, system resources, filesystem, network configuration, routing, DNS configuration, and external connectivity were inspected and verified before moving into deeper Linux administration and cybersecurity exercises.

## Next Project

**Project 03: Linux Administration & Permissions**

The next project will move from observing the Linux system to actively administering it, including:

- Directory navigation
- File and directory creation
- Copying and moving files
- File removal
- Text inspection and searching
- Users and groups
- Linux permissions
- `sudo`
- Processes
- Services
- Logs
- Package management
