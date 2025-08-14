---
sidebar_label: Environment
title: Environment
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/application/environment"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Environment** tab allows you to configure how the Rancher Desktop command-line utilities are added to your system's `PATH`.

<Tabs groupId="os">
<TabItem value="macOS">

![](rd-versioned-asset://preferences/macOS_application_tabEnvironment.png)

#### Configure PATH

Rancher Desktop includes several command-line utilities for interacting with its features, such as `docker`, `nerdctl`, `kubectl`, and `helm`. These utilities are located in the `~/.rd/bin` directory. For you to be able to use them from the command line, this directory must be included in your shell's `PATH` environment variable.

There are two options for managing the `PATH`:

-   **Automatic:** Rancher Desktop will automatically add the `~/.rd/bin` directory to your `PATH` by modifying your shell's `.rc` files.
-   **Manual:** You will need to manually add the `~/.rd/bin` directory to your `PATH` yourself.

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://preferences/Linux_application_tabEnvironment.png)

#### Configure PATH

Rancher Desktop includes several command-line utilities for interacting with its features, such as `docker`, `nerdctl`, `kubectl`, and `helm`. These utilities are located in the `~/.rd/bin` directory. For you to be able to use them from the command line, this directory must be included in your shell's `PATH` environment variable.

There are two options for managing the `PATH`:

-   **Automatic:** Rancher Desktop will automatically add the `~/.rd/bin` directory to your `PATH` by modifying your shell's `.rc` files.
-   **Manual:** You will need to manually add the `~/.rd/bin` directory to your `PATH` yourself.

</TabItem>
</Tabs>
