---
title: FAQ
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import TabsConstants from '@site/core/TabsConstants';

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/faq"/>
</head>

This FAQ answers the questions our users most frequently ask about Rancher Desktop.

### Is Rancher Desktop a desktop version of Rancher?

No. Although Rancher and Rancher Desktop share the "Rancher" name, they serve different purposes. Rancher is a comprehensive platform for managing multiple Kubernetes clusters, while Rancher Desktop provides a local Kubernetes and container management setup for development. The two tools are designed to work well together.

### Is there a Kubernetes Cluster Explorer available in Rancher Desktop?

Yes. The Rancher Dashboard is included as a feature preview in release 1.2.1 and later. You can access it by clicking **Dashboard** in the system tray menu.

### Can I have Docker Desktop installed alongside Rancher Desktop?

Yes, but they cannot be run at the same time. Be sure to stop one before starting the other.

### How can I perform a clean uninstall of Rancher Desktop?

First, perform a [Factory Reset](ui/troubleshooting.md#factory-reset), and then uninstall the application. The uninstallation process varies by operating system. For more information, please refer to the [installation documentation](./getting-started/installation.md).

### What support is available for DNS over VPN on Windows?

An alternative DNS resolver for Windows has been implemented to address some of the VPN issues on Windows. It should support DNS lookup over VPN connections. It has to be enabled manually by editing an internal [configuration file](https://github.com/rancher-sandbox/rancher-desktop/issues/1899#issuecomment-1109128277).

### What does the "WSL Integration" tab do?

This tab allows you to make your Kubernetes configuration accessible to your WSL distributions so that you can use tools like `kubectl` to communicate with your cluster.

### Where can I find detailed logs?

In the main window, click on the **Troubleshooting** tab, then click **Show Logs**.

### How can I enable the dashboard for the Traefik Ingress controller?

For security reasons, the Traefik dashboard is not exposed by default. However, you can expose it in several ways.

**Using `port-forward`:**

```
kubectl port-forward -n kube-system $(kubectl -n kube-system get pods --selector "app.kubernetes.io/name=traefik" --output=name) 9000:9000
```

You can then access the dashboard at `http://127.0.0.1:9000/dashboard/`.

**Using `HelmChartConfig`:**

Create a file named `expose-traefik.yaml` with the following content:

```yaml
apiVersion: helm.cattle.io/v1
kind: HelmChartConfig
metadata:
  name: traefik
  namespace: kube-system
spec:
  valuesContent: |-
    dashboard:
      enabled: true
    ports:
      traefik:
        expose: true # Avoid this in production deployments
    logs:
      access:
        enabled: true
```

Then, apply the configuration:

```
kubectl apply -f expose-traefik.yaml
```

You can then access the dashboard at `http://127.0.0.1:9000/dashboard/`.

### Can I disable Traefik, and will doing so remove Traefik resources?

Yes, you can disable Traefik to free up ports 80 and 443 for an alternative Ingress configuration. To do so, uncheck the **Enable Traefik** option in the **Kubernetes Settings** panel. Disabling Traefik will not delete any existing resources.

If you want to delete the Traefik resources, you will need to reset Kubernetes.

### Is there support for internal container port forwarding?

Yes. This feature is available on Windows, Linux, and macOS as of v1.1.0.

### Does file sharing work similarly to Docker Desktop?

By default, the following directories are shared with the Rancher Desktop virtual machine:

-   **macOS:** `/Users/$USER`
-   **Linux:** `/home/$USER` and `/tmp/rancher-desktop`
-   **Windows:** All files are automatically shared via WSL2.

Standard Docker volumes that are not accessible from the host will work out of the box. To access other directories on macOS or Linux, or to change the behavior of the mounts, you will need to perform [additional configuration](https://github.com/rancher-sandbox/rancher-desktop/issues/1209#issuecomment-1370181132).

### Can containers reach back to host services via `host.docker.internal`?

Yes. On Windows, you may need to create a firewall rule to allow communication between the host and the container. You can run the following command in a privileged PowerShell session to create the rule:

```powershell
New-NetFirewallRule -DisplayName "WSL" -Direction Inbound -InterfaceAlias "vEthernet (WSL)" -Action Allow
```

### Can I map `host.docker.internal` to `host-gateway`?

No. The special value `host-gateway` is specific to Docker Desktop and is not yet supported in Rancher Desktop. However, you can access services running on the host from within a container using `host.docker.internal` or `host.rancher-desktop.internal` without passing the `--add-host` flag.

### How do I disable Kubernetes to save resources?

In the main window, navigate to **Preferences > Kubernetes** and uncheck the **Enable Kubernetes** option. This will allow you to run only the container runtime (`containerd` or `dockerd`) without allocating resources for Kubernetes.

### What happened to the Kubernetes Image Manager (kim)?

As of version 1.0, Kim is no longer shipped with Rancher Desktop. It has been replaced by `nerdctl` and the Docker CLI.

### Why is `brew install rancher` failing?

If you have already used Homebrew to install any of the utilities that are bundled with Rancher Desktop (e.g., `helm`, `kubectl`, `nerdctl`, `docker`), the `brew install rancher` command may fail. This is due to a conflict in the Homebrew cask formula. To avoid this issue, we recommend installing Rancher Desktop from the `.dmg` file.

### Why doesn't `nerdctl` from the Arch User Repository work?

For Rancher Desktop, `nerdctl` must run inside the virtual machine, not on the host. The version of `nerdctl` that is distributed with Rancher Desktop is a shell wrapper that executes the command inside the VM.

### How do I fix the "Insufficient permission to manipulate /usr/local/bin" error?

This error occurs when you do not have ownership of the `/usr/local/bin` directory. As a temporary workaround, you can change the ownership of the directory by running `sudo chown $USER /usr/local/bin`.

As of version 1.3.0, Rancher Desktop no longer creates symbolic links in `/usr/local/bin`. Instead, it creates them in `~/.rd/bin` and adds that directory to your `PATH`. We strongly recommend upgrading to the latest version of Rancher Desktop to avoid this issue.

### Is Cygwin compatible with Rancher Desktop?

No, but there are plans to add compatibility in the future.

### Where does Rancher Desktop store data volumes?

-   **Windows:**
    -   `dockerd (moby):` `\\wsl$\rancher-desktop-data\var\lib\docker\volumes`
    -   `containerd:` `\\wsl$\rancher-desktop-data\var\lib\nerdctl\dbb19c5e\volumes\<namespace>`
-   **macOS & Linux:**
    -   `dockerd (moby):` `/var/lib/docker/volumes`
    -   `containerd:` `/var/lib/nerdctl/dbb19c5e/volumes/<namespace>`

    You can access these paths in the virtual machine by running `rdctl shell`.

### How can I downgrade Rancher Desktop?

We strongly recommend using the current release of Rancher Desktop, as it includes the latest features and bug fixes. However, if you need to downgrade, please follow these steps:

1.  Perform a `Troubleshooting > Factory Reset`. Ensure that the **Keep cached Kubernetes images** box is not checked.
2.  On Windows, shut down WSL by running `wsl --shutdown` in a PowerShell session.
3.  Uninstall the current version of Rancher Desktop.
4.  Install the older version.

### How do I fix an unresponsive session after hibernation on Windows?

This is due to a known WSL [bug](https://github.com/microsoft/WSL/issues/8696) that can cause it to become unresponsive after hibernation. To resolve this without rebooting your machine, you can try the following steps:

1.  Shut down WSL by running `wsl --shutdown`.
2.  If the command is successful, skip to step 3. If not, you will need to manually stop and restart the `LxssManager` service. You can do this from the Services application or by running the following commands in a PowerShell session:
    ```powershell
    stop-service lxssmanager
    start-service lxssmanager
    ```
3.  Exit and restart Rancher Desktop.

### What is `rancher-desktop-data`?

`rancher-desktop-data` is a persistent volume that is used for storage (e.g., container images). It is mounted inside the `rancher-desktop` distribution and will not run by itself. This is an implementation detail and can be ignored.
