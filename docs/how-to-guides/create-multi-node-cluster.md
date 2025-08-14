---
title: Create a Multi-Node Cluster with k3d
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/create-multi-node-cluster"/>
</head>

By default, Rancher Desktop provides a single-node Kubernetes cluster, which is sufficient for most local development needs. However, some use cases may require a multi-node cluster or the ability to create multiple clusters. While Rancher Desktop does not have built-in support for multi-node clusters, you can use a third-party tool called [k3d](https://k3d.io) to achieve this.

k3d is a lightweight wrapper that runs k3s (the same minimal Kubernetes distribution used by Rancher Desktop) in Docker. It simplifies the process of creating single- and multi-node k3s clusters, making it an excellent tool for local Kubernetes development.

### Creating a Multi-Node Cluster

Follow these steps to set up a multi-node cluster with k3d:

1.  **Set the Container Runtime**

    In the Rancher Desktop UI, navigate to the **Kubernetes Settings** page and ensure that **dockerd(moby)** is selected as the container runtime.

2.  **Install k3d**

    You can install k3d using one of the following commands:

<Tabs groupId="installation-approach">
<TabItem value="wget" default>

```
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

</TabItem>
<TabItem value="curl">

```
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

</TabItem>
</Tabs>

3.  **Create a Multi-Node Cluster**

    Use the `k3d cluster create` command to create a cluster with the desired number of nodes. For example, to create a two-node cluster, run:

    ```
    k3d cluster create two-node-cluster --agents 2
    ```

    To create a three-node cluster, run:

    ```
    k3d cluster create three-node-cluster --agents 3
    ```

4.  **Switch Between Clusters**

    k3d automatically sets the newly created cluster as the active one. You can switch between different clusters using the `kubectl config use-context` command. For example:

    ```
    kubectl config use-context k3d-two-node-cluster
    ```

To learn more about k3s and k3d, please refer to their official documentation:

-   [k3s documentation](https://docs.k3s.io/)
-   [k3d documentation](https://k3d.io/)

:::caution warning
Please note that clusters created with `k3d` are not managed by the Rancher Desktop GUI.
:::
