---
title: Transfer Container Images
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/transfer-container-images"/>
</head>

There are times when you may need to transfer container images from one environment to another. For example, you might want to move images between the `containerd` and `dockerd` runtimes in Rancher Desktop, or you might want to import images from a previous container management tool.

This guide explains how to use the `save` and `load` commands to transfer images.

### Step 1: Save Images to a Tar Archive

First, you will need to save your images from the source environment into a `.tar` archive.

#### Saving a Single Image

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl save -o local-image.tar image:tag
```

</TabItem>
<TabItem value="docker">

```bash
docker save -o local-image.tar image:tag
```

</TabItem>
</Tabs>

#### Saving Multiple Images

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl save -o local-images.tar image1:tag1 image2:tag2
```

</TabItem>
<TabItem value="docker">

```bash
docker save -o local-images.tar image1:tag1 image2:tag2
```

</TabItem>
</Tabs>

#### Saving All Images in a Namespace

You can save all images in a given namespace using a combination of the `ls` and `save` commands, along with a JSON parser like `jq`.

The following commands will:
1.  List all images in the specified namespace.
2.  Filter and format the list of images.
3.  Save the formatted list of images to a `.tar` archive.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

<Tabs groupId="shell">
<TabItem value="Bash" default>

```bash
nerdctl -n k8s.io save -o all-local-images-in-namespace.tar $(nerdctl -n k8s.io image ls --format '{{json .}}' | jq -r 'select(.Repository!="<none>") | if (.Tag=="<none>") then .Repository else (.Repository+":"+.Tag) end')
```

</TabItem>
<TabItem value="PowerShell">

```powershell
nerdctl -n k8s.io save -o all-local-images-in-namespace.tar $(nerdctl -n k8s.io image ls --format '{{json .}}' | jq -r 'select(.Repository!=\"<none>\") | if (.Tag==\"<none>\") then .Repository else ($_.Repository+\":\"+$_.Tag) end')
```

</TabItem>
</Tabs>

</TabItem>
<TabItem value="docker">

<Tabs groupId="shell">
<TabItem value="Bash" default>

```bash
docker save -o all-local-images.tar $(docker image ls --format '{{json .}}' | jq -r 'select(.Repository!="<none>") | if (.Tag=="<none>") then .Repository else (.Repository+":"+.Tag) end')
```

</TabItem>
<TabItem value="PowerShell">

```powershell
docker save -o all-local-images.tar $(docker image ls --format '{{json .}}' | jq -r 'select(.Repository!=\"<none>\") | if (.Tag==\"<none>\") then .Repository else ($_.Repository+\":\"+$_.Tag) end')
```

</TabItem>
</Tabs>

</TabItem>
</Tabs>

### Step 2: Load Images from the Tar Archive

Once you have saved your images to a `.tar` archive, you can load them into your target environment.

<Tabs groupId="container-runtime">
<TabItem value="nerdctl" default>

```bash
nerdctl load < local-images.tar
```

</TabItem>
<TabItem value="docker">

```bash
docker load -i local-images.tar
```

</TabItem>
</Tabs>
