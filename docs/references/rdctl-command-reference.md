---
title: "Command Reference: rdctl"
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/references/rdctl-command-reference"/>
</head>

`rdctl` is a command-line tool that allows you to interact with Rancher Desktop from your terminal. It is designed for scripting, automation, troubleshooting, and remote management.

> **Note:** As `rdctl` is still under development, all subcommands, arguments, and output formats are subject to change.

### Prerequisites

-   The Rancher Desktop application must be running to use commands such as `rdctl list-settings`, `rdctl set`, and `rdctl shutdown`.
-   Some `rdctl` commands require credentials to access the Rancher Desktop API. These credentials can be found in the following locations:
    -   **Linux:** `~/.local/share/rancher-desktop/`
    -   **macOS:** `~/Library/Application Support/rancher-desktop/`
    -   **Windows:** `%LOCALAPPDATA%\rancher-desktop\`

## rdctl help

Run `rdctl` or `rdctl help` to see a list of all available commands.

<details>

**Options**

```console autoupdate=true
$ rdctl help
The eventual goal of this CLI is to enable any UI-based operation to be done from the command-line as well.

Usage:
  rdctl [command]

Available Commands:
  api            Run API endpoints directly
  completion     Generate the autocompletion script for the specified shell
  create-profile Generate a deployment profile in either macOS plist or Windows registry format
  extension      Manage extensions
  factory-reset  Clear all the Rancher Desktop state and shut it down.
  help           Help about any command
  list-settings  Lists the current settings.
  set            Update selected fields in the Rancher Desktop UI and restart the backend.
  shell          Run an interactive shell or a command in a Rancher Desktop-managed VM
  shutdown       Shuts down the running Rancher Desktop application
  snapshot       Manage Rancher Desktop snapshots
  start          Start up Rancher Desktop, or update its settings.
  version        Shows the CLI version.

Flags:
      --config-path string   config file (default .../rancher-desktop/rd-engine.json)
  -h, --help                 help for rdctl
      --host string          default is 127.0.0.1; most useful for WSL
      --password string      overrides the password setting in the config file
      --port int             overrides the port setting in the config file
      --user string          overrides the user setting in the config file
      --verbose              Be verbose

Use "rdctl [command] --help" for more information about a command.
```

</details>

## rdctl api

The `rdctl api` command allows you to interact directly with the Rancher Desktop API. This is an advanced feature that is most useful for developers who are working with the API itself. For most users, the other `rdctl` commands provide a more user-friendly interface.

<details>
<summary>Using the API</summary>

For many `rdctl` commands, there are corresponding API calls that can be made directly. The following examples assume you are using `curl` to interact with the API.

-   **List all endpoints globally:** `rdctl api /`
-   **List all endpoints in version 1:** `rdctl api /v1`
-   **Get current settings:** `curl -s -H "Authorization: Basic $AUTH" http://localhost:6107/v1/settings -X GET`
-   **Set properties:** `curl -s -H "Authorization: Basic $AUTH" http://localhost:6107/v1/settings -d '{ "kubernetes": { "containerEngine": "docker", "enabled": false, "version":"1.23.5" }}' -X PUT`
-   **Shutdown Rancher Desktop:** `curl -s -H "Authorization: Basic $AUTH" http://localhost:6107/v1/shutdown -X PUT`

</details>

## rdctl create-profile

Generates a deployment profile for Rancher Desktop settings in either macOS `.plist` or Windows `.reg` format.

```console
$ rdctl create-profile <options> <options-input>
```

<details>

**Options**

```console auto-update=true
$ rdctl create-profile --help
--input [FILE]              File containing a JSON document.
--body [JSON]               Command-line option containing a JSON document
--from-settings             Use current settings.
--output [plist, reg]       An output of .plist files for macOS and .reg files for Windows.

Additional options for --output reg:
--type [locked, defaults]   The locked field is set as default, otherwise the default type can be specified.
--hive [hklm, hkcu]         The hklm field is set as default, otherwise hkcu can be specified.
```

**Example**

```console
$ rdctl create-profile --output reg --hive=hkcu --from-settings
```

</details>

## rdctl extension install

Installs a Rancher Desktop extension.

```console
$ rdctl extension install <image-id>
```

<details>

**Options**

```console auto-update=true
$ rdctl extension install --help
--force               Avoids any interactivity.
<image-id>:<tag>      The <tag> is optional, e.g. splatform/epinio-docker-desktop:latest.
```

**Example**

```console autoupdate=true
$ rdctl extension install docker/logs-explorer-extension:0.2.2
Installing image docker/logs-explorer-extension:0.2.2: Created
```

</details>

## rdctl extension ls

Lists currently installed images.

