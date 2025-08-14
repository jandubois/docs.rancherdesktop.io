---
sidebar_label: Emulation
title: Emulation (macOS)
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/virtual-machine/emulation"/>
</head>

The **Emulation** tab allows you to configure the virtualization technology used by Rancher Desktop on macOS.

![](rd-versioned-asset://preferences/macOS_virtualMachine_tabEmulation.png)

### Virtual Machine Type

You can choose between two virtual machine types:

-   **QEMU:** The default option, [QEMU](https://www.qemu.org/documentation/) is a general-purpose machine emulator and virtualizer.
-   **VZ:** This option uses the native macOS [Virtualization.Framework](https://developer.apple.com/documentation/virtualization) for running the virtual machine.

### Rosetta

When using the VZ virtual machine type on Apple Silicon, you can enable Rosetta support to run x86_64 (Intel) binaries. This is useful for running container images that have not been built for the ARM64 architecture.
