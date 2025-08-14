---
sidebar_label: Allowed Images
title: Allowed Images
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/preferences/container-engine/allowed-images"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

The **Allowed Images** tab allows you to control which container images can be pulled or pushed in Rancher Desktop. This is useful for enforcing security policies and ensuring that users only access images from approved registries.

<Tabs groupId="os">
<TabItem value="Windows">

![](rd-versioned-asset://preferences/Windows_containerEngine_tabAllowedImages.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://preferences/macOS_containerEngine_tabAllowedImages.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://preferences/Linux_containerEngine_tabAllowedImages.png)

</TabItem>
</Tabs>

### Allowed Image Patterns

To enable this feature, check the **Enable** box. Once enabled, only images that match at least one of the specified patterns will be allowed. You can use the **+** and **-** buttons to add and remove patterns.

#### Pattern Format

Allowed image patterns are specified using the following format:

```
[registry/][:port/][organization/]repository[:tag]
```

-   If `registry` is not specified, it defaults to Docker Hub (`docker.io`).
-   If `port` is not specified, it defaults to `443`.
-   `organization` only applies to Docker Hub and defaults to `library`.
-   If `tag` is not specified, it defaults to any tag.

> **Note:** Filtering by `tag` is not fully supported. To allow a specific tag, you must also add the corresponding image digest (`repository@digest`) to the allow list.

#### Examples

| Pattern                  | Meaning                                                                                                                                                                                                             |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `busybox`                | Allows the `busybox` repository in the `library` organization on Docker Hub.                                                                                                                                        |
| `suse/`                  | Allows any image in the `suse` organization on Docker Hub with a single segment in the repository name (e.g., `suse/nginx`, but not `suse/cap/uaa`).                                                                 |
| `suse//`                 | Allows any image in the `suse` organization on Docker Hub with one or more segments in the repository name (e.g., `suse/cap/uaa`).                                                                                    |
| `registry.internal:5000` | Allows any image from the `registry.internal:5000` registry.                                                                                                                                                          |
| `registry.suse.com/nginx`| Allows the `nginx` image from the `registry.suse.com` registry.                                                                                                                                                       |
