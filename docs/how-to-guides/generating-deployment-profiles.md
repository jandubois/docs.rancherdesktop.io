---
title: Generating Deployment Profiles
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/generating-deployment-profiles"/>
</head>

This guide provides step-by-step instructions for creating and implementing deployment profiles in Rancher Desktop. Deployment profiles are used to define default settings for new installations and to lock specific configurations to prevent user modifications.

For a general overview of deployment profiles, please refer to the [Deployment Profiles documentation](../getting-started/deployment.md).

:::note
If your organization has its own methods of remotely configuring users' systems, it is out of the scope of this document.
:::

## Step 1: Understand Profile Locations

Before creating a deployment profile, it is important to know where to install it. The location varies depending on the operating system and whether you are creating a user or system profile.

### Linux

-   **User Profiles:**
    -   `~/.config/rancher-desktop.defaults.json`
    -   `~/.config/rancher-desktop.locked.json`
    -   *If the `XDG_CONFIG_HOME` environment variable is set, profiles are stored there instead.*
-   **System Profiles:**
    -   `/etc/rancher-desktop/defaults.json`
    -   `/etc/rancher-desktop/locked.json`

Linux profiles are stored as JSON files.

### macOS

-   **User Profiles:**
    -   `~/Library/Preferences/io.rancherdesktop.profile.defaults.plist`
    -   `~/Library/Preferences/io.rancherdesktop.profile.locked.plist`
-   **System Profiles:**
    -   `/Library/Preferences/io.rancherdesktop.profile.defaults.plist`
    -   `/Library/Preferences/io.rancherdesktop.profile.locked.plist`

macOS profiles are stored as `plist` files, which are XML-based. System profiles require root privileges to modify.

### Windows

-   **User Profiles:**
    -   `HKEY_CURRENT_USER\SOFTWARE\Policies\Rancher Desktop\Defaults`
    -   `HKEY_CURRENT_USER\SOFTWARE\Policies\Rancher Desktop\Locked`
-   **System Profiles:**
    -   `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Rancher Desktop\Defaults`
    -   `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Rancher Desktop\Locked`

Windows profiles are stored in the registry. The structure of the registry keys mirrors the JSON structure used on other platforms.

## Step 2: Generate JSON Profile Files

The easiest way to create a deployment profile is to start with your current Rancher Desktop settings. This guide assumes you have Rancher Desktop installed and are familiar with the `rdctl` command-line tool.

### Prerequisite

This guide uses the `jq` command-line tool to work with JSON. Please ensure it is installed on your system. If you prefer a different JSON processor, you can substitute it in the following commands.

### Creating the Base Files

1.  **Start with a Clean Slate (Optional)**

    If you want to create a profile based on the default Rancher Desktop settings, you can perform a factory reset first:

    ```bash
    rdctl factory-reset
    ```

2.  **Generate `defaults` and `locked` Files**

    Use `rdctl` to export your current settings into two separate files—one for default values and one for locked values:

    ```bash
    rdctl list-settings | jq . > working-defaults.json
    rdctl list-settings | jq . > working-locked.json
    ```

    You only need to create the files for the profile types you intend to use.

### Editing the Profile Files

Next, edit the `working-defaults.json` and `working-locked.json` files to include only the settings you wish to manage. Remove any settings that you do not want to include in the profile.

Here is an example of a `locked` profile that restricts the container engine, allowed images, and Kubernetes version:

```json
{
  "containerEngine": {
    "allowedImages": {
      "enabled": true,
      "patterns": [
        "PATTERN1",
        "PATTERN2"
      ]
    },
    "name": "moby"
  },
  "kubernetes": {
    "version": "1.24.10"
  }
}
```

:::caution
With the profile above, users will be forced to use the `moby` container engine, will only be able to pull images that match the specified patterns, and will be locked to the defined Kubernetes version.
:::

### Validating the JSON

After editing your profile files, you can validate their JSON syntax using `jq`:

```bash
jq -e . working-defaults.json >/dev/null 2>&1
```

This command will exit with a status of `0` if the file is valid and `1` otherwise.

## Step 3: Test and Deploy the Profiles

Once you have created and validated your profile files, you can test them on a local Rancher Desktop installation before deploying them to your users' machines.

### Testing on Linux

1.  Shut down Rancher Desktop and perform a factory reset.
2.  Copy your profile files to the appropriate location:

    ```bash
    rdctl factory-reset
    cp working-defaults.json ~/.config/rancher-desktop.defaults.json
    cp working-locked.json ~/.config/rancher-desktop.locked.json
    ```

3.  Start Rancher Desktop and open the **Preferences** window.
4.  Verify that the settings from your `locked` profile are applied, display a padlock icon, and cannot be changed.
5.  Verify that the settings from your `defaults` profile are applied but can be changed.

### Deploying to Other Systems

If you need to deploy profiles to macOS or Windows systems, you can use `rdctl` to convert your JSON files into the appropriate format.

#### macOS

To generate `plist` files for macOS, run the following commands:

```bash
rdctl create-profile --output plist --input working-defaults.json > io.rancherdesktop.profile.defaults.plist
rdctl create-profile --output plist --input working-locked.json > io.rancherdesktop.profile.locked.plist
```

You can then deploy these `plist` files to the locations described in Step 1.

#### Windows

For Windows, you will generate registry import files (`.reg`). You need to specify the profile type (`defaults` or `locked`) and the hive (`hklm` for system or `hkcu` for user):

```bash
# Generate system profiles
rdctl create-profile --output reg --hive hklm --type defaults working-defaults.json > reg-system-defaults.reg
rdctl create-profile --output reg --hive hklm --type locked working-locked.json > reg-system-locked.reg

# Generate user profiles
rdctl create-profile --output reg --hive hkcu --type defaults working-defaults.json > reg-user-defaults.reg
rdctl create-profile --output reg --hive hkcu --type locked working-locked.json > reg-user-locked.reg
```

You can then deploy these `.reg` files to the target Windows systems and import them using the `reg import` command in a shell with administrative privileges:

```bash
reg import FILENAME.reg
```

## Troubleshooting

System profiles are loaded only when there is no `settings.json` in the config area - so these typically are loaded only on first-run and after a factory-reset.

Locked profiles are loaded at the start of every run, and take precedence over the values in existing `settings.json` files (which means a crafty user can't shut down Rancher Desktop and manually edit the `settings.json` file to get around the locked-deployment-profile restrictions).

### Running `reg.exe`

Running `reg.exe` - registry entries under the `Policies` key can only be loaded with the `reg import` command when the user is running as an administrator.
