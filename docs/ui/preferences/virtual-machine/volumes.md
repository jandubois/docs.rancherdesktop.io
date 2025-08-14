---
sidebar_label: Volumes
title: Volumes (macOS & Linux)
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/virtual-machine/volumes"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Volumes** tab allows you to configure how your local file system is mounted into the Rancher Desktop virtual machine on macOS and Linux.

<Tabs groupId="os">
<TabItem value="macOS">

![](rd-versioned-asset://preferences/macOS_virtualMachine_tabVolumes.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://preferences/Linux_virtualMachine_tabVolumes.png)

</TabItem>
</Tabs>

### Mount Type

You can choose from the following mount types:

-   **reverse-sshfs:** The default mount type. This option exposes the file system by running an SFTP server on the host.
-   **9p (Experimental):** This option uses QEMU's `virtio-9p-pci` devices to expose the file system.
-   **virtiofs (Experimental):** This option uses the `virtiofs` shared directory device, which is implemented using Apple's `Virtualization.Framework`.

### 9p Options

When using the `9p` mount type, you can configure the following options:

-   **Cache Mode:** Specifies the caching policy. The default is `mmap`.
-   **Memory Size In KiB:** The packet size for `9p` messages. The default is `128` KiB.
-   **Protocol Version:** The `9p` protocol version. The default is `9p2000.L`.
-   **Security Model:** The security model to use. The default is `none`.