```console
$ rdctl extension ls
```

<details>

**Example**

```console autoupdate=true
$ rdctl extension ls
Extension IDs

docker/logs-explorer-extension:0.2.2
```

</details>

## rdctl extension uninstall

Uninstalls a Rancher Desktop extension.

```console
$ rdctl extension uninstall <image-id>
```

<details>

**Options**

```console autoupdate=true
$ rdctl extension uninstall --help
rdctl extension uninstall <image-id>
The <image-id> is an image reference, e.g. splatform/epinio-docker-desktop:latest (the tag is optional).

Usage:
  rdctl extension uninstall [flags]

Flags:
  -h, --help   help for uninstall

Global Flags:
      --config-path string   config file (default .../rancher-desktop/rd-engine.json)
      --host string          default is 127.0.0.1; most useful for WSL
      --password string      overrides the password setting in the config file
      --port int             overrides the port setting in the config file
      --user string          overrides the user setting in the config file
      --verbose              Be verbose
```

**Example**

```console autoupdate=true
$ rdctl extension uninstall docker/logs-explorer-extension:0.2.2
Uninstalling image docker/logs-explorer-extension:0.2.2: Deleted docker/logs-explorer-extension:0.2.2
```

</details>

## rdctl list-settings

Run `rdctl list-settings` to see the current active configuration.

<details>
<summary>Example Output</summary>

```json
{
  "version": 15,
  "application": {
    "adminAccess": false,
    "debug": false,
    "extensions": {
      "allowed": {
        "enabled": false,
        "list": []
      },
      "installed": {}
    },
    "pathManagementStrategy": "rcfiles",
    "telemetry": {
      "enabled": true
    },
    "updater": {
      "enabled": false
    },
    "autoStart": false,
    "startInBackground": false,
    "hideNotificationIcon": false,
    "window": {
      "quitOnClose": false
    }
  },
  "containerEngine": {
    "allowedImages": {
      "enabled": false,
      "patterns": []
    },
    "name": "moby"
  },
  "virtualMachine": {
    "memoryInGB": 6,
    "numberCPUs": 2,
    "type": "qemu",
    "useRosetta": false
  },
  "WSL": {
    "integrations": {}
  },
  "kubernetes": {
    "version": "1.32.5",
    "port": 6443,
    "enabled": true,
    "options": {
      "traefik": true,
      "flannel": true
    },
    "ingress": {
      "localhostOnly": false
    }
  },
  "portForwarding": {
    "includeKubernetesServices": false
  },
  "images": {
    "showAll": true,
    "namespace": "default"
  },
  "containers": {
    "showAll": true,
    "namespace": "default"
  },
  "diagnostics": {
    "showMuted": false,
    "mutedChecks": {}
  },
  "experimental": {
    "containerEngine": {
      "webAssembly": {
        "enabled": false
      }
    },
    "kubernetes": {
      "options": {
        "spinkube": false
      }
    },
    "virtualMachine": {
      "mount": {
        "type": "reverse-sshfs",
        "9p": {
          "securityModel": "none",
          "protocolVersion": "9p2000.L",
          "msizeInKib": 128,
          "cacheMode": "mmap"
        }
      },
      "proxy": {
        "enabled": false,
        "address": "",
        "password": "",
        "port": 3128,
        "username": "",
        "noproxy": [
          "0.0.0.0/8",
          "10.0.0.0/8",
          "127.0.0.0/8",
          "169.254.0.0/16",
          "172.16.0.0/12",
          "192.168.0.0/16",
          "224.0.0.0/4",
          "240.0.0.0/4"
        ]
      }
    }
  }
}
```

</details>

## rdctl set

Run `rdctl set [flags]` to update the settings. In most cases, Kubernetes will be reset when you run this command. You can set multiple properties in a single command.

<details>
<summary>Examples</summary>

```bash
# Disable Kubernetes
rdctl set --kubernetes-enabled=false

# Set the container engine and Kubernetes version
rdctl set --container-engine docker --kubernetes-version 1.21.2
```

</details>

## rdctl shutdown

Run `rdctl shutdown` to gracefully shut down Rancher Desktop.

<details>
<summary>Example</summary>

```bash
$ rdctl shutdown
Shutting down.
```

</details>

## rdctl snapshot

Run `rdctl snapshot` to store the current configuration of your virtual machine and all associated settings as a snapshot.

<details>

**Options**

