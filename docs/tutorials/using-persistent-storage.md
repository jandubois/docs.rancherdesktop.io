---
title: Using Persistent Storage
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/tutorials/using-persistent-storage/"/>
</head>

By design, containers are ephemeral and stateless. However, in most real-world scenarios, you will need to persist the data produced or consumed by your containers. To address this, container engines provide two primary mechanisms for persistent storage: **bind mounts** and **volumes**.

This guide will walk you through the process of using both bind mounts and volumes in Rancher Desktop.

## Bind Mounts

A **bind mount** allows you to mount a file or directory from your host machine into a container. This is useful for sharing source code, configuration files, or other data between your host and your containers.

You can create a bind mount using either the `-v` or `--mount` flag with the `docker run` or `nerdctl run` command. The following examples demonstrate how to mount the `src` subdirectory of your current working directory into the `/app/src` directory of a container.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

**Using the `-v` flag:**
```bash
nerdctl run --rm -it -v $(pwd)/src:/app/src alpine:latest /bin/sh
```

**Using the `--mount` flag:**
```bash
nerdctl run --rm -it --mount=type=bind,source=$(pwd)/src,target=/app/src alpine:latest /bin/sh
```

</TabItem>
<TabItem value="docker">

**Using the `-v` flag:**
```bash
docker run --rm -it -v $(pwd)/src:/app/src alpine:latest /bin/sh
```

**Using the `--mount` flag:**
```bash
docker run --rm -it --mount=type=bind,source=$(pwd)/src,target=/app/src alpine:latest /bin/sh
```

</TabItem>
</Tabs>

:::note
In Command Prompt, replace `$(pwd)` with `%cd%`. In PowerShell, replace `$(pwd)` with `${pwd}`.
:::

Once the container is running, any changes you make to the `/app/src` directory inside the container will be reflected in the `src` directory on your host, and vice versa.

:::note
By default, Rancher Desktop only allows bind mounts to be created in the following directories: `/Users/$USER` on macOS, `/home/$USER` on Linux, and `/tmp/rancher-desktop` on both. On Windows, all files are automatically shared via WSL2. To mount other directories, you will need to use a [provisioning script](../how-to-guides/provisioning-scripts.md).
:::

## Volumes

A **volume** is another mechanism for persisting data in containers. Unlike bind mounts, volumes are managed by the container engine and are isolated from the host's file system. This makes them a good choice for storing application data that does not need to be directly accessed from the host.

### Step 1: Create a Volume

First, create a named volume.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl volume create my-persistent-data
```

</TabItem>
<TabItem value="docker">

```bash
docker volume create my-persistent-data
```

</TabItem>
</Tabs>

### Step 2: Mount the Volume

Now, you can mount the volume into a container using either the `-v` or `--mount` flag.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

**Using the `-v` flag:**
```bash
nerdctl run --rm -it -v my-persistent-data:/app/src alpine:latest /bin/sh
```

**Using the `--mount` flag:**
```bash
nerdctl run --rm -it --mount=type=volume,source=my-persistent-data,target=/app/src alpine:latest /bin/sh
```

</TabItem>
<TabItem value="docker">

**Using the `-v` flag:**
```bash
docker run --rm -it -v my-persistent-data:/app/src alpine:latest /bin/sh
```

**Using the `--mount` flag:**
```bash
docker run --rm -it --mount=type=volume,source=my-persistent-data,target=/app/src alpine:latest /bin/sh
```

</TabItem>
</Tabs>

Any data written to the `/app/src` directory inside the container will be stored in the `my-persistent-data` volume. The data will persist even if you stop or remove the container.
