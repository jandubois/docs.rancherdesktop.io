---
sidebar_label: Containers
title: Containers
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/containers"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![Containers_Example](rd-versioned-asset://ui-main/Windows_Containers.png)

</TabItem>
<TabItem value="macOS">

![Containers_Example](rd-versioned-asset://ui-main/macOS_Containers.png)

</TabItem>
<TabItem value="Linux">

![Containers_Example](rd-versioned-asset://ui-main/Linux_Containers.png)

</TabItem>
</Tabs>

The **Containers** tab allows you to view and manage your running containers. The main view provides a list of all your containers, along with the following key information:

-   **State:** The current state of the container (e.g., "Running," "Stopped").
-   **Name:** The name of the container.
-   **Image:** The image used to create the container.
-   **Port(s):** Any ports that are exposed by the container. You can click on a port to open it in your web browser.
-   **Started:** The time when the container was started.

You can sort the list by any of these fields and use the filter box to search for specific containers. You can also select multiple containers to perform bulk actions.

When using the `containerd` runtime, a **Namespace** dropdown will be available, allowing you to filter containers by namespace.

### Container Management

The **Containers** tab provides several options for managing your containers:

-   **Stop:** Stops a running container.
-   **Start:** Starts a stopped container.
-   **Delete:** Deletes a container from your system.
-   **⋮:** The kebab menu provides additional options for each container, such as starting, stopping, and deleting.

The management buttons at the top of the view will be enabled or disabled depending on the state of the selected container(s).