```console autoupdate=true
$ rdctl snapshot --help
Manage Rancher Desktop snapshots

Usage:
  rdctl snapshot [command]

Available Commands:
  create      Create a snapshot
  delete      Delete a snapshot
  list        List snapshots
  restore     Restore a snapshot
  unlock      Remove snapshot lock

Flags:
  -h, --help   help for snapshot

Global Flags:
      --config-path string   config file (default .../rancher-desktop/rd-engine.json)
      --host string          default is 127.0.0.1; most useful for WSL
      --password string      overrides the password setting in the config file
      --port int             overrides the port setting in the config file
      --user string          overrides the user setting in the config file
      --verbose              Be verbose

Use "rdctl snapshot [command] --help" for more information about a command.
```

**Example**

```console autoupdate=true
$ rdctl snapshot create example_snapshot
$ rdctl snapshot delete example_snapshot
$ rdctl snapshot list --json
```

</details>

## rdctl start

<Tabs groupId="command-reference">
  <TabItem value="CLI" default>

Run `rdctl start` to ensure that Rancher Desktop is running and configured as requested.

<details>

**Options:**

```console autoupdate=true
$ rdctl start --help
Starts up Rancher Desktop with the specified settings.
If it's running, behaves the same as 'rdctl set ...'.

Usage:
  rdctl start [flags]

Flags:
      --application.admin-access                                        enable privileged operations
      --application.auto-start                                          start app when logging in
      --application.debug                                               generate more verbose logging
      --application.hide-notification-icon                              don't show notification icon
      --application.path-management-strategy string                     update PATH to include ~/.rd/bin (allowed values: [manual, rcfiles])
      --application.start-in-background                                 start app without window
      --application.telemetry.enabled                                   allow collection of anonymous statistics
      --application.updater.enabled                                     automatically update to the latest release
      --application.window.quit-on-close                                terminate app when the main window is closed
      --container-engine.allowed-images.enabled                         only allow images to be pulled that match the allowed patterns
      --container-engine.name string                                    set engine (allowed values: [containerd, docker, moby])
      --containers.namespace string                                     select only namespaces from this namespace (containerd only)
      --containers.show-all                                             show system containers on Containers page
      --diagnostics.show-muted                                          unhide muted diagnostics
      --experimental.container-engine.web-assembly.enabled              enable support for containerd-wasm shims
      --experimental.kubernetes.options.spinkube                        install spin operator
      --experimental.virtual-machine.mount.9p.cache-mode string         (allowed values: [none, loose, fscache, mmap])
      --experimental.virtual-machine.mount.9p.msize-in-kib int          maximum packet size
      --experimental.virtual-machine.mount.9p.protocol-version string   (allowed values: [9p2000, 9p2000.u, 9p2000.L])
      --experimental.virtual-machine.mount.9p.security-model string     (allowed values: [passthrough, mapped-xattr, mapped-file, none])
      --experimental.virtual-machine.mount.type string                  how directories are shared (allowed values: [reverse-sshfs, 9p, virtiofs])
  -h, --help                                                            help for start
      --images.namespace string                                         select only images from this namespace (containerd only)
      --images.show-all                                                 show system images on Images page
      --kubernetes.enabled                                              run Kubernetes
      --kubernetes.options.flannel                                      use flannel networking; disable to install your own CNI
      --kubernetes.options.traefik                                      install and run traefik
      --kubernetes.port int                                             apiserver port
      --kubernetes.version string                                       choose which version of Kubernetes to run
      --no-modal-dialogs                                                avoid displaying dialog boxes
  -p, --path string                                                     path to main executable
      --port-forwarding.include-kubernetes-services                     show Kubernetes system services on Port Forwarding page
      --virtual-machine.memory-in-gb int                                reserved RAM size
      --virtual-machine.number-cpus int                                 reserved number of CPUs
      --virtual-machine.type string                                     (allowed values: [qemu, vz])
      --virtual-machine.use-rosetta

Global Flags:
      --config-path string   config file (default .../rancher-desktop/rd-engine.json)
      --host string          default is 127.0.0.1; most useful for WSL
      --password string      overrides the password setting in the config file
      --port int             overrides the port setting in the config file
      --user string          overrides the user setting in the config file
      --verbose              Be verbose
```

**Example:**

```console
$ rdctl start --container-runtime dockerd -- kubernetes-version 1.19.3
```

</details>

  </TabItem>
  <TabItem value="API" default>

Run the following API call to ensure Rancher Desktop is running and configured, making sure to fill in your respective user and password values:

<details>
<summary>Example Command</summary>

```shell
$ curl -s -H "Authorization: Basic $(echo -n "user:PASSWORD" | base64)"
```

</details>

  </TabItem>
</Tabs>

## rdctl version

Run `rdctl version` to see the current rdctl CLI version.

<details>
<summary>Example Output</summary>

```console autoupdate=true
$ rdctl version
rdctl client version: v1.19.0, targeting server version: v1
```

</details>
