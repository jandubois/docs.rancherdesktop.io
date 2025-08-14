---
title: Working with Images
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/tutorials/working-with-images"/>
</head>

This tutorial provides a quick reference for common commands to build, tag, and manage container images in Rancher Desktop. You can use either the `nerdctl` command-line interface (with the `containerd` runtime) or the Docker CLI (with the `moby` runtime).

:::note
Unlike Docker, `containerd` uses namespaces to isolate data. By default, `nerdctl` stores images in the `default` namespace. To make your images available to Kubernetes, you must use the `k8s.io` namespace by including the `--namespace k8s.io` or `-n k8s.io` flag in your commands. Please note that `nerdctl` namespaces are separate from Kubernetes namespaces.
:::

## Listing Images

To see the images that are currently available in your environment, run the following command:

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl images
```

</TabItem>
<TabItem value="docker">

```bash
docker images
```

</TabItem>
</Tabs>

## Building Images

To build an image from a `Dockerfile`, use the `build` command. You can also use the `-t` flag to tag the image at the same time.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
# Build an image
nerdctl build .

# Build and tag an image
nerdctl build -t my-image:my-tag .

# Build an image for use with Kubernetes
nerdctl build -n k8s.io -t my-image:my-tag .
```

</TabItem>
<TabItem value="docker">

```bash
# Build an image
docker build .

# Build and tag an image
docker build -t my-image:my-tag .
```

</TabItem>
</Tabs>

### Example: Building and Running a Local Image

This example demonstrates how to build a local image and run it as a container in Kubernetes.

1.  **Get the Sample Application:**
    Clone the Rancher Desktop documentation repository, which contains a sample Node.js application.

    ```bash
    git clone https://github.com/rancher-sandbox/docs.rancherdesktop.io.git
    cd docs.rancherdesktop.io/assets/express-sample
    ```

2.  **Build the Image:**

    <Tabs groupId="container-runtime">
    <TabItem value="nerdctl" default>

    ```bash
    nerdctl --namespace k8s.io build -t expressapp:v1.0 .
    ```

    </TabItem>
    <TabItem value="docker">

    ```bash
    docker build -t expressapp:v1.0 .
    ```

    </TabItem>
    </Tabs>

3.  **Run the Container:**

    ```bash
    kubectl run --image expressapp:v1.0 expressapp
    kubectl port-forward pods/expressapp 3000:3000
    ```

    :::note
    When using an image with the `:latest` tag, you must set `imagePullPolicy: Never` in your pod specification. Otherwise, Kubernetes will try to pull the image from a remote repository instead of using your local image.
    :::

## Tagging Images

To add a new tag to an existing image, use the `tag` command.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

</TabItem>
<TabItem value="docker">

```bash
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

</TabItem>
</Tabs>

## Removing Images

To remove an image, use the `rmi` command.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl rmi IMAGE
```

</TabItem>
<TabItem value="docker">

```bash
docker rmi IMAGE
```

</TabItem>
</Tabs>