---
title: Working with Containers
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/tutorials/working-with-containers"/>
</head>

This tutorial provides a quick reference for common commands to build, run, and manage containers in Rancher Desktop. You can use either the `nerdctl` command-line interface (with the `containerd` runtime) or the Docker CLI (with the `moby` runtime).

## Running a Container

To run a container with the default `bridge` CNI network (10.4.0.0/24):

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl run -it --rm alpine
```

</TabItem>
<TabItem value="docker">

```bash
docker run -it --rm alpine
```

</TabItem>
</Tabs>

## Building an Image

To build an image using BuildKit:

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl build -t foo /some-dockerfile-directory
nerdctl run -it --rm foo
```

</TabItem>
<TabItem value="docker">

```bash
docker build -t foo /some-dockerfile-directory
docker run -it --rm foo
```

</TabItem>
</Tabs>

To build an image and send the output to a local directory:

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl build -o type=local,dest=. /some-dockerfile-directory
```

</TabItem>
<TabItem value="docker">

```bash
docker build -o type=local,dest=. /some-dockerfile-directory
```

</TabItem>
</Tabs>

## Using Docker Compose

Docker Compose is a tool for defining and running multi-container applications.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

The `nerdctl-compose` CLI is designed to be compatible with `docker-compose`.

```bash
nerdctl compose up -d
nerdctl compose down
```

</TabItem>
<TabItem value="docker">

The `compose` command in the Docker CLI is a drop-in replacement for `docker-compose`.

```bash
docker compose up -d
docker compose down
```

</TabItem>
</Tabs>

## Exposing a Port

To expose port 8000 for a container:

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl run -d -p 8000:80 nginx
```

</TabItem>
<TabItem value="docker">

```bash
docker run -d -p 8000:80 nginx
```

</TabItem>
</Tabs>

You can now access the container at `http://localhost:8000`.

:::note
By default, exposed ports are only accessible on the localhost network interface on Windows. To expose a port to other network interfaces, you can [configure a `portproxy`](https://github.com/rancher-sandbox/rancher-desktop/issues/1180#issuecomment-1005514200) on the Windows host.
:::

### Exposing a Port on a Running Container

If you forgot to expose a port when you started a container, you can use a proxy container to forward traffic to the original container. This avoids the need to restart the container, which is particularly useful for services with long startup times.

1.  **Get the Container IP Address:**
    First, get the IP address of the container you want to expose.

    <Tabs groupId="container-runtime">
    <TabItem value="nerdctl" default>

    ```bash
    CONTAINER_IP=$(nerdctl inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container-name-or-id>)
    ```

    </TabItem>
    <TabItem value="docker">

    ```bash
    CONTAINER_IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container-name-or-id>)
    ```

    </TabItem>
    </Tabs>

2.  **Start the Proxy Container:**
    Next, start a proxy container that forwards traffic from a host port to the container port.

    <Tabs groupId="container-runtime">
    <TabItem value="nerdctl" default>

    ```bash
    nerdctl run --rm -p <host-port>:<container-port> alpine/socat TCP-LISTEN:<container-port>,fork TCP-CONNECT:${CONTAINER_IP}:${CONTAINER_PORT}
    ```

    </TabItem>
    <TabItem value="docker">

    ```bash
    docker run --rm -p <host-port>:<container-port> alpine/socat TCP-LISTEN:<container-port>,fork TCP-CONNECT:${CONTAINER_IP}:${CONTAINER_PORT}
    ```

    </TabItem>
    </Tabs>

You can now access the container from your host machine at `localhost:<host-port>`.

## Targeting a Kubernetes Namespace (nerdctl only)

When using `nerdctl`, you can target a specific Kubernetes namespace with the `--namespace` flag. This feature is not available in the Docker CLI.

```bash
nerdctl --namespace k8s.io build -t demo:latest /path/to/dockerfile/directory
nerdctl --namespace k8s.io ps
```
