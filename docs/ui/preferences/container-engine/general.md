---
sidebar_label: General
title: General
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/container-engine/general"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **General** tab allows you to configure the container runtime and enable experimental features.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://preferences/Windows_containerEngine_tabGeneral.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://preferences/macOS_containerEngine_tabGeneral.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://preferences/Linux_containerEngine_tabGeneral.png)

</TabItem>
</Tabs>

### Container Runtime

Rancher Desktop allows you to choose between two container runtimes:

-   **[containerd]**: A standard container runtime that can be used with its own CLI, `nerdctl`. `containerd` also supports namespaces for isolating containers.
-   **[dockerd (moby)]**: The open-source components of the Docker ecosystem, which allows you to use the Docker CLI.

> **Important:** Only one container runtime can be active at a time. When you switch between runtimes, any workloads or images associated with the previous runtime will not be available.

### WebAssembly (Wasm)

This setting enables experimental support for running WebAssembly applications as containers. For more information, please refer to the [Working with WebAssembly tutorial](../../../tutorials/working-with-webassembly.md).

> **Note:** When using the `moby` container engine, enabling Wasm support will switch to a different image store. Any images you have previously built or downloaded will not be available until you disable Wasm support again.

[container runtime]:
https://kubernetes.io/docs/setup/production-environment/container-runtimes/

[containerd]:
https://containerd.io/

[dockerd (moby)]:
https://mobyproject.org/
