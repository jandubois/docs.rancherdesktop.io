---
title: Running in an Air-Gapped Environment
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/running-air-gapped"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

Rancher Desktop can be run in an air-gapped environment, where there is no access to the internet. This guide explains the requirements and procedures for running Rancher Desktop offline.

:::note
This guide uses PowerShell syntax for environment variables (e.g., `$env:FOO`). If you are using the Command Prompt, please use the syntax `%FOO%` instead.
:::

## How Rancher Desktop Handles Offline Environments

Rancher Desktop requires network access for two main functions:

1.  **Pulling Kubernetes Images:** When you select a new version of Kubernetes, Rancher Desktop pulls the necessary k3s container images and stores them in a local cache.
2.  **Managing `kubectl` Versions:** Rancher Desktop uses a wrapper called `kuberlr` to manage `kubectl` versions. This ensures that the `kubectl` client version is always compatible with the Kubernetes server version.

When running in an air-gapped environment, you will need to manually populate the cache with the required Kubernetes images and `kubectl` executables.

## Option 1: Preparing an Existing Installation for Offline Use

If you have already installed and run Rancher Desktop on a machine with internet access, you can prepare it for offline use by pre-populating the necessary caches.

The key is to ensure that for every version of Kubernetes you intend to use offline, the corresponding `kubectl` executable has been downloaded.

For example, if you have versions `1.29.7`, `1.24.3`, and `1.21.14` of k3s in your cache, but you have only ever used `kubectl` with versions `1.24.3` and `1.21.14`, the `kuberlr` directory will only contain the `kubectl` executables for those two versions. If you then go offline and switch to Kubernetes `1.29.7`, `kubectl` commands will fail because `kuberlr` will be unable to download the required executable.

To prevent this, you should perform the following steps while you still have an internet connection:

1.  Open the Rancher Desktop **Kubernetes Settings** panel.
2.  For each version of Kubernetes that you plan to use offline, select it from the dropdown menu and wait for the new version to become active.
3.  Run a `kubectl` command, such as `kubectl cluster-info`, to trigger the download of the corresponding `kubectl` executable.

By doing this, you will ensure that all the necessary `kubectl` versions are cached and available for offline use.

## Option 2: Preparing a New Air-Gapped Installation

To set up Rancher Desktop on a machine that has never had internet access, you will need to use a machine with internet access to download the necessary files and then transfer them to the air-gapped machine using removable media.

### Step 1: Populate the `k3s` Cache

On the machine with internet access, create a directory to store the k3s files. For each version of Kubernetes you want to use offline, you will need to download the following files from the [k3s releases page](https://github.com/k3s-io/k3s/releases):

-   The k3s air-gap container images (`k3s-airgap-images-amd64.tar` or `k3s-airgap-images-arm64.tar`).
-   The `k3s` executable.
-   The `sha256sum-amd64.txt` or `sha256sum-arm64.txt` file.

You will also need the `k3s-versions.json` file from the [Rancher Desktop repository](https://raw.githubusercontent.com/rancher-sandbox/rancher-desktop/main/resources/k3s-versions.json).

Here is an example of the commands to download the necessary files for k3s v1.24.3:

```bash
# Create a directory for the k3s files
mkdir -p <source-disk>/k3s/v1.24.3+k3s1
cd <source-disk>/k3s/v1.24.3+k3s1

# Download the files
wget https://github.com/k3s-io/k3s/releases/download/v1.24.3%2Bk3s1/k3s-airgap-images-amd64.tar
wget https://github.com/k3s-io/k3s/releases/download/v1.24.3%2Bk3s1/sha256sum-amd64.txt
wget https://github.com/k3s-io/k3s/releases/download/v1.24.3%2Bk3s1/k3s

# Download the k3s versions file
cd ..
wget https://raw.githubusercontent.com/rancher-sandbox/rancher-desktop/main/resources/k3s-versions.json
```

Once you have downloaded all the necessary files, transfer them to the air-gapped machine. Then, on the air-gapped machine, copy the files to the Rancher Desktop cache directory.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

**Cache Location:** `$env:HOMEDRIVE\$env:HOMEPATH\AppData\Local\rancher-desktop\cache`

**Commands:**
```powershell
# Create the cache directory
mkdir -Force $env:HOMEDRIVE\$env:HOMEPATH\AppData\Local\rancher-desktop\cache\k3s

# Copy the files
copy-item -Force <source-disk>\k3s\k3s-versions.json $env:HOMEDRIVE\$env:HOMEPATH\AppData\Local\rancher-desktop\cache\
copy-item -Recurse -Force <source-disk>\k3s\v* $env:HOMEDRIVE\$env:HOMEPATH\AppData\Local\rancher-desktop\cache\k3s\
```

</TabItem>
<TabItem value="macOS">

**Cache Location:** `~/Library/Caches/rancher-desktop`

**Commands:**
```bash
CACHEDIR=~/Library/Caches/rancher-desktop
mkdir -p $CACHEDIR/k3s
cp <source-disk>/k3s/k3s-versions.json $CACHEDIR/
cp -r <source-disk>/k3s/v* $CACHEDIR/k3s/
```

</TabItem>
<TabItem value="Linux">

**Cache Location:** `~/.cache/rancher-desktop`

**Commands:**
```bash
CACHEDIR=~/.cache/rancher-desktop
mkdir -p $CACHEDIR/k3s
cp <source-disk>/k3s/k3s-versions.json $CACHEDIR/
cp -r <source-disk>/k3s/v* $CACHEDIR/k3s/
```

</TabItem>
</Tabs>

### Step 2: Populate the `kuberlr` Directory

Next, you will need to populate the `kuberlr` directory with the `kubectl` executables for each Kubernetes version you plan to use.

The `kuberlr` directory is located at `HOME/.kuberlr/PLATFORM-ARCH`, where:

-   `HOME` is your home directory.
-   `PLATFORM` is `windows`, `linux`, or `darwin`.
-   `ARCH` is `amd64` or `aarch64`.

On the machine with internet access, download the required `kubectl` executables from `https://dl.k8s.io/VERSION/bin/PLATFORM/ARCH/kubectl`. For example, to download `kubectl` v1.22.1 for Windows, you would use the following URL: `https://dl.k8s.io/v1.22.1/bin/windows/amd64/kubectl.exe`.

After downloading the executables, rename them to `kubectl<VERSION>` (e.g., `kubectl1.22.1.exe`) and copy them to the `kuberlr` directory on the air-gapped machine.

:::note
A `kubectl` client is guaranteed to work with a Kubernetes server that is running the same major version and is within one minor version (plus or minus). For example, if you are using Kubernetes versions `v1.21.x`, `v1.22.x`, and `v1.23.x`, you only need to install a `v1.22.x` version of `kubectl`.
:::
