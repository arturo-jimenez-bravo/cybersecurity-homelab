# Kali Linux ARM64 Installation on Apple Silicon

## Purpose

This document records the installation of Kali Linux ARM64 on an Apple Silicon MacBook Pro using UTM.

The goal was to create the first virtual machine in my cybersecurity homelab and establish a working Kali Linux environment for future hands-on security training.

## Environment

| Component | Configuration |
|---|---|
| Host system | Apple Silicon MacBook Pro |
| Virtualization platform | UTM |
| Guest operating system | Kali Linux ARM64 |
| Architecture | ARM64 / aarch64 |
| Memory | 3 GB |
| Virtual disk | 21.5 GB VirtIO |
| Network mode | UTM Shared Network |
| Desktop environment | Xfce |
| Display | virtio-gpu-pci |
| Hostname | kali |

## 1. Preparing the Virtual Machine

UTM was used as the virtualization platform for running Kali Linux on Apple Silicon.

A Kali Linux ARM64 installer image was attached to the virtual CD/DVD drive so the virtual machine could boot into the installer.

## 2. Starting the Kali Installer

After starting the virtual machine, the Kali Linux boot menu appeared.

The menu included options such as:

- Install
- Graphical install
- Advanced options
- Accessibility installation options

The standard `Install` option was selected.

## 3. Installer Display Problem

After starting the installer, the graphical display became blank.

Because the Kali boot menu had loaded successfully, the installation media was bootable. The problem occurred when attempting to view the installer.

A Serial device was added to the UTM virtual machine configuration.

The Serial device was configured using:

- Mode: Built-in Terminal
- Target: Automatic Serial Device

After restarting the virtual machine, UTM provided both the graphical display and a Serial display.

Switching to `Serial 1` successfully displayed the Kali text installer.

The complete troubleshooting process is documented separately in `troubleshooting.md`.

## 4. Language and Regional Configuration

The following settings were selected during installation:

| Setting | Selection |
|---|---|
| Language | English |
| Location | United States |
| Keyboard | American English |
| Time zone | Pacific |

## 5. Network Configuration

The Kali hostname was configured as:

```text
kali
```

The domain-name field was left blank because this virtual machine was not being joined to a managed domain during the initial installation.

## 6. User Account

A standard Linux user account was created during installation.

The password is intentionally not documented or stored in this repository.

## 7. Virtual Disk Configuration

The installer detected the UTM virtual disk as:

```text
Virtual disk 1 (vda)
21.5 GB VirtIO Block Device
```

The following partitioning method was selected:

```text
Guided - use entire disk
```

This referred to the UTM virtual disk presented to Kali, not the physical storage device in the MacBook Pro.

The partitioning scheme selected was:

```text
All files in one partition
```

The proposed disk layout included:

- EFI System Partition
- Linux ext4 root filesystem
- Swap space

The partition configuration was reviewed before allowing the installer to write changes to the virtual disk.

## 8. Software Selection

The Xfce desktop environment was retained as the graphical desktop environment.

The installation configuration included:

```text
Xfce           Selected
GNOME          Not selected
KDE Plasma     Not selected
```

The Kali tool collection was also retained during installation.

## 9. Completing the Installation

After the required packages and desktop environment were installed, Kali reported that the installation was complete.

Before performing the first normal boot, the virtual machine was stopped so the temporary installation configuration could be cleaned up.

## 10. Removing the Installation Media

The Kali installer image was removed from the UTM virtual CD/DVD drive.

The drive then displayed:

```text
CD/DVD: (empty)
```

Removing the installer media ensured that the next boot used the installed Kali system instead of returning to the installer.

## 11. Removing the Temporary Serial Console

The Serial device used during installation was removed from the UTM virtual machine configuration.

The graphical display was verified as:

```text
virtio-gpu-pci
```

Auto Resolution remained enabled.

## 12. First Normal Boot

The virtual machine was started again after the installation media and temporary Serial device were removed.

Kali successfully reached the graphical login screen.

After authentication, the Kali Xfce desktop loaded successfully.

This confirmed that:

- Kali Linux was installed to the virtual disk
- The ARM64 virtual machine booted successfully
- The graphical desktop was functional
- The temporary installer configuration was no longer required

## Result

Kali Linux ARM64 is now successfully installed and running through UTM on the Apple Silicon MacBook Pro.

This virtual machine establishes the first working system in the cybersecurity homelab.

## Next Steps

- Add installation screenshots
- Document the Serial-console troubleshooting process
- Verify the Kali system baseline
- Verify network connectivity
- Update Kali Linux
- Configure VM snapshots and recovery
- Begin Linux, networking, SOC, and security exercises

## Security Notes

Passwords, authentication secrets, API keys, tokens, and other sensitive information are intentionally excluded from this repository.

All future cybersecurity exercises documented in this repository will be performed only on systems that I own or have explicit authorization to test.
