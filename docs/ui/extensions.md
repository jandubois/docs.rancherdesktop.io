---
sidebar_label: Extensions
title: Extensions
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/extensions"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Extensions** section allows you to discover, install, and manage extensions for Rancher Desktop.

### Catalog

The **Catalog** tab serves as a marketplace for available Rancher Desktop extensions. From this tab, you can browse through the list of extensions, view their descriptions, and install them directly through the UI. You can also use the search bar to find specific extensions.

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

### Installed

The **Installed** tab displays a list of all the extensions that are currently installed in your Rancher Desktop application. From this tab, you can uninstall an extension by clicking the **Uninstall** button next to its name.

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

### Remote Debugging Extensions

If you are developing extensions, you can use the Chrome DevTools to debug your extension in Rancher Desktop. For more information, please refer to the instructions on [remote debugging an extension](https://github.com/rancher-sandbox/rancher-desktop/#remote-debugging-an-extension) in the Rancher Desktop repository.
