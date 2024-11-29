# Deploy a k8s cluster using Minikube

Deploying a Kubernetes cluster using **Minikube** is a great way to set up a local Kubernetes environment for testing and development. Minikube runs a single-node Kubernetes cluster inside a virtual machine (VM) or container on your local machine. Here’s a step-by-step guide to deploying a Kubernetes cluster using Minikube.

### Prerequisites

Before starting, ensure you have the following prerequisites installed:

1. **Minikube**: A tool for running Kubernetes clusters locally.
2. **kubectl**: The command-line tool for interacting with a Kubernetes cluster.
3. **Virtualization Software**: Minikube requires a hypervisor (e.g., VirtualBox, HyperKit, or Docker) to run the cluster inside a VM or container.

### Step 1: Install Minikube

You can install Minikube on different platforms, such as macOS, Windows, and Linux. Choose the appropriate installation method for your operating system.

### macOS:

```bash
brew install minikube

```

### Windows:

On Windows, you can use Chocolatey:

```bash
choco install minikube

```

Alternatively, you can download the installer directly from the [Minikube releases page](https://github.com/kubernetes/minikube/releases).

### Linux:

For Linux, use the following commands:

```bash
curl -Lo minikube <https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64>
chmod +x minikube
sudo mv minikube /usr/local/bin/

```

### Step 2: Install kubectl

You can install `kubectl` via package managers or by downloading the binary directly.

### macOS:

```bash
brew install kubectl

```

### Windows:

```bash
choco install kubernetes-cli

```

### Linux:

```bash
curl -LO "<https://storage.googleapis.com/kubernetes-release/release/v1.26.0/bin/linux/amd64/kubectl>"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

```

### Step 3: Start Minikube

Once you have both **Minikube** and **kubectl** installed, you can start Minikube to create your local Kubernetes cluster.

Run the following command to start Minikube:

```bash
minikube start

```

By default, Minikube will start a VM using VirtualBox or the default hypervisor on your system. If you have Docker installed, you can use Docker as the driver for Minikube with the following command:

```bash
minikube start --driver=docker

```

The `minikube start` command will download the necessary Kubernetes components (like the API server, controller manager, etc.) and set up the Kubernetes cluster. It will also configure `kubectl` to communicate with your local cluster.

### Step 4: Verify the Cluster

Once the cluster has started, you can verify that Minikube is running and kubectl is configured correctly:

```bash
kubectl cluster-info

```

This should display the URL for the Kubernetes control plane and other services running within the cluster.

You can also check the status of your Minikube cluster:

```bash
minikube status

```

### Step 5: Use kubectl to Interact with the Cluster

Now that your Kubernetes cluster is up and running, you can use `kubectl` to interact with the cluster. For example, you can check the nodes in your cluster:

```bash
kubectl get nodes

```

This should return the information for the single Minikube node (since it’s a single-node cluster).

### Step 6: Deploy an Example Application

Let’s deploy a simple application to the Minikube cluster. For example, you can deploy a basic nginx web server:

1. Create a deployment for the nginx server:
    
    ```bash
    kubectl create deployment nginx --image=nginx
    
    ```
    
2. Expose the nginx deployment as a service:
    
    ```bash
    kubectl expose deployment nginx --type=NodePort --port=80
    
    ```
    
3. Check the service to get the port where it's exposed:
    
    ```bash
    kubectl get services
    
    ```
    
    You should see the `nginx` service listed with a `NodePort` type. The external port for this service will be listed in the **PORT(S)** column.
    
4. Access the application by opening a browser and navigating to the following URL:
    
    ```
    http://<minikube_ip>:<node_port>
    
    ```
    
    To get the Minikube IP address:
    
    ```bash
    minikube ip
    
    ```
    
    The `node_port` can be found by running:
    
    ```bash
    kubectl get services
    
    ```
    
    The service should be accessible in your web browser, showing the default nginx page.
    

### Step 7: Stop Minikube

Once you are done with your local Kubernetes cluster, you can stop Minikube to free up resources:

```bash
minikube stop

```

This will stop the Minikube VM or container and shut down the Kubernetes cluster.

### Step 8: Delete Minikube Cluster (Optional)

If you want to delete the Minikube cluster and free up disk space, use the following command:

```bash
minikube delete

```

This will delete the VM/container and remove all associated Kubernetes resources.

---

### Conclusion

Minikube is an excellent tool for quickly setting up a local Kubernetes cluster for development, testing, and learning. With just a few simple commands, you can have a single-node Kubernetes cluster running on your machine, allowing you to experiment with Kubernetes features and deploy applications locally.