 # Troubleshooting: Blank Kali Installer Display in UTM

## Problem

During the installation of Kali Linux ARM64 in UTM on an Apple Silicon MacBook Pro, the Kali boot menu loaded successfully.

After selecting:

```text
Install
```

the graphical display became blank instead of displaying the Kali installer.

A terminal-style cursor was visible, but the expected installation interface did not appear.

## Initial Observation

The Kali boot menu had already loaded successfully before the blank screen occurred.

This indicated that the virtual machine was able to boot from the Kali installation media.

The problem appeared after launching the installer and was related to accessing the installer's display.

## Troubleshooting Approach

Instead of repeatedly restarting the installation or switching to another installer option, the UTM virtual machine configuration was inspected.

A Serial console was added so the Kali installer output could be accessed through a text-based interface.

## Solution

### 1. Stop the Virtual Machine

The Kali virtual machine was stopped and returned to the UTM virtual machine library.

### 2. Open the Virtual Machine Settings

The Kali virtual machine settings were opened in UTM.

Under the Devices section, a new Serial device was added.

### 3. Configure the Serial Device

The Serial device was configured as:

```text
Mode: Built-in Terminal
Target: Automatic Serial Device
```

The configuration was saved.

### 4. Restart the Virtual Machine

The Kali virtual machine was started again.

The Kali boot menu appeared normally.

The standard:

```text
Install
```

option was selected.

### 5. Switch to the Serial Display

UTM now provided two display options:

```text
Display 1: virtio-gpu-pci
Serial 1
```

The active display was switched to:

```text
Serial 1
```

### 6. Installer Appeared

After switching to the Serial console, the Kali text installer became visible.

Installation could then continue normally through the text-based interface.

## Result

The blank graphical installer did not prevent Kali Linux from being installed.

Using the UTM Serial console provided access to the installer and allowed the installation to continue successfully.

## Post-Installation Cleanup

After Kali Linux finished installing:

1. The Kali installer ISO was removed from the virtual CD/DVD drive.
2. The temporary Serial device was removed from the UTM configuration.
3. The graphical display was verified as `virtio-gpu-pci`.
4. The virtual machine was started from the installed virtual disk.
5. Kali successfully reached the graphical login screen.
6. The Xfce desktop loaded successfully after authentication.

## What I Learned

This problem demonstrated several useful troubleshooting principles:

- A blank virtual-machine display does not automatically mean the virtual machine has crashed.
- Successful access to the boot menu can help narrow down where a failure is occurring.
- Virtual machines can expose more than one console or display interface.
- Installation output may be accessible through a Serial console even when the graphical display is unavailable.
- Temporary troubleshooting configuration should be removed when it is no longer required.
- Documenting both the problem and the solution creates a reusable troubleshooting reference.

## Final Configuration

After installation and cleanup, the relevant display configuration was:

```text
Display: virtio-gpu-pci
Serial device: Removed
Installer ISO: Removed
```

Kali Linux ARM64 was then able to boot normally into the graphical Xfce desktop.

## Related Documentation

- `README.md` - Kali Linux project overview
- `installation.md` - Complete Kali Linux installation process
- `screenshots/` - Visual documentation of the installation and troubleshooting process
