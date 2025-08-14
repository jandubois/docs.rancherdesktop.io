---
title: Working with WebAssembly
---

<head>
  <link rel="canonical" href="https://docs.rancherdesktop.io/tutorials/working-with-webassembly"/>
</head>

Rancher Desktop provides experimental support for running WebAssembly (Wasm) applications. This tutorial will guide you through the process of developing, building, and running a Wasm application with Rancher Desktop.

### Step 1: Enable Wasm Support

To get started, you will need to enable Wasm support in Rancher Desktop:

1.  Navigate to **Preferences > Container Engine > General**.
2.  Check the **Enable Wasm** option.

> **Important:** When using the `moby` container engine, enabling Wasm support will switch to a different image store. Any images you have previously built or downloaded will not be available until you disable Wasm support again.

### Step 2: Create a Wasm Application

This tutorial uses the [Fermyon Spin](https://developer.fermyon.com/spin/v2/install) framework to create a simple "Hello World" application.

1.  **Install Spin:**
    Follow the instructions on the [Spin installation page](https://developer.fermyon.com/spin/v2/install) to install the Spin CLI.

2.  **Create a New Application:**
    Run the `spin new` command and follow the prompts to create a new application. For this example, you can select the `http-ts` template to create a TypeScript-based application.

3.  **Customize the Application Code:**
    In the generated `index.ts` file, modify the `handleRequest` function to return a custom message:

    ```typescript
    import { HandleRequest, HttpRequest, HttpResponse } from "@fermyon/spin-sdk"

    export const handleRequest: HandleRequest = async function (request: HttpRequest): Promise<HttpResponse> {
      return {
        status: 200,
        headers: { "content-type": "text/plain" },
        body: "Hello from Wasm container!"
      }
    }
    ```

4.  **Build the Wasm Module:**
    From the root of your application directory, run the `spin build` command to compile your code into a Wasm module.

    ```bash
    spin build
    ```

    This will create a `.wasm` file in the `target` directory.

### Step 3: Package and Push the Wasm Module

Next, you will package your Wasm module as an OCI container image and push it to a container registry.

1.  **Create a `Dockerfile`:**
    Create a new file named `Dockerfile` with the following content:

    ```dockerfile
    FROM scratch
    COPY spin.toml /spin.toml
    COPY /target/rd-spin-hello-world.wasm /target/rd-spin-hello-world.wasm
    ```

2.  **Build the Container Image:**

    <Tabs groupId="container-runtime">
    <TabItem value="nerdctl" default>

    ```bash
    nerdctl build \
      --namespace k8s.io \
      --platform wasi/wasm \
      -t <YOUR-REGISTRY>/rd-spin-hello-world:0.1.0 .
    ```

    </TabItem>
    <TabItem value="docker">

    ```bash
    docker buildx build \
      --load \
      --platform wasi/wasm \
      --provenance=false \
      -t <YOUR-REGISTRY>/rd-spin-hello-world:0.1.0 .
    ```

    </TabItem>
    </Tabs>

3.  **Push the Image to a Registry:**

    <Tabs groupId="container-runtime">
    <TabItem value="nerdctl" default>

    ```bash
    nerdctl push <YOUR-REGISTRY>/rd-spin-hello-world:0.1.0
    ```

    </TabItem>
    <TabItem value="docker">

    ```bash
    docker push <YOUR-REGISTRY>/rd-spin-hello-world:0.1.0
    ```

    </TabItem>
    </Tabs>

### Step 4: Run the Wasm Application

You can run your Wasm application as a standalone container or as a deployment in Kubernetes.

#### Running as a Standalone Container

> **Note:** Running a Wasm container directly is currently only supported with the `moby` container engine.

Ensure that `dockerd (moby)` is selected as your container engine, and then run the following command:

```bash
docker run \
    --detach \
    --name spin-demo \
    --runtime io.containerd.spin.v2 \
    --platform wasi/wasm \
    --publish 8080:80 \
    <YOUR-REGISTRY>/rd-spin-hello-world:0.1.0 \
    /
```

You can now access your application at `http://localhost:8080`.

#### Running in Kubernetes

> **Note:** Running a Wasm application in Kubernetes is currently only supported with the `containerd` runtime.

Ensure that `containerd` is selected as your container engine, and then apply the following Kubernetes manifests:

<details>
<summary>Click to see the Kubernetes manifests</summary>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-spin
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-spin
  template:
    metadata:
      labels:
        app: hello-spin
    spec:
      runtimeClassName: spin
      containers:
      - name: hello-spin
        image: <YOUR-REGISTRY>/rd-spin-hello-world:0.1.0
        command: ["/"]
---
apiVersion: v1
kind: Service
metadata:
  name: hello-spin
spec:
  type: ClusterIP
  selector:
    app: hello-spin
  ports:
  - port: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-spin
  annotations:
    traefik.ingress.kubernetes.io/router.entrypoints: web
spec:
  rules:
  - host: localhost
    http:
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: hello-spin
              port:
                number: 80
```

</details>

Apply the manifests using `kubectl`:

```bash
kubectl apply -f <your-manifest-file>.yaml
```

You can now access your application at `http://localhost`.

> **Note on Windows:** On Windows, you will need to use the IP address of the Traefik load balancer instead of `localhost`. You can get the IP address by running `kubectl get service traefik --namespace kube-system --output "jsonpath={.status.loadBalancer.ingress[0].ip}"`.
