---
title: Installing and Uninstalling Rancher Desktop Extensions
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/installing-uninstalling-extensions"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

Rancher Desktop's **Extensions** feature allows you to enhance the application's functionality by installing and using Docker Desktop Extensions. This guide will walk you through the process of installing and uninstalling extensions.

## Installing Extensions

You can install extensions either through the Rancher Desktop user interface or from the command line.

### Using the UI

1.  In the main Rancher Desktop window, click on **Extensions** in the side menu.
2.  In the **Catalog** tab, you can browse through the available extensions.
3.  Click the **Install** button for the extension you wish to add.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://ui-main/Windows_Extensions.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://ui-main/macOS_Extensions.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://ui-main/Linux_Extensions.png)

</TabItem>
</Tabs>

### Using the Command Line

You can also use the `rdctl` command-line tool to install extensions.

```bash
rdctl extension install <image-id>:<tag>
```

> Note: The `<tag>` parameter is optional.

## Uninstalling Extensions

You can uninstall extensions either through the user interface or from the command line.

### Using the UI

You can uninstall an extension from either the **Catalog** tab or the **Installed** tab.

-   From the **Catalog** tab, find the installed extension and click the **Uninstall** button.
-   From the **Installed** tab, click the **Uninstall** button next to the extension you wish to remove.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://ui-main/Windows_Extensions-Installed.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://ui-main/macOS_Extensions-Installed.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://ui-main/Linux_Extensions-Installed.png)

</TabItem>
</Tabs>

### Using the Command Line

To uninstall an extension from the command line, use the `rdctl extension uninstall` command:

```bash
rdctl extension uninstall <image-id>:<tag>
```

> Note: The `<tag>` parameter is optional.
