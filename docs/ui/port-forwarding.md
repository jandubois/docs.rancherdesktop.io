---
sidebar_label: Port Forwarding
title: Port Forwarding
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/port-forwarding"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Port Forwarding** tab allows you to forward ports from your local machine to services running in your Kubernetes cluster. This is useful for accessing applications running in your cluster from your local machine.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://ui-main/Windows_PortForwarding.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://ui-main/macOS_PortForwarding.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://ui-main/Linux_PortForwarding.png)

</TabItem>
</Tabs>

### Forwarding a Port

To forward a port for a service:

1.  In the list of services, find the one you want to forward and click the **Forward** button.
2.  In the dialog that appears, you can either specify a port to use or accept the randomly assigned port.
3.  Click the checkmark button to confirm your selection.
4.  To stop forwarding a port, click the **Cancel** button.

### Admin vs. Non-Admin Port Mappings

Rancher Desktop supports automated port forwarding. For users without administrative privileges, port mappings are configured to the localhost and are limited to unprivileged ports (greater than 1024).

If you have administrative privileges, you can also configure port mappings for privileged ports (less than or equal to 1024).

:::note
You may also need to configure port access at the operating system level. For more information, please see the documentation on [Traefik port binding access](../getting-started/installation.md#traefik-port-binding-access).
:::
