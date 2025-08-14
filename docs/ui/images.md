---
sidebar_label: Images
title: Images
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/ui/images"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="Windows">

![](rd-versioned-asset://ui-main/Windows_Images.png)

</TabItem>
<TabItem value="macOS">

![](rd-versioned-asset://ui-main/macOS_Images.png)

</TabItem>
<TabItem value="Linux">

![](rd-versioned-asset://ui-main/Linux_Images.png)

</TabItem>
</Tabs>

The **Images** tab allows you to build, pull, scan, and manage your container images through the Rancher Desktop user interface.

For information on how to manage images from the command line, please refer to the [Working with Images tutorial](../tutorials/working-with-images.md).

### Image List

The main view of the **Images** tab displays a list of all the container images that are currently available in your environment. For each image, you can see its name, tag, and size.

You can use the following UI elements to manage the image list:

-   **Filter:** The search box allows you to filter the list of images by name.
-   **Show All Images (containerd only):** When using the `containerd` runtime with the `k8s.io` namespace, you can check this box to display all images, including system images.
-   **Namespace (containerd only):** When using the `containerd` runtime, you can use this dropdown menu to filter images by namespace.

### Building an Image

To build a new image from a `Dockerfile`:

1.  Click the **Add Image** button in the upper-right corner.
2.  Select the **Build** tab.
3.  Enter a name for your new image (e.g., `my-image:my-tag`).
4.  Click the **Build** button and select the `Dockerfile` you want to use.

### Pulling an Image

To pull an image from a container registry:

1.  Click the **Add Image** button in the upper-right corner.
2.  Select the **Pull** tab.
3.  Enter the name of the image you want to pull.
    > By default, images are pulled from [Docker Hub]. To pull from a different registry, you must include the hostname in the image name (e.g., `registry.example.com/repo/image:tag`).
4.  Click the **Pull** button.

[Docker Hub]: https://hub.docker.com/

### Scanning Images

Rancher Desktop uses [Trivy] to scan your images for security vulnerabilities.

To scan an image:

1.  In the image list, find the image you want to scan.
2.  Click the **⋮** button and select **Scan**.
3.  A summary of the vulnerabilities, sorted by severity, will be displayed.
4.  You can expand each vulnerability to view more details, including links to more information.

[Trivy]: https://github.com/aquasecurity/trivy

### Deleting an Image

To delete an image from your local environment:

1.  In the image list, find the image you want to delete.
2.  Click the **⋮** button and select **Delete**.
