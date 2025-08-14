---
title: Increasing Open File Limit
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/increasing-open-file-limit"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

In some cases, the default `ulimit` for open files in Rancher Desktop may be too low for your workloads. This guide explains how to increase the open file limit using provisioning scripts.

## macOS & Linux

On macOS and Linux, you can use a provisioning script to increase the open file limit.

1.  **Create an `override.yaml` File**

    First, you will need to create an `override.yaml` file in the lima configuration directory. Please note that you must run Rancher Desktop at least once for this directory to be created.

<Tabs groupId="os">
<TabItem value="macOS">

**Path:** `~/Library/Application Support/rancher-desktop/lima/_config/override.yaml`

</TabItem>
<TabItem value="Linux">

**Path:** `~/.local/share/rancher-desktop/lima/_config/override.yaml`

</TabItem>
</Tabs>

2.  **Add the Provisioning Script**

    Add the following content to your `override.yaml` file to increase the `ulimit` for containers:

    ```yaml
    provision:
    - mode: system
      script: |
        #!/bin/sh
        cat <<'EOF' > /etc/security/limits.d/rancher-desktop.conf
        * soft     nofile         82920
        * hard     nofile         82920
        EOF
    ```

    If you are using the Elastic platform, you will also need to set the `vm.max_map_count` parameter. You can add this to the same script:

    ```yaml
    provision:
    - mode: system
      script: |
        #!/bin/sh
        cat <<'EOF' > /etc/security/limits.d/rancher-desktop.conf
        * soft     nofile         82920
        * hard     nofile         82920
        EOF
        sysctl -w vm.max_map_count=262144
    ```

3.  **Restart Rancher Desktop**

    For the changes to take effect, you must stop and restart Rancher Desktop.

## Windows

On Windows, you can use a provisioning script to increase the `max_map_count` parameter, which will raise the open file limit.

1.  **Create a Provisioning Script**

    First, ensure that you have run Rancher Desktop at least once to initialize the necessary configurations.

    Next, create a new file named `map_count.start` in the following directory:

    **Path:** `%LOCALAPPDATA%\rancher-desktop\provisioning`

    Add the following content to the file:

    ```sh
    #!/bin/sh
    sysctl -w vm.max_map_count=262144
    ```

2.  **Restart Rancher Desktop**

    For the changes to take effect, you must stop and restart Rancher Desktop.
