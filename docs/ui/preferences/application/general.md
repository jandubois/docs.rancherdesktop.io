---
sidebar_label: General
title: General
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/application/general"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **General** tab allows you to configure application-wide settings, such as automatic updates and administrative access.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://preferences/Windows_application_tabGeneral.png)

-   **Automatic Updates:** When enabled, Rancher Desktop will automatically download and install new updates.
-   **Statistics:** When enabled, Rancher Desktop will collect anonymous information about how you interact with the application. This information is used to help improve the application and does not include any personal or sensitive data.

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://preferences/macOS_application_tabGeneral.png)

-   **Administrative Access:** Enabling this option allows Rancher Desktop to create the Docker socket at the default location (`/var/run/docker.sock`) and to use a bridged IP address that is reachable from other machines on your local network.

    :::note
    The bridged IP address is provided by Apple’s `vmnet` framework, which uses port 53. As a result, you cannot run a DNS server in a container and forward it to port 53 on the host when administrative access is enabled.
    :::

-   **Automatic Updates:** When enabled, Rancher Desktop will automatically download and install new updates.
-   **Statistics:** When enabled, Rancher Desktop will collect anonymous information about how you interact with the application. This information is used to help improve the application and does not include any personal or sensitive data.

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://preferences/Linux_application_tabGeneral.png)

-   **Administrative Access:** Enabling this option allows Rancher Desktop to create the Docker socket at the default location (`/var/run/docker.sock`).

-   **Automatic Updates:** When enabled, Rancher Desktop will automatically download and install new updates.
-   **Statistics:** When enabled, Rancher Desktop will collect anonymous information about how you interact with the application. This information is used to help improve the application and does not include any personal or sensitive data.

</TabItem>
</Tabs>
