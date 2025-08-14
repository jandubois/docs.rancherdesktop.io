---
sidebar_label: Troubleshooting
title: Troubleshooting
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/troubleshooting"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Troubleshooting** tab provides several options to help you diagnose and resolve issues with Rancher Desktop.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://ui-main/Windows_Troubleshooting.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://ui-main/macOS_Troubleshooting.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://ui-main/Linux_Troubleshooting.png)

</TabItem>
</Tabs>

### Show Logs

Clicking the **Show Logs** button will open the directory containing the Rancher Desktop log files. These files are essential for diagnosing and reporting issues.

### Enable Debug Mode

The **Enable Debug Mode** checkbox increases the verbosity of the logs, which can be helpful for troubleshooting complex issues.

### Reset Kubernetes

Clicking the **Reset Kubernetes** button will delete the current Kubernetes cluster and reset it to its initial state. This will also delete all workloads and configurations.

When you click the button, a confirmation dialog will appear, giving you the option to also delete your container images.

### Factory Reset

A factory reset will completely remove the Kubernetes cluster and all other Rancher Desktop settings. After a factory reset, you will need to go through the initial setup process again.

When you click the **Factory Reset** button, a confirmation dialog will appear, giving you the option to keep the cache of Kubernetes images.

