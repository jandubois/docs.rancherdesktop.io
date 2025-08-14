---
title: Installation
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/getting-started/installation"/>
</head>

Rancher Desktop is available as a desktop application for Windows, macOS, and Linux. You can download it from the [releases page on GitHub](https://github.com/rancher-sandbox/rancher-desktop/releases).

When you run Rancher Desktop for the first time, or when you switch to a new version, it will download the necessary Kubernetes container images. This process may take a few moments, so please be patient during the initial startup.

Once installed, Rancher Desktop provides access to several essential command-line utilities:

- **Helm:** The package manager for Kubernetes.
- **kubectl:** The command-line tool for interacting with Kubernetes clusters.
- **nerdctl:** A Docker-compatible CLI for containerd.
- **Moby:** The open-source container framework developed by Docker.
- **Docker Compose:** A tool for defining and running multi-container Docker applications.

## macOS

### Requirements

To install Rancher Desktop on macOS, you will need:

- **macOS:** Ventura (13) or newer.
- **CPU:** Apple Silicon (aarch64) or Intel (x86_64) with VT-x support.
- **Internet:** A persistent internet connection.

For optimal performance, we recommend:

- **Memory:** 8 GB or more.
- **CPU:** At least 4 cores.

Please note that you may need additional resources depending on the workloads you intend to run.

### Installing on macOS

1. Visit the [releases page] on GitHub.
2. Locate the version of Rancher Desktop you wish to download.
3. In the **Assets** section, download the file named `Rancher.Desktop-X.Y.Z.dmg`, where `X.Y.Z` corresponds to the release version.
4. Open the downloaded `.dmg` file from your `Downloads` folder.
5. In the window that appears, drag the Rancher Desktop icon into your **Applications** folder.
6. Launch Rancher Desktop from your **Applications** folder.

[releases page]: https://github.com/rancher-sandbox/rancher-desktop/releases

### Uninstalling on macOS

1. Open **Finder** and navigate to your **Applications** folder.
2. Locate the Rancher Desktop application.
3. Drag the application to the **Trash**, or right-click and select **Move to Trash**.
4. To permanently remove the application, right-click the **Trash** icon in your dock and select **Empty Trash**.

## Windows

### Requirements

To install Rancher Desktop on Windows, you will need:

- **Windows:**
  - Windows 10 (Home edition is supported).
  - Windows 11 (Home edition is supported).
  - Windows Server 2022.
- **Virtualization:** Your machine must have virtualization capabilities enabled.
- **WSL:** [Windows Subsystem for Linux](https://aka.ms/wslinstall) must be installed.
- **Internet:** A persistent internet connection.

For optimal performance, we recommend:

- **Memory:** 8 GB or more.
- **CPU:** At least 4 cores.

Please note that you may need additional resources depending on the workloads you intend to run.

**Note on User Privileges:** While you can use Rancher Desktop as a non-admin user, administrator privileges are required to install the **Rancher Desktop Privileged Service**. This service is necessary for exposing applications and services running in containers to all network interfaces on your host machine. If you choose not to install this service, you will be limited to exposing services on `127.0.0.1`.

### Installing on Windows

1. Visit the [releases page] on GitHub.
2. Locate the version of Rancher Desktop you wish to download.
3. In the **Assets** section, download the Windows installer, `Rancher.Desktop.Setup.X.Y.Z.msi`, where `X.Y.Z` corresponds to the release version.
4. Run the installer from your `Downloads` folder.
5. Review the license agreement and click **I Agree** to continue.
6. If prompted, choose whether to install for all users on the machine or only for the current user. Installing for all users is recommended to enable the Rancher Desktop Privileged Service.
7. Follow the on-screen prompts to complete the installation.
8. Once the installation is finished, click **Finish**.

[release page]: https://github.com/rancher-sandbox/rancher-desktop/releases

### Uninstalling on Windows

1. Open the **Start** menu and go to **Settings > Apps > Apps & features**.
2. Locate the Rancher Desktop entry in the list.
3. Select it and click **Uninstall**. Confirm your choice when prompted.
4. Follow the steps in the uninstaller wizard to remove Rancher Desktop.
5. Click **Finish** to complete the process.

## Linux

### Requirements

To install Rancher Desktop on Linux, you will need:

- **Distribution:** A distribution that supports `.deb`, `.rpm`, or AppImage packages.
- **CPU:** An x86_64 processor with AMD-V or VT-x support.
- **Permissions:** Read and write access to `/dev/kvm`.
- **Internet:** A persistent internet connection.

For optimal performance, we recommend:

- **Memory:** 8 GB or more.
- **CPU:** At least 4 cores.

Please note that you may need additional resources depending on the workloads you intend to run.

:::note
Some Linux distributions that use GNOME, such as Ubuntu and Fedora, do not have out-of-the-box support for a system tray. As a result, the Rancher Desktop tray icon may not be displayed in these environments.
:::

### Linux Setup

Before installing Rancher Desktop, there are a few one-time setup steps you may need to perform.

#### Access to `/dev/kvm`

On some distributions, such as Ubuntu 18.04, users may not have sufficient privileges to use `/dev/kvm`, which is required by Rancher Desktop. To check if you have the necessary permissions, run the following command:

```
[ -r /dev/kvm ] && [ -w /dev/kvm ] || echo 'Insufficient privileges'
```

If the output is `Insufficient privileges`, you will need to add your user to the `kvm` group:

```
sudo usermod -a -G kvm "$USER"
```

After running this command, you must reboot your system for the changes to take effect.

#### `pass` for Credential Management

Rancher Desktop uses `pass` to securely store credentials for `docker login` and `nerdctl login`. If you plan to use these commands, you will need to set up `pass`. If this is your first time using `pass`, you will need to generate a GPG key:

```
gpg --generate-key
```

This command will output a key ID (e.g., `8D818FB37A9279E341F01506ED96AD27A40C9C73`). Use this ID to initialize `pass`:

```
pass init <your-key-id>
```

For more information, please refer to the [official `pass` website][its website].

[its website]: https://www.passwordstore.org/

#### Traefik Port Binding

Rancher Desktop uses Traefik as its default Ingress controller. On many Linux distributions, non-root users are not permitted to listen on TCP and UDP ports below 1024. This can cause a "permission denied" error for Traefik. To resolve this, you can run the following command to allow processes to bind to ports 80 and above:

```
sudo sysctl -w net.ipv4.ip_unprivileged_port_start=80
```

To make this change persistent across reboots, add the command to your `/etc/sysctl.conf` file.

### Installing and Uninstalling on Linux

You can install Rancher Desktop on Linux using a `.deb` package, an `.rpm` package, or an AppImage.

#### .deb Package (Debian, Ubuntu)

**Installation:**

```
curl -s https://download.opensuse.org/repositories/isv:/Rancher:/stable/deb/Release.key | gpg --dearmor | sudo dd status=none of=/usr/share/keyrings/isv-rancher-stable-archive-keyring.gpg
echo 'deb [signed-by=/usr/share/keyrings/isv-rancher-stable-archive-keyring.gpg] https://download.opensuse.org/repositories/isv:/Rancher:/stable/deb/ ./' | sudo dd status=none of=/etc/apt/sources.list.d/isv-rancher-stable.list
sudo apt update
sudo apt install rancher-desktop
```

**Uninstallation:**

```
sudo apt remove --autoremove rancher-desktop
sudo rm /etc/apt/sources.list.d/isv-rancher-stable.list
sudo rm /usr/share/keyrings/isv-rancher-stable-archive-keyring.gpg
sudo apt update
```

#### .rpm Package (Fedora, openSUSE)

**Installation on openSUSE:**

```
sudo zypper addrepo https://download.opensuse.org/repositories/isv:/Rancher:/stable/rpm/isv:Rancher:stable.repo
sudo zypper install rancher-desktop
```

**Installation on Fedora:**

```bash
sudo dnf config-manager --add-repo https://download.opensuse.org/repositories/isv:/Rancher:/stable/fedora/isv:Rancher:stable.repo
sudo dnf install rancher-desktop
```

:::note
The Fedora packages are not tested on RHEL or related distributions. We recommend using the AppImage for these systems.
:::

**Uninstallation on openSUSE:**

```
sudo zypper remove --clean-deps rancher-desktop
sudo zypper removerepo isv_Rancher_stable
```

**Uninstallation on Fedora:**

```
sudo dnf remove rancher-desktop
sudo rm '/etc/yum.repos.d/isv:Rancher:stable.repo'
```

#### AppImage

**Installation:**

First, ensure that `pass` and `gpg` are installed on your system. For example, on Rocky Linux:

```
# Enable EPEL if not already enabled.
dnf install pass gnupg2
```

Download the AppImage from the [releases page]. Make it executable, and then run it. For better desktop integration, consider using [AppImageLauncher].

[here]: https://download.opensuse.org/repositories/isv:/Rancher:/stable/AppImage/rancher-desktop-latest-x86_64.AppImage
[AppImageLauncher]: https://github.com/TheAssassin/AppImageLauncher

**Uninstallation:**

To uninstall, simply delete the AppImage file.

## Proxy Environments

If you are in a secure or locked-down environment, you may need to allow certain URLs through your proxy for Rancher Desktop to function correctly. Here is a list of essential URL patterns:

- **K3s Releases:**
  - `https://api.github.com/repos/k3s-io/k3s/releases` (for available versions)
  - `https://github.com/k3s-io/k3s/releases/download` (for downloading releases)
- **kubectl Releases:** `https://storage.googleapis.com/kubernetes-release/release` (for `kuberlr`)
- **Upgrades:** `https://desktop.version.rancher.io/v1/checkupgrade`
- **Documentation:** `https://docs.rancherdesktop.io` (for in-app help and online status checks)

Please note that this list is specific to Rancher Desktop and may not cover all dependencies for the broader Rancher platform.
