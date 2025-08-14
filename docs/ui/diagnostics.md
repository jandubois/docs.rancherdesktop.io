---
sidebar_label: Diagnostics
title: Diagnostics
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/diagnostics"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Diagnostics** tab runs a series of checks to detect common problems in your environment, such as missing minimum requirements or misconfigurations. This feature is designed to help you troubleshoot and resolve issues with the Rancher Desktop application.

The diagnostics checks are run automatically every time the application launches. If any problems are identified, a badge will be displayed next to the **Diagnostics** menu item, indicating the number of failed checks.

:::note
Rancher Desktop does not send any diagnostics data to a remote server for processing or storage. All diagnostics are performed locally on your machine.
:::

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://ui-main/Windows_Diagnostics.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://ui-main/macOS_Diagnostics.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://ui-main/Linux_Diagnostics.png)

</TabItem>
</Tabs>

From this tab, you can view the results of the diagnostics tests and see which checks have passed or failed. For failed checks, Rancher Desktop will provide guidance on how to resolve the issue.

You can also perform the following actions:

-   **Mute/Unmute:** If you have a non-standard setup and know that a particular check is not relevant to your environment, you can mute it to prevent it from running.
-   **Rerun:** You can rerun the diagnostics at any time to verify that any changes you have made to your environment have resolved the issue.
