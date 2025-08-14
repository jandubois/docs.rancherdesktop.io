---
title: Troubleshooting Tips
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/troubleshooting-tips"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

This page provides tips to help you troubleshoot common issues with Rancher Desktop.

### API

#### Rancher Desktop is stuck on "Waiting for Kubernetes API." What should I do?

The cause of this issue can be difficult to determine without more information. Please navigate to the **Troubleshooting** tab, click the **Show Logs** button to access the log files, and then file an issue on [GitHub](https://github.com/rancher-sandbox/rancher-desktop/issues) with the logs attached.

### Containers

#### How do I fix the Docker error when starting a container with the VS Code Dev Containers extension?

There is a known issue with the Dev Containers extension (v0.266 and later) and Rancher Desktop v1.8.1. As a workaround, you can disable Wayland support in the VS Code settings.

In **Settings > Extensions > Dev Containers**, uncheck the **Dev > Containers: Mount Wayland Socket (Applies to All Profiles)** option.

#### How do I fix the "subnet overlaps" error when running a container with `nerdctl`?

If you see the error `FATA[0005] subnet 10.4.0.0/24 overlaps with other one on this address space`, it is because there is a route rule with a conflicting IP address in your `iptables`.

As a quick workaround, you can shut down WSL by running `wsl --shutdown`.

:::caution
Shutting down WSL will stop all of your distributions, not just the `rancher-desktop` distribution.
:::

### Installation

#### Why is `brew install rancher-desktop` failing?

Due to Homebrew's cask naming conventions, the `-desktop` suffix is dropped from the cask formula name. To install Rancher Desktop with Homebrew, use the command `brew install rancher`.

#### How do I fix the "kubectl: command not found" issue on Linux?

By default, Rancher Desktop creates symbolic links for its command-line utilities in the `~/.local/bin` directory. To use these utilities from the command line, you will need to add this directory to your `PATH`.

You can do this by running the following command and then logging out and back in:

```bash
echo "export PATH=\$PATH:/home/$(whoami)/.local/bin" >> ~/.bashrc
```

#### How do I fix the "Installation Aborted" error when downgrading on Windows?

When downgrading from an MSI installation to an EXE installation (v1.6.x or earlier), you may encounter an "Installation Aborted" error. This can happen if a specific Windows registry key is not deleted during the uninstallation process.

To fix this, you will need to manually delete the registry key by running the following command in a privileged shell:

```powershell
reg.exe delete HKLM\System\CurrentControlSet\Services\EventLog\Application\RancherDesktopPrivilegedService /reg:64 /f
```

#### Why can't I run `docker compose` after uninstalling Docker Desktop?

This was a known issue in versions of Rancher Desktop prior to 1.1.0. As of v1.1.0, Rancher Desktop bundles `docker-compose` and makes the CLI plugins available in the `~/.docker/cli-plugins` directory. We strongly recommend using the latest version of Rancher Desktop to avoid this issue.

If you are on the latest version and still cannot run `docker compose`, please file an issue on [GitHub](https://github.com/rancher-sandbox/rancher-desktop/issues).

#### Why don't I see an entry for Rancher Desktop in my kubeconfig?

Rancher Desktop places its configuration in the default `~/.kube/config` file. If you do not see an entry for Rancher Desktop, it is likely that your `KUBECONFIG` environment variable is set to look for a configuration file in a different location.

### Networking

#### Why do I see a blank screen when I launch the Cluster Dashboard?

The Cluster Dashboard may not be running correctly because another process on your machine is using port `9080` or `9443`, which the dashboard depends on. To resolve this, you will need to identify and terminate the conflicting process.

You can use the following commands to find the process that is using a specific port:

<Tabs groupId="os">
<TabItem value="Windows">

```powershell
netstat -ano | findstr :9443
```

</TabItem>
<TabItem value="macOS">

```bash
lsof -nP -iTCP -sTCP:LISTEN | grep 9443
```

</TabItem>
<TabItem value="Linux">

```bash
lsof -nP -iTCP -sTCP:LISTEN | grep 9443
```

</TabItem>
</Tabs>

> **Note:** On macOS and Linux, the Rancher Dashboard process is named `steve`. On Windows, it is `steve.exe`. If `steve` is the only process using these ports, do not terminate it.

### WSL

#### Why isn't my WSL distribution listed on the WSL Integration page?

Rancher Desktop only supports WSL 2 distributions. If your distribution is not listed, it is likely a WSL 1 distribution. You can convert it to WSL 2 by running the following command:

```powershell
wsl --set-version <distro-name> 2
```

You can also set WSL 2 as the default for all future distributions by running `wsl --set-default-version 2`.

#### How do I fix the "permission denied" errors when using Docker in WSL?

If you are encountering "permission denied" errors, it is likely because you do not have write permissions for the Docker socket. To resolve this, you can run the following commands from within your WSL distribution:

```bash
sudo groupadd docker
sudo adduser $USER docker
sudo chown root:docker /var/run/docker.sock
sudo chmod g+w /var/run/docker.sock
newgrp docker
```
