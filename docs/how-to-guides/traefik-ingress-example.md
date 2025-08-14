---
title: Using the Traefik Ingress Controller
---

import TabsConstants from '@site/core/TabsConstants';

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/traefik-ingress-example"/>
</head>

Rancher Desktop uses [Traefik](https://doc.traefik.io/traefik/) as the default Ingress controller for its Kubernetes cluster. This guide will walk you through the process of using the Traefik Ingress controller to route traffic to a sample application.

### Step 1: Set the Node IP Address

First, you will need to get the IP address of your node. The command varies depending on your operating system.

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="macOS">

<Tabs>
<TabItem value="Default" default>

```bash
IP=127.0.0.1
```

</TabItem>
<TabItem value="socket-vmnet Enabled">

```bash
IP=`kubectl get node/lima-rancher-desktop -o jsonpath='{.status.addresses[?(@.type=="ExternalIP")].address}'`
```

</TabItem>
</Tabs>

</TabItem>
<TabItem value="Linux">

```bash
IP=127.0.0.1
```

</TabItem>
<TabItem value="Windows">

```shell
$IP = (kubectl get node/$env:COMPUTERNAME -o=jsonpath="{range .status.addresses[?(@.type == 'InternalIP')]}{.address}{end}")
```

</TabItem>
</Tabs>

### Step 2: Create a Namespace

Next, create a namespace for the sample application:

```bash
kubectl create ns demo
```

### Step 3: Deploy the Application

Now, deploy a sample `whoami` application, along with a service and an Ingress resource.

> **Note for Linux Users:** Some Linux distributions do not allow non-root users to listen on privileged ports by default. If you encounter any issues, please refer to the documentation on [Traefik port binding access](../getting-started/installation.md#traefik-port-binding-access) for instructions on how to authorize the necessary ports.

<details>
<summary>Click to see the YAML for the `whoami` application</summary>

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: whoami
  name: whoami
spec:
  replicas: 1
  selector:
    matchLabels:
      app: whoami
  template:
    metadata:
      labels:
        app: whoami
    spec:
      containers:
        - image: traefik/whoami:latest
          name: whoami
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: whoami-svc
spec:
  type: ClusterIP
  selector:
    app: whoami
  ports:
    - port: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: whoami-http
  annotations:
    traefik.ingress.kubernetes.io/router.entrypoints: web
spec:
  rules:
    - host: whoami.$IP.sslip.io
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: whoami-svc
                port:
                  number: 80
```

</details>

You can apply this configuration using the following command:

<Tabs groupId="os" defaultValue={TabsConstants.defaultOs}>
<TabItem value="macOS / Linux">

```bash
cat << EOF | kubectl apply -n demo -f -
# Paste the YAML from the section above here
EOF
```

</TabItem>
<TabItem value="Windows">

```shell
echo @"
# Paste the YAML from the section above here
"@ | kubectl apply -n demo -f -
```

</TabItem>
</Tabs>

### Step 4: Verify the Ingress

Verify that the Ingress is working correctly by sending a request to the application using `curl`.

```bash
curl whoami.$IP.sslip.io
```

You should see a response containing information about the `whoami` service.

### Step 5: Clean Up

Once you are finished, you can delete the resources you created:

```bash
kubectl delete all,ingress --all -n demo
```
