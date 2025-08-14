---
title: Architecture
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/references/architecture"/>
</head>

![Rancher Desktop Architecture](../img/how-it-works-rancher-desktop.svg)

Rancher Desktop is an Electron-based application that provides a simple, unified user experience for container management and Kubernetes.

On macOS and Linux, Rancher Desktop uses a virtual machine to run `containerd` or `dockerd` and Kubernetes. On Windows, it leverages the Windows Subsystem for Linux (WSL) v2 for the same purpose. This architecture allows Rancher Desktop to provide a consistent experience across all platforms while using the best available technology for each operating system.