---
title: Mirroring Private Registries
---

import TabsConstants from '@site/core/TabsConstants';

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/mirror-private-registry"/>
</head>

Rancher Desktop can be configured to use a private container registry by using a provisioning script to update the `registries.yaml` file used by k3s. This allows you to mirror a private registry, making it easier to access and manage your container images.

For more information on private registry configuration, please refer to the [k3s documentation](https://docs.k3s.io/installation/private-registry).

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="macOS / Linux">

On macOS and Linux, you can use a provisioning script to configure your private registry.

1.  **Create an `override.yaml` File**

    You will need to create an `override.yaml` file in the lima configuration directory. Please note that you must run Rancher Desktop at least once for this directory to be created.

    -   **macOS:** `~/Library/Application Support/rancher-desktop/lima/_config/override.yaml`
    -   **Linux:** `~/.local/share/rancher-desktop/lima/_config/override.yaml`

2.  **Add the Provisioning Script**

    Add the following content to your `override.yaml` file, replacing `<my.private.registry>` with the URL of your private registry:

    ```yaml
    provision:
      - mode: system
        script: |
          #!/bin/sh
          set -eux
          mkdir -p /etc/rancher/k3s
          cat <<EOF >/etc/rancher/k3s/registries.yaml
          mirrors:
            "<my.private.registry>:5000":
              endpoint:
              - http://<my.private.registry>:5000
          EOF
    ```

3.  **Restart and Verify**

    Restart Rancher Desktop for the changes to take effect. You can then verify that the configuration has been applied by running the following command:

    ```bash
    rdctl shell cat /etc/rancher/k3s/registries.yaml
    ```

</TabItem>
<TabItem value="Windows">

On Windows, you can use a `.start` provisioning script to configure your private registry.

1.  **Create a Provisioning Script**

    First, ensure that you have run Rancher Desktop at least once to initialize the necessary configurations.

    Next, create a new file named `mirror-registry.start` in the following directory:

    -   **Path:** `%LOCALAPPDATA%\rancher-desktop\provisioning\mirror-registry.start`

    Add the following content to the file:

    ```sh
    #!/bin/sh
    set -eux
    mkdir -p /etc/rancher/k3s
    cat <<EOF >/etc/rancher/k3s/registries.yaml
    mirrors:
      "localhost:5000":
        endpoint:
          - http://localhost:5000
    EOF
    ```

2.  **Restart and Verify**

    Restart Rancher Desktop for the changes to take effect. You can then verify that the configuration has been applied by running the following command:

    ```shell
    rdctl shell cat /etc/rancher/k3s/registries.yaml
    ```

</TabItem>
</Tabs>
