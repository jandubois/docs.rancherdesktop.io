---
sidebar_label: Integrations
title: Integrations
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/wsl/integrations"/>
</head>

The **Integrations** tab allows you to make the Rancher Desktop Kubernetes configuration accessible to your WSL distributions. When this option is enabled, you can use tools like `kubectl` from within your WSL distributions to communicate with the Rancher Desktop Kubernetes cluster.

![](rd-versioned-asset://preferences/Windows_wsl_tabIntegrations.png)

> **Note on Resource Allocation:** In WSL, memory and CPU allocation are configured globally across all Linux distributions. For more information, please refer to the [WSL documentation].

[WSL documentation]: https://docs.microsoft.com/en-us/windows/wsl/wsl-config#options-for-wslconfig

### `~/.kube/config`

When you enable a WSL integration, Rancher Desktop creates a symbolic link from `~/.kube/config` inside the WSL distribution to the `~/.kube/config` file on your Windows file system.

-   If the `~/.kube/config` file inside the WSL distribution already exists and only contains a `rancher-desktop` context, it will be replaced with the symbolic link.
-   If the file contains any other information, it will not be modified. In this case, you will need to manually update the Kubernetes context inside the WSL distribution.

:::info
In Rancher Desktop versions `1.11.1` and `1.12.*`, the `~/.kube/config` file was converted from a symbolic link to a regular file to support network tunneling. As of version `1.13.0`, a network proxy is used instead, and the file will be converted back to a symbolic link if it only contains a `rancher-desktop` context.
:::
