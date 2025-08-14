---
title: Using Testcontainers on Rancher Desktop
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/using-testcontainers"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

[Testcontainers](https://testcontainers.com/) is a powerful tool for writing ephemeral tests with real services in Docker containers. This guide will walk you through the process of setting up and using Testcontainers with Rancher Desktop.

### Prerequisites

-   **Container Runtime:** Testcontainers requires a Docker-API compatible container runtime. Please ensure that **moby (dockerd)** is selected as the container runtime in the Rancher Desktop UI.
-   **Kubernetes (Apple Silicon only):** On Apple Silicon machines, you must disable Kubernetes. You can do this from the **Kubernetes Settings** panel or by running the following command:
    ```bash
    rdctl set --kubernetes-enabled=false
    ```
-   **Apache Maven:** Please ensure that [Apache Maven](https://maven.apache.org/install.html) is installed on your machine.

### Step 1: Download the Sample Project

This guide uses a sample Java project to demonstrate how to use Testcontainers. Clone the repository to your local machine:

```bash
git clone https://github.com/testcontainers/testcontainers-java-repro
```

### Step 2: Configure Your Environment (Apple Silicon Only)

If you are using a Mac with Apple Silicon, you will need to perform some additional configuration steps.

#### Using QEMU

If you are using the QEMU virtual machine, you will need to enable administrative access and export the `TESTCONTAINERS_HOST_OVERRIDE` environment variable.

1.  Enable administrative access in **Preferences > Application > General**.
2.  Export the environment variable:
    ```bash
    export TESTCONTAINERS_HOST_OVERRIDE=$(rdctl shell ip a show rd0 | awk '/inet / {sub("/.*",""); print $2}')
    ```

#### Using VZ

If you are using the VZ virtual machine, you have two options:

-   **With Administrative Access:**
    1.  Enable administrative access in **Preferences > Application > General**.
    2.  Export the environment variable:
        ```bash
        export TESTCONTAINERS_HOST_OVERRIDE=$(rdctl shell ip a show vznat | awk '/inet / {sub("/.*",""); print $2}')
        ```

-   **Without Administrative Access:**
    Export the following environment variables:
    ```bash
    export DOCKER_HOST=unix://$HOME/.rd/docker.sock
    export TESTCONTAINERS_DOCKER_SOCKET_OVERRIDE=/var/run/docker.sock
    export TESTCONTAINERS_HOST_OVERRIDE=$(rdctl shell ip a show vznat | awk '/inet / {sub("/.*",""); print $2}')
    ```

### Step 3: Run the Tests

Once you have configured your environment, navigate to the `testcontainers-java-repro` directory and run the tests using Maven:

```bash
cd testcontainers-java-repro
mvn verify
```

After the tests have completed, you should see a `BUILD SUCCESS` message in your terminal, along with a summary of the test results.
