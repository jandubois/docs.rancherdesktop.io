---
sidebar_label: Kubernetes
title: Kubernetes
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/kubernetes"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Kubernetes** tab allows you to configure your local Kubernetes cluster.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://preferences/Windows_kubernetes.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://preferences/macOS_kubernetes.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://preferences/Linux_kubernetes.png)

</TabItem>
</Tabs>

#### Enable Kubernetes

You can enable or disable Kubernetes by checking the **Enable Kubernetes** checkbox. If you do not need to run Kubernetes, you can disable it to conserve resources. When you disable Kubernetes, any existing workloads and configurations will be preserved and will become available again when you re-enable it.

#### Kubernetes Version

This dropdown menu allows you to select the version of Kubernetes you want to run.

> **Note:** When you switch between Kubernetes versions, your existing workloads and images will be affected.
>
> -   **Upgrading:** Workloads and images are retained.
> -   **Downgrading:** Workloads are removed, but images are retained.

#### Kubernetes Port

This setting allows you to specify the port on which the Kubernetes API server is exposed. This is useful for avoiding port collisions if you are running multiple Kubernetes clusters on the same machine.

### Options

#### Enable Traefik

This option allows you to enable or disable the built-in [Traefik](https://traefik.io/) Ingress controller. If you want to use a different Ingress controller, you can disable Traefik to free up ports 80 and 443.

#### Install Spin Operator

This setting enables experimental support for running WebAssembly applications in Kubernetes using the [Spin](https://www.spinkube.dev/) operator.

> **Note:** This feature requires that [WebAssembly support](./container-engine/general.md) is enabled in the **Container Engine** tab.
