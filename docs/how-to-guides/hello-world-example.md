---
title: Hello World Example
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/hello-world-example"/>
</head>

This tutorial provides a hands-on introduction to Rancher Desktop by walking you through two common use cases: building a container image and running it locally, and deploying a containerized application to a local Kubernetes cluster.

Rancher Desktop supports two container runtimes: [containerd](https://containerd.io/) and [Moby](https://mobyproject.org/) (dockerd). You can select your preferred runtime in the **Kubernetes Settings** panel. This guide provides commands for both `nerdctl` (for containerd) and the Docker CLI (for Moby/dockerd).

## Example 1: Build an Image and Run a Container

This example demonstrates how to build a simple "Hello World" container image and run it locally.

1.  **Create a Project Directory**

    ```bash
    mkdir hello-world
    cd hello-world
    ```

2.  **Create a Dockerfile**

    Create a new file named `Dockerfile` and add the following content:

    ```dockerfile
    FROM alpine
    CMD ["echo", "Hello World!!"]
    ```

3.  **Build and Run the Image**

    Use the following commands to build the image and run it in a container. After running the container, the commands will also show you how to list the image and then remove it.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
# Build the image
nerdctl build --tag helloworld:v1.0 .

# List the image
nerdctl images | grep helloworld

# Run the container
nerdctl run --rm helloworld:v1.0

# Remove the image
nerdctl rmi helloworld:v1.0
```

</TabItem>
<TabItem value="docker">

```bash
# Build the image
docker build --tag helloworld:v1.0 .

# List the image
docker images | grep helloworld

# Run the container
docker run --rm helloworld:v1.0

# Remove the image
docker rmi helloworld:v1.0
```

</TabItem>
</Tabs>

## Example 2: Deploy an Application to Kubernetes

This example demonstrates how to build a container image for a simple NGINX web server and deploy it to your local Kubernetes cluster.

1.  **Create a Project Directory and Sample HTML**

    ```bash
    mkdir nginx
    cd nginx
    echo "<h1>Hello World from NGINX!!</h1>" > index.html
    ```

2.  **Create a Dockerfile**

    Create a new file named `Dockerfile` and add the following content:

    ```dockerfile
    FROM nginx:alpine
    COPY ./index.html /usr/share/nginx/html
    ```

3.  **Build the Image**

    Use the following commands to build the image.

    :::caution Important for `nerdctl` users:
    When using `nerdctl`, you must include the `--namespace k8s.io` flag to make the image available to the Kubernetes cluster.
    :::

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl --namespace k8s.io build --tag nginx-helloworld:latest .
nerdctl --namespace k8s.io images | grep nginx-helloworld
```

</TabItem>
<TabItem value="docker">

```bash
docker build --tag nginx-helloworld:latest .
docker images | grep nginx-helloworld
```

</TabItem>
</Tabs>

4.  **Deploy to Kubernetes**

    Use the following commands to create a pod for the application and forward a local port to it.

    :::note
    When using an image with the `:latest` tag, you must set `--image-pull-policy=Never`. Otherwise, Kubernetes will try to pull the image from a remote repository instead of using your local image.
    :::

    ```bash
    kubectl run hello-world --image=nginx-helloworld:latest --image-pull-policy=Never --port=80
    kubectl port-forward pods/hello-world 8080:80
    ```

    You can now access your application by navigating to `http://localhost:8080` in your web browser or by running `curl localhost:8080` in your terminal.

5.  **Clean Up**

    Once you are finished, use the following commands to delete the pod and the container image.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
kubectl delete pod hello-world
nerdctl --namespace k8s.io rmi nginx-helloworld:latest
```

</TabItem>
<TabItem value="docker">

```bash
kubectl delete pod hello-world
docker rmi nginx-helloworld:latest
```

</TabItem>
</Tabs>
