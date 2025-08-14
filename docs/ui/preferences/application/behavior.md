---
sidebar_label: Behavior
title: Behavior
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/application/behavior"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Behavior** tab allows you to configure various aspects of the application's behavior, including startup, background processes, and the notification icon.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://preferences/Windows_application_tabBehavior.png)

#### Startup

You can configure Rancher Desktop to start automatically when you log in to your computer. When this option is enabled, all other behavior settings will be applied on startup.

#### Background

-   **Start in Background:** When this option is enabled, Rancher Desktop will start without a main application window or an icon in the taskbar. You can still access the application window from the notification icon's context menu.
-   **Quit on Close:** By default, Rancher Desktop will continue to run in the background even when the main window is closed. If you enable this option, the application will terminate when the main window is closed.

#### Notification Icon

Rancher Desktop displays a notification icon in the system tray to indicate the application's status and provide quick access to other features. You can disable the notification icon by unchecking the **Show Notification Icon** option.

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://preferences/macOS_application_tabBehavior.png)

#### Startup

You can configure Rancher Desktop to start automatically when you log in to your computer. When this option is enabled, all other behavior settings will be applied on startup.

#### Background

-   **Start in Background:** When this option is enabled, Rancher Desktop will start without a main application window or an icon in the dock. You can still access the application window from the notification icon's context menu.
-   **Quit on Close:** By default, Rancher Desktop will continue to run in the background even when the main window is closed. If you enable this option, the application will terminate when the main window is closed.

#### Notification Icon

Rancher Desktop displays a notification icon in the menu bar to indicate the application's status and provide quick access to other features. You can disable the notification icon by unchecking the **Show Notification Icon** option.

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://preferences/Linux_application_tabBehavior.png)

#### Startup

You can configure Rancher Desktop to start automatically when you log in to your computer. When this option is enabled, all other behavior settings will be applied on startup.

#### Background

-   **Start in Background:** When this option is enabled, Rancher Desktop will start without a main application window or an icon in the task switcher. You can still access the application window from the notification icon's context menu.
-   **Quit on Close:** By default, Rancher Desktop will continue to run in the background even when the main window is closed. If you enable this option, the application will terminate when the main window is closed.

#### Notification Icon

Rancher Desktop displays a notification icon in the system tray to indicate the application's status and provide quick access to other features. You can disable the notification icon by unchecking the **Show Notification Icon** option.

:::note
On Ubuntu 20.04.5 LTS and newer, there is a known issue with hiding the notification icon. For more information, please see [this issue comment](https://github.com/rancher-sandbox/rancher-desktop/issues/4205#issuecomment-1533750167).
:::

</TabItem>
</Tabs>
