---
title: Provisioning Scripts
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/provisioning-scripts"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

Provisioning scripts allow you to customize the Rancher Desktop environment by overriding internal processes. You can use them to pass command-line arguments to K3s, add extra mounts, increase resource limits, and more. This guide explains how to set up provisioning scripts on macOS, Linux, and Windows.

## macOS & Linux

On macOS and Linux, you can use an `override.yaml` file to define your provisioning scripts and other lima configurations.

1.  **Create the `override.yaml` File**

    First, run Rancher Desktop at least once to ensure the necessary configuration directories are created.

    :::note
    The configuration directory is deleted during a factory reset. Be sure to back up your `override.yaml` file if you wish to restore it after a reset.
    :::

    Create an empty `override.yaml` file in the following location:

    <Tabs groupId="os">
    <TabItem value="macOS">

    ```
    ~/Library/Application\ Support/rancher-desktop/lima/_config/override.yaml
    ```

    </TabItem>
    <TabItem value="Linux">

    ```
    ~/.local/share/rancher-desktop/lima/_config/override.yaml
    ```

    </TabItem>
    </Tabs>

2.  **Add Your Configurations**

    You can add various configurations to the `override.yaml` file. The following example demonstrates how to increase the `ulimit` for containers, add an additional writable mount, and pass a startup flag to the K3s server.

    ```yaml
    # Increase ulimit for containers
    provision:
    - mode: system
      script: |
        #!/bin/sh
        cat <<'EOF' > /etc/security/limits.d/rancher-desktop.conf
        * soft     nofile         82920
        * hard     nofile         82920
        EOF

    # Add an additional writable mount
    mounts:
      - location: /some/path
        writable: true

    # Add a TLS SAN to the K3s server certificate
    env:
      K3S_EXEC: --tls-san value
    ```

    For more information on K3s environment variables and flags, please refer to the [K3s documentation](https://docs.k3s.io/reference/env-variables).

## Windows

On Windows, you can use `.start` and `.stop` files to run provisioning scripts at different points in the Rancher Desktop lifecycle.

1.  **Locate the `provisioning` Directory**

    First, run Rancher Desktop at least once to ensure the necessary configuration directories are created.

    :::note
    The `provisioning` directory is deleted during a factory reset. Be sure to back up your scripts if you wish to restore them after a reset.
    :::

    The provisioning directory is located at `%LOCALAPPDATA%\rancher-desktop\provisioning`. For example: `C:\Users\Joe\AppData\Local\rancher-desktop\provisioning`.

2.  **Create `.start` and `.stop` Scripts**

    You can create two types of scripts in the `provisioning` directory:

    -   **`.start` scripts:** These scripts are executed after Rancher Desktop's internal setup is complete but before the container runtime and Kubernetes are started.
    -   **`.stop` scripts:** These scripts are executed after the container runtime and Kubernetes have been shut down.

    For example, you can create a file named `insecure-registry.start` to configure an insecure registry for `nerdctl`:

    ```sh
    #!/bin/sh
    mkdir -p /etc/nerdctl
    cat >  /etc/nerdctl/nerdctl.toml <<EOF
    insecure_registry = true
    EOF
    ```

    :::important
    Scripts on Windows must be saved with Unix-style line endings (LF). Files with DOS-style line endings (CRLF) may produce unexpected results.
    :::

There are some limitations to what you can change with provisioning scripts. For example, you cannot change the hard `ulimit` on WSL2. If you have questions about specific use cases, please feel free to reach out to the Rancher Desktop community on [Slack](https://slack.rancher.io/) or [GitHub](https://github.com/rancher-sandbox/rancher-desktop/issues).
