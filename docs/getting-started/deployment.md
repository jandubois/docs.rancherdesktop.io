---
title: Deployment Profiles
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/getting-started/deployment"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

Deployment profiles are a powerful feature for administrators who need to standardize and manage Rancher Desktop settings across multiple users and machines. They allow you to define default configurations and lock specific settings to ensure consistency and compliance.

There are two main types of deployment profiles:

- **Defaults:** These profiles provide a set of default preference values that are applied on the first run of Rancher Desktop or after a factory reset.
- **Locked:** These profiles allow an administrator to pin specific preference values, preventing users from modifying them through the GUI or CLI.

Profiles can be specified for both an "admin" and a "user." However, if an admin profile (either "defaults" or "locked") is present, any user profiles will be ignored.

### How Rancher Desktop Loads Settings

Rancher Desktop loads its settings in the following order:

1.  **Admin Profile:** It first loads the "admin" deployment profile, including both "defaults" and "locked" settings.
2.  **User Profile:** If no admin profile is found, it loads the "user" deployment profile.
3.  **Saved Preferences:** It then loads the saved preferences from the `settings.json` file.
4.  **Defaults Fallback:** If there are no saved settings, the "defaults" from the loaded profile are used.
5.  **Command-Line Arguments:** Settings are updated with any values passed as command-line arguments during launch.
6.  **First-Run Dialog:** If no settings have been configured at this point, the first-run dialog is displayed.
7.  **Application Defaults:** Any remaining missing values are filled in from the built-in application defaults.
8.  **Locked Settings:** Finally, any values from the "locked" profile are applied, overriding all other settings.

It is important to note that Rancher Desktop will not start if a profile is present but cannot be parsed correctly. Additionally, deployment profiles are not modified or removed by Rancher Desktop and will persist through factory resets and uninstalls.

The structure of a deployment profile mirrors the application's settings, which you can view by running `rdctl list-settings`:

```json title="rdctl list-settings output"
{
  "version": 10,
  "containerEngine": {
    "allowedImages": {
      "enabled": false,
      "patterns": []
    },
    "name": "containerd"
  },
  ...
}
```

The following sections provide platform-specific instructions for creating a deployment profile that sets the default container engine to `moby`, disables Kubernetes, and locks the list of allowed images to `busybox` and `nginx`.

### Locked Preference Fields

When a setting is locked, it cannot be changed by the user from the Rancher Desktop UI or from the command line. This feature is useful for enforcing specific configurations in an enterprise environment.

For example, if you lock the Kubernetes version, the user will not be able to change it from the **Kubernetes Settings** screen. The UI will indicate that the setting is locked by displaying a lock icon next to the field.

<details>
<summary>Locked Fields UI Examples</summary>

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://preferences/Windows_containerEngine_tabAllowedImages_lockedFields.png)

