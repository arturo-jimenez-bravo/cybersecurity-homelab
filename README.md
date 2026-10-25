# Cybersecurity Homelab

A hands-on cybersecurity homelab documenting my development in Linux administration, SOC analysis, networking, detection engineering, defensive security, automation, and AI security.

## Overview

This repository documents the construction and development of my personal cybersecurity homelab.

The lab is designed to provide practical experience with operating systems, networking, security monitoring, incident investigation, threat detection, and security automation. Each project will include configuration decisions, technical notes, troubleshooting results, screenshots, commands, and lessons learned.

## Current Environment

| Component | Configuration |
|---|---|
| Host system | Apple Silicon MacBook Pro |
| Virtualization platform | UTM |
| Guest operating system | Kali Linux ARM64 |
| Desktop environment | Xfce |
| VM architecture | ARM64 / aarch64 |
| VM memory | 3 GB |
| Virtual disk | 21.5 GB VirtIO |
| Network mode | UTM Shared Network |
| Display device | virtio-gpu-pci |

## Lab Architecture

```text
Apple Silicon MacBook Pro
└── UTM
    └── Kali Linux ARM64
        ├── Linux administration
        ├── Networking
        ├── Security tools
        ├── SOC analysis
        ├── Detection engineering
        └── AI security
```
The architecture will expand as additional virtual machines, monitoring systems, vulnerable targets, and defensive security tools are added.

## Projects

### Project 01: Kali Linux ARM64 on Apple Silicon

**Status:** Installation completed

The first project establishes the Kali Linux virtual machine that will serve as the initial cybersecurity workstation in the homelab.

Topics documented include:

- Creating an ARM64 virtual machine in UTM
- Booting the Kali Linux installer
- Troubleshooting a black installer screen
- Installing through the UTM Serial console
- Configuring the user account and network settings
- Partitioning the virtual disk
- Installing the Xfce desktop environment
- Removing the installer media
- Restoring the graphical display
- Completing the first successful boot

Detailed documentation and screenshots will be added under the `kali-linux` directory.

## Planned Areas

- Linux administration
- Network fundamentals
- SOC workflows
- Log analysis
- Security monitoring
- Threat detection
- Incident response
- Detection engineering
- Security automation
- AI security
- Vulnerability assessment in authorized lab environments

## Skills Being Developed

- Virtual-machine deployment
- Linux command-line administration
- Technical troubleshooting
- Network configuration
- Security-event analysis
- Documentation with Markdown
- Git and GitHub workflows
- Responsible use of cybersecurity tools

## Progress

- [x] Install UTM
- [x] Create Kali Linux ARM64 virtual machine
- [x] Resolve the blank installer display
- [x] Complete Kali Linux installation
- [x] Boot into the Kali Xfce desktop
- [x] Create the GitHub homelab repository
- [ ] Document the complete Kali installation
- [ ] Verify the Kali system baseline
- [ ] Update the operating system
- [ ] Configure snapshots and recovery
- [ ] Build the first networking lab
- [ ] Begin SOC and detection exercises

## Responsible Use

All exercises documented in this repository are intended for educational purposes and will be performed only within systems that I own or have explicit authorization to test.

No passwords, private keys, access tokens, API keys, or sensitive personal information will be committed to this repository.

## Documentation Approach

Each lab will document:

1. Objective
2. Environment
3. Configuration
4. Commands and procedures
5. Problems encountered
6. Troubleshooting process
7. Results
8. Lessons learned
9. Supporting screenshots

## Current Milestone

Kali Linux ARM64 is successfully installed and running through UTM on an Apple Silicon MacBook Pro.

The next milestone is documenting the completed installation before making additional changes to the Kali system.
