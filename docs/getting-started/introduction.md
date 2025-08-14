---
title: Introduction
slug: /
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/"/>
</head>

import TabsConstants from '@site/core/TabsConstants';

Rancher Desktop is an application that brings container management and Kubernetes to your local desktop. It is available for Mac (Intel and Apple Silicon), Windows, and Linux.

![](rd-versioned-asset://getting-started/introduction_preferences_tabKubernetes.png)

_The image above shows the Kubernetes settings in Rancher Desktop on macOS (left) and Windows (right)._

## Container Management

Rancher Desktop gives you the power to build, push, and pull container images, and to run containers. You can choose between two container engines: Moby/dockerd or containerd. When you select Moby/dockerd, you can use the Docker CLI. If you opt for containerd, you can use the nerdctl, a Docker-compatible CLI.

## Kubernetes

Rancher Desktop includes a built-in Kubernetes instance, powered by k3s, a lightweight, certified Kubernetes distribution. This integration allows you to select your preferred Kubernetes version and easily reset your Kubernetes environment, or the entire container runtime, with a single click.

## Rancher vs. Rancher Desktop

Although Rancher and Rancher Desktop share the "Rancher" name, they serve different purposes. Rancher is a comprehensive platform for managing multiple Kubernetes clusters, while Rancher Desktop provides a local Kubernetes and container management setup for development. The two tools are designed to work well together.

If you wish to run Rancher on your local machine, you can easily install it within Rancher Desktop.
