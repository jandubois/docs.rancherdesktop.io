---
title: Debugging with VS Code
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/vs-code-docker"/>
</head>

The [VS Code Docker extension](https://code.visualstudio.com/docs/containers/overview) makes it easy to build, manage, and deploy containerized applications in Visual Studio Code. This guide will walk you through the process of debugging a sample application running in a container.

### Step 1: Set Up Your Environment

1.  **Install and Launch Rancher Desktop:**
    Ensure that `dockerd (moby)` is selected as the container runtime in the **Kubernetes Settings** panel.

    ![](../img/vscodedocker/rd-main.png)

2.  **Install and Launch Visual Studio Code:**
    This guide uses the standard version of Visual Studio Code.

    ![](../img/vscodedocker/vscode-main.png)

3.  **Install the VS Code Docker Extension:**
    Install the [Docker extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) from the VS Code Marketplace.

    ![](../img/vscodedocker/vscode-docker-marketplace.png)

### Step 2: Prepare the Sample Application

1.  **Clone the Sample Repository:**
    This guide uses a sample application from the following repository: https://github.com/bwateratmsft/samples.

    ```bash
    git clone https://github.com/bwateratmsft/samples.git
    ```

2.  **Open the Project in VS Code:**
    Open the `expressapp` folder from the cloned repository in VS Code.

### Step 3: Configure the Debugger

1.  **Add Docker Files to Workspace:**
    Open the command palette (`Ctrl+Shift+P`, `F1`, or `Cmd+Shift+P`) and run the **Add Docker Files to Workspace** command.

    -   **Application Platform:** Select `Node.js`.
    -   **Port:** Enter `3000` or another available port.
    -   **Docker Compose Files:** Select `No`.

    This will create a `Dockerfile` and a launch configuration for your project.

    ![](../img/vscodedocker/vscode-docker-add-docker-files-1.png)

2.  **Set a Breakpoint:**
    Open the `app.js` file and set a breakpoint by clicking in the gutter to the left of the line numbers.

    ![](../img/vscodedocker/vscode-docker-debug-breakpoint.png)

### Step 4: Start Debugging

1.  **Select the Debug Configuration:**
    In the **Run and Debug** view, select the **Docker: Launch Node.js** configuration from the dropdown menu.

    ![](../img/vscodedocker/vscode-docker-debug-configuration.png)

2.  **Start the Debugger:**
    Press `F5` to start the debugger. This will build and run the container in debug mode. Your default web browser will open, and the application will pause at the breakpoint you set.

    ![](../img/vscodedocker/vscode-docker-debug-breakpoint-hit.png)

### Troubleshooting

-   **Breakpoint Not Hit:** If the application does not stop at the breakpoint on the first run, it may be because the debugger has not yet attached. Refresh your web browser to trigger the execution again. You can also set `"inspectMode": "break"` in the `task.json` file to prevent the application from running until the debugger is attached.

-   **Firewall Issues:** On some machines, firewall settings may prevent the debugger from connecting to the container. If you are on Windows, you can add a firewall rule to allow communication from the WSL VM by running the following command in a privileged PowerShell session:

    ```powershell
    New-NetFirewallRule -Action Allow -Description 'Allow communication from WSL containers' -Direction Inbound -Enabled True -InterfaceAlias 'vEthernet (WSL)' -Name 'WSL Inbound' -DisplayName 'WSL Inbound'
    ```
