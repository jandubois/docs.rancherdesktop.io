---
sidebar_label: Snapshots
title: Snapshots
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/snapshots"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Snapshots** tab allows you to create, restore, and delete snapshots of your Rancher Desktop environment. A snapshot captures the current state of your virtual machine, including all settings and configurations.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![Snapshots_Example](rd-versioned-asset://ui-main/Windows_Snapshots-List.png)

</TabItem>
<TabItem value="macOS">

![Snapshots_Example](rd-versioned-asset://ui-main/macOS_Snapshots-List.png)

</TabItem>
<TabItem value="Linux">

![Snapshots_Example](rd-versioned-asset://ui-main/Linux_Snapshots-List.png)

</TabItem>
</Tabs>

Snapshots are stored in the `snapshots` directory in the following locations:

-   **macOS:** `~/Library/Application Support/rancher-desktop/snapshots`
-   **Linux:** `~/.local/share/rancher-desktop/snapshots`
-   **Windows:** `%LOCALAPPDATA%\rancher-desktop\snapshots`

You can move the `snapshots` directory to a different location by creating a symbolic link from the default path to your preferred location.

### Creating a Snapshot

1.  Click the **Create Snapshot** button.
2.  In the dialog that appears, enter a name and an optional description for your snapshot.
3.  Click the **Create** button.

> **Note:** Rancher Desktop will be unavailable while the snapshot is being created.

### Restoring a Snapshot

1.  From the list of snapshots, select the one you want to restore.
2.  Click the **Restore** button.

> **Note:** Restoring a snapshot will replace your current environment, including all settings and configurations. Rancher Desktop will be unavailable while the snapshot is being restored.

### Deleting a Snapshot

1.  From the list of snapshots, select the one you want to delete.
2.  Click the **Delete** button.

### Using the Command Line

You can also manage snapshots from the command line using the `rdctl snapshot` command. For more information, please refer to the [`rdctl` command reference](../references/rdctl-command-reference.md).
