---
title: Setup NGINX Ingress Controller
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/how-to-guides/setup-NGINX-Ingress-Controller"/>
</head>

Rancher Desktop uses Traefik as the default Ingress controller for its Kubernetes cluster. However, in some cases, you may want to use the NGINX Ingress controller instead. This guide will walk you through the process of disabling Traefik and setting up the NGINX Ingress controller for a sample application.

### Step 1: Disable Traefik

In the Rancher Desktop UI, navigate to the **Kubernetes Settings** page and uncheck the **Enable Traefik** option. You may need to restart Rancher Desktop for this change to take effect.

### Step 2: Deploy the NGINX Ingress Controller

You can deploy the NGINX Ingress controller using either Helm or `kubectl`.

<Tabs groupId="deployment-approach">
<TabItem value="helm" default>

```bash
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

</TabItem>
<TabItem value="kubectl">

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.2/deploy/static/provider/cloud/deploy.yaml
```

</TabItem>
</Tabs>

### Step 3: Verify the Ingress Controller Pods

Wait for the Ingress controller pods to start by running the following command:

```bash
kubectl get pods --namespace=ingress-nginx
```

### Step 4: Create a Sample Deployment

Next, create a sample NGINX deployment and expose it as a service:

```bash
kubectl create deployment demo --image=nginx --port=80
kubectl expose deployment demo
```

### Step 5: Create an Ingress Resource

Now, create an Ingress resource to expose the sample application. The following command creates a rule that maps `demo.localtest.me` to the `demo` service.

```bash
kubectl create ingress demo-localhost --class=nginx --rule="demo.localtest.me/*=demo:80"
```

### Step 6: Forward a Local Port to the Ingress Controller

Finally, forward a local port to the NGINX Ingress controller to access the application from your local machine:

```bash
kubectl port-forward --namespace=ingress-nginx service/ingress-nginx-controller 8080:80
```

You can now access the sample NGINX application by navigating to `http://demo.localtest.me:8080` in your web browser.
