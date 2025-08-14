---
title: Using VS Code Remote Containers
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/vs-code-remote-containers"/>
</head>

The [Visual Studio Code Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension allows you to use a Docker container as a full-featured development environment. This helps to ensure a consistent environment across developer machines and makes it easy for new team members to get started.

This guide will walk you through the process of using the Remote - Containers extension with Rancher Desktop.

### Step 1: Set Up Your Environment

1.  **Install and Launch Rancher Desktop:**
    Ensure that `dockerd (moby)` is selected as the container runtime in the **Kubernetes Settings** panel.

    ![](../img/vscoderemotecontainers/rd-main.png)

2.  **Install and Launch Visual Studio Code:**
    This guide uses the standard version of Visual Studio Code.

    ![](../img/vscoderemotecontainers/vscode-main.png)

3.  **Install the Remote Development Extension Pack:**
    Install the [Remote Development extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) from the VS Code Marketplace. This pack includes the Remote - Containers extension.

    ![](../img/vscoderemotecontainers/vscode-remotedevelopment-marketplace.png)

    After the extension is installed, you will see a new item in the side bar and a green button in the lower-left corner, which provides access to the Remote Development commands.

    ![](../img/vscoderemotecontainers/vscode-remotedevelopment-installed.png)

### Step 2: Open a Dev Container

1.  **Clone the Sample Repository:**
    This guide uses the sample dev containers provided by Microsoft. Clone the repository to your local machine:

    ```bash
    git clone https://github.com/microsoft/vscode-dev-containers.git
    ```

2.  **Open a Project in a Container:**
    -   Press `F1` to open the command palette.
    -   Type "Dev Containers: Open Folder in Container..." and select the command.
    -   Browse to the `vscode-dev-containers` directory you cloned and select one of the sample projects, such as `javascript-node`.

    ![](../img/vscoderemotecontainers/vscode-remotedevelopment-commandpalette.png)

    ![](../img/vscoderemotecontainers/vscode-remotedevelopment-openfolder.png)

    VS Code will now build and start the dev container. You can monitor the progress in the notification area. Once the container is running, the name of the container will be displayed in the lower-left corner.

    ![](../img/vscoderemotecontainers/vscode-remotedevelopment-containersuccess.png)

### Step 3: Run the Application

Once the dev container is running, you can start the sample application.

1.  Press `F5` to start the debugger.
2.  The application will start and be forwarded to `localhost:3000`.

![](../img/vscoderemotecontainers/vscode-remotedevelopment-appinbrowser.png)

You have now successfully used the VS Code Remote - Containers extension with Rancher Desktop to develop an application in a container.

### Next Steps

Microsoft provides extensive documentation on using dev containers in various scenarios. For more information, please refer to the [official documentation](https://code.visualstudio.com/docs/remote/remote-overview).

[Visual Studio Code Remote - Containers]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers
[Moby]: https://mobyproject.org/
[here]: https://code.visualstudio.com/docs/remote/remote-overview
