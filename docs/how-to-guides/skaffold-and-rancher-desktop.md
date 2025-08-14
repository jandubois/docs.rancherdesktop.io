---
title: Skaffold and Rancher Desktop
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/skaffold-and-rancher-desktop"/>
</head>

[Skaffold](https://skaffold.dev/docs/) is a command-line tool that facilitates continuous development for Kubernetes applications. It handles the workflow for building, pushing, and deploying your application, allowing you to focus on iterating on your code while Skaffold continuously deploys to your cluster.

This guide will walk you through setting up Skaffold with Rancher Desktop using a [sample Node.js application](https://github.com/rancher-sandbox/docs.rancherdesktop.io/tree/main/assets/express-sample).

### Prerequisites

-   **Container Runtime:** Skaffold works with the `dockerd (moby)` container runtime. Please ensure that `dockerd` is selected in the **Kubernetes Settings** panel of the Rancher Desktop UI.

### Step 1: Install Skaffold

Install Skaffold by following the instructions on the [official installation page](https://skaffold.dev/docs/install/).

### Step 2: Prepare the Sample Application

Clone the Rancher Desktop documentation repository and navigate to the sample application directory:

```bash
git clone https://github.com/rancher-sandbox/docs.rancherdesktop.io.git
cd docs.rancherdesktop.io/assets/express-sample
```

### Step 3: Initialize the Project

Run the `skaffold init` command to generate a `skaffold.yaml` configuration file for your project.

```bash
skaffold init
```

Skaffold will scan your project for build configuration files (e.g., `Dockerfile`, `package.json`) and prompt you to confirm the configuration. For this example, you can accept the defaults and allow Skaffold to write the `skaffold.yaml` file.

### Step 4: Configure the Image Repository

Before you can deploy your application, you need to decide where Skaffold will push your container image. You have three main options:

<Tabs>
<TabItem value="docker-hub" label="Docker Hub" default>

If you have a Docker Hub account, you can configure Skaffold to push your image to your personal repository.

1.  Log in to Docker Hub:
    ```bash
    docker login
    ```

2.  In the `skaffold.yaml` and `manifests.yaml` files, replace `matamagu/express-sample` with `<YOUR-DOCKER-HUB-USERNAME>/express-sample`.

</TabItem>
<TabItem value="local-registry" label="Local Registry">

You can run a local container registry to store your images.

1.  Start the local registry:
    ```bash
    docker run -d -p 5000:5000 --restart=always --name registry registry:2
    ```

2.  When you run `skaffold dev`, use the `--default-repo` flag to point to your local registry:
    ```bash
    skaffold dev --default-repo=localhost:5000
    ```

</TabItem>
<TabItem value="local-build" label="Local Build">

You can build your image locally without pushing it to a registry.

1.  In your `manifests.yaml` file, set the `imagePullPolicy` to `IfNotPresent`.

2.  Update your `skaffold.yaml` file to disable pushing the image:

    ```yaml
    apiVersion: skaffold/v2beta29
    kind: Config
    metadata:
      name: skaffold
    build:
      local:
        push: false
        useDockerCLI: true
    ```

</TabItem>
</Tabs>

### Step 5: Start the Development Workflow

Now you can start the Skaffold development workflow using the `skaffold dev` command. This will build and deploy your application and automatically redeploy it whenever you make changes to your code.

```bash
skaffold dev
```

### Step 6: Access Your Application

Once the application is deployed, you can access it by navigating to `http://localhost:3000` in your web browser.