![](rd-versioned-asset://preferences/Windows_kubernetes_lockedFields.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://preferences/macOS_containerEngine_tabAllowedImages_lockedFields.png)

![](rd-versioned-asset://preferences/macOS_kubernetes_lockedFields.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://preferences/Linux_containerEngine_tabAllowedImages_lockedFields.png)

![](rd-versioned-asset://preferences/Linux_kubernetes_lockedFields.png)

</TabItem>
</Tabs>

</details>

### Profile Location and Format

Deployment profiles are stored in platform-specific locations and formats.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

On Windows the deployment profiles are stored in the registry and can be distributed via group policy.

The locations for the profiles are:

```
HKEY_LOCAL_MACHINE\Software\Policies\Rancher Desktop\Defaults
HKEY_LOCAL_MACHINE\Software\Policies\Rancher Desktop\Locked
HKEY_CURRENT_USER\Software\Policies\Rancher Desktop\Defaults
HKEY_CURRENT_USER\Software\Policies\Rancher Desktop\Locked
```

The `reg` tool can be used to create a profile manually. To create an "admin" profile it will have to be executed from an elevated shell.

Boolean values are stored in `REG_DWORD` format, and lists in `REG_MULTI_SZ`.

#### Delete existing profiles

```
reg delete "HKCU\Software\Policies\Rancher Desktop" /f
```

#### By default use the "moby" container engine and disable Kubernetes

```
reg add "HKCU\Software\Policies\Rancher Desktop\Defaults" /v version /t REG_DWORD -d 10
reg add "HKCU\Software\Policies\Rancher Desktop\Defaults\containerEngine" /v name /t REG_SZ -d moby
reg add "HKCU\Software\Policies\Rancher Desktop\Defaults\kubernetes" /v enabled /t REG_DWORD -d 0
```

#### Lock allowed images list to only allow "busybox" and "nginx"

```
reg add "HKCU\Software\Policies\Rancher Desktop\Locked" /v version /t REG_DWORD -d 10
reg add "HKCU\Software\Policies\Rancher Desktop\Locked\containerEngine\allowedImages" /v enabled /t REG_DWORD -d 1
reg add "HKCU\Software\Policies\Rancher Desktop\Locked\containerEngine\allowedImages" /v patterns /t REG_MULTI_SZ -d busybox\0nginx
```

#### Verify registry settings

The profile can be exported into a `*.reg` file

```
C:\>reg export "HKCU\Software\Policies\Rancher Desktop" rd.reg
The operation completed successfully.
```

This file can be used to distribute the profile to other machines. Note that the `REG_MULTI_SZ` values are encoded in UTF16LE, so are not easily readable:

```text title="HKCU\Software\Policies\Rancher Desktop"
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Policies\Rancher Desktop]

[HKEY_CURRENT_USER\Software\Policies\Rancher Desktop\Defaults]
"version"=dword:a

[HKEY_CURRENT_USER\Software\Policies\Rancher Desktop\Defaults\containerEngine]
"name"="moby"

[HKEY_CURRENT_USER\Software\Policies\Rancher Desktop\Defaults\kubernetes]
"enabled"=dword:00000000

[HKEY_CURRENT_USER\Software\Policies\Rancher Desktop\Locked]
"version"=dword:a

[HKEY_CURRENT_USER\Software\Policies\Rancher Desktop\Locked\containerEngine]

[HKEY_CURRENT_USER\Software\Policies\Rancher Desktop\Locked\containerEngine\allowedImages]
"enabled"=dword:00000001
"patterns"=hex(7):62,00,75,00,73,00,79,00,62,00,6f,00,78,00,00,00,6e,00,67,00,\
  69,00,6e,00,78,00,00,00,00,00
```

</TabItem>
<TabItem value="macOS">

On macOS the deployment profiles are stored as plist files.

The locations for the profiles are:

```
/Library/Preferences/io.rancherdesktop.profile.defaults.plist
/Library/Preferences/io.rancherdesktop.profile.locked.plist
~/Library/Preferences/io.rancherdesktop.profile.defaults.plist
~/Library/Preferences/io.rancherdesktop.profile.locked.plist
```

#### Convert all current settings into a deployment profile

A simple way to turn existing settings into a deployment profile is by converting them from JSON into the plist XML format, and then manually trimming it down in an editor.

```
rdctl list-settings | plutil -convert xml1 - -o ~/Library/Preferences/io.rancherdesktop.profile.defaults.plist
```

Alternatively the profile can be created manually, either with an editor, or with the `plutil` tool. The `defaults` utility doesn't work because it cannot create nested keys.

#### By default use the "moby" container engine and disable Kubernetes

```
DEFAULTS=~/Library/Preferences/io.rancherdesktop.profile.defaults.plist
plutil -create xml1 "$DEFAULTS"

plutil -insert version -integer 10 "$DEFAULTS"

plutil -insert containerEngine -dictionary "$DEFAULTS"
plutil -insert containerEngine.name -string moby "$DEFAULTS"

plutil -insert kubernetes -dictionary "$DEFAULTS"
plutil -insert kubernetes.enabled -bool false "$DEFAULTS"
```

#### Lock allowed images list to only allow "busybox" and "nginx"

```
LOCKED=~/Library/Preferences/io.rancherdesktop.profile.locked.plist
plutil -create xml1 "$LOCKED"

plutil -insert version -integer 10 "$LOCKED"

plutil -insert containerEngine -dictionary "$LOCKED"
plutil -insert containerEngine.allowedImages -dictionary "$LOCKED"
plutil -insert containerEngine.allowedImages.enabled -bool true "$LOCKED"

plutil -insert containerEngine.allowedImages.patterns -array "$LOCKED"
plutil -insert containerEngine.allowedImages.patterns -string busybox -append "$LOCKED"
plutil -insert containerEngine.allowedImages.patterns -string nginx -append "$LOCKED"
```

#### Verify the plist files

```xml title="~/Library/Preferences/io.rancherdesktop.profile.defaults.plist"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>containerEngine</key>
	<dict>
		<key>name</key>
		<string>moby</string>
	</dict>
	<key>kubernetes</key>
	<dict>
		<key>enabled</key>
		<false/>
	</dict>
	<key>version</key>
	<integer>10</integer>
</dict>
</plist>
```

```xml title="~/Library/Preferences/io.rancherdesktop.profile.locked.plist"
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>containerEngine</key>
	<dict>
		<key>allowedImages</key>
		<dict>
			<key>enabled</key>
			<true/>
			<key>patterns</key>
			<array>
				<string>busybox</string>
				<string>nginx</string>
			</array>
		</dict>
	</dict>
	<key>version</key>
	<integer>10</integer>
</dict>
</plist>
```

</TabItem>
<TabItem value="Linux">

On Linux the deployment profiles are stored in JSON format.

The locations for the profiles are:

```
/etc/rancher-desktop/defaults.json
/etc/rancher-desktop/locked.json
~/.config/rancher-desktop.defaults.json
~/.config/rancher-desktop.locked.json
```

#### Convert all current settings into a deployment profile

Since deployment profiles are stored in JSON format, the simplest way to create them is by saving the current application settings to the profile location, and then fine-tuning the profile with a text editor.

```
rdctl list-settings > ~/.config/rancher-desktop.defaults.json
```

#### By default use the "moby" container engine and disable Kubernetes

```json title="~/.config/rancher-desktop.defaults.json"
{
  "version": 10,
  "containerEngine": {
    "name": "moby"
  },
  "kubernetes": {
    "enabled": false
  }
}
```

#### Lock allowed images list to only allow "busybox" and "nginx"

```json title="~/.config/rancher-desktop.locked.json"
{
  "version": 10,
  "containerEngine": {
    "allowedImages": {
      "enabled": true,
      "patterns": ["busybox","nginx"]
    }
  }
}
```

</TabItem>
</Tabs>

### Important: The `version` Field

When creating or updating deployment profiles, it is essential to include a `version` field. This field ensures that Rancher Desktop can correctly interpret the profile. As of version 1.12, `rdctl` automatically includes this field in generated profiles.

If you have deployment profiles created with older versions of Rancher Desktop, you will need to add the `version` field manually. The current version is `10`.

**Linux:**

Add `"version": 10` at the beginning of your JSON file, immediately after the opening brace `{`.

**macOS:**

Add `<key>version</key><integer>10</integer>` after the initial `<dict>` tag in your `.plist` file.

**Windows:**

Add a `DWORD` value named `version` with a value of `10` (hexadecimal `a`) at the top level of each profile:

```
"version"=dword:a
```

### Known Issues and Limitations

-   Currently, you can set default values for `diagnostics.showMuted` (and `WSL.integrations` on Windows) through a deployment profile, but you cannot lock them.
