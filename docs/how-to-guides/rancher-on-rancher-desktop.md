---
title: Rancher on Rancher Desktop
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/rancher-on-rancher-desktop"/>
</head>

Although [Rancher](https://rancher.com/) and [Rancher Desktop](https://rancherdesktop.io/) share the "Rancher" name, they serve different purposes. Rancher is a comprehensive platform for managing multiple Kubernetes clusters, while Rancher Desktop provides a local Kubernetes and container management setup. The two tools complement each other, and you can install Rancher as a workload in Rancher Desktop to explore its features.

This guide outlines two methods for installing the Rancher dashboard on Rancher Desktop: using a container runtime or using Helm.

:::note
You may encounter issues if your supporting utilities (e.g., Helm) or workload versions are incompatible with your current Kubernetes version. If this occurs, you can switch to a compatible Kubernetes version in **Preferences > Kubernetes**. For a list of supported platforms for Rancher, please refer to the [Rancher Support Matrix](https://www.suse.com/suse-rancher/support-matrix/all-supported-versions/).
:::

## Method 1: Using a Container Runtime

You can install Rancher using a single command with either `nerdctl` or `docker`.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```console
nerdctl run --privileged -d --restart=always -p 8080:80 -p 8443:443 rancher/rancher
```

</TabItem>
<TabItem value="docker">

```console
docker run --privileged -d --restart=always -p 8080:80 -p 8443:443 rancher/rancher
```

</TabItem>
</Tabs>

Once the installation is complete, you can access the Rancher UI at `https://localhost:8443`.

To log in, you will need to retrieve the bootstrap password:

1.  **Get the Container ID:**
    Use `nerdctl ps` or `docker ps` to find the container ID for the `rancher/rancher` image.

2.  **Get the Password:**
    Run the following command, replacing `[rancherContainerID]` with the ID from the previous step:

    ```bash
    # For nerdctl
    nerdctl logs [rancherContainerID] 2>&1 | grep "Bootstrap Password:"

    # For docker
    docker logs [rancherContainerID] 2>&1 | grep "Bootstrap Password:"
    ```

You can now use this password to log in to the Rancher dashboard.

## Method 2: Using Helm

You can also install Rancher using the Helm package manager.

1.  **Add the Jetstack Chart Repository:**
    ```console
    helm repo add jetstack https://charts.jetstack.io
    ```

2.  **Add the Rancher Chart Repository:**
    ```console
    helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
    ```

3.  **Create the `cert-manager` Namespace:**
    ```console
    kubectl create namespace cert-manager
    ```

4.  **Install `cert-manager`:**
    ```console
    helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.7.1 --set installCRDs=true
    ```

5.  **Apply CRDs:**
    ```console
    kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.7.1/cert-manager.crds.yaml
    ```

6.  **Create the `cattle-system` Namespace:**
    ```console
    kubectl create namespace cattle-system
    ```

7.  **Install Rancher:**
    ```console
    helm install rancher rancher-latest/rancher --namespace cattle-system --set hostname=rancher.rd.localhost --wait --timeout=10m
    ```

The installation may take a few minutes to complete. Once finished, you can access the Rancher UI at `https://rancher.rd.localhost`.

![](../img/examples/rancherUiWelcomePage.png)

Follow the setup wizard to configure Rancher and get started.

![](../img/examples/rancherUiMainPage.png)

From the Rancher dashboard, you can manage your local cluster, nodes, and more. For further information, please refer to the [Rancher documentation](https://ranchermanager.docs.rancher.com/).
