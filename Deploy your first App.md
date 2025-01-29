Deploying your first application in Kubernetes involves several steps, but it’s relatively straightforward once you have your Kubernetes cluster set up. Here's a step-by-step guide to deploying a simple web application, such as **nginx**, using **kubectl**.

### Prerequisites

1. **Kubernetes Cluster**: You should have a working Kubernetes cluster (e.g., Minikube, EKS, GKE, AKS, etc.). If you're using Minikube, make sure it's running.
2. **kubectl**: Ensure that `kubectl` is configured to communicate with your cluster.
3. **Container Image**: For this example, we will use the **nginx** container image. You can replace this with any other containerized application if needed.

---

### Step 1: Create a Deployment

A **Deployment** is a Kubernetes resource that defines how an application should run. It specifies the container image, the number of replicas, and other settings.

Run the following command to create a simple **nginx** deployment:

```bash
kubectl create deployment nginx --image=nginx

```

Explanation:

- `kubectl create deployment`: The command to create a new deployment.
- `nginx`: The name of the deployment.
- `-image=nginx`: Specifies the Docker image for the deployment. In this case, it's the official `nginx` image.

### Step 2: Expose the Deployment as a Service

Once the deployment is created, you need to expose it to allow external access. In Kubernetes, **Services** provide a stable endpoint for accessing deployed applications.

Run the following command to expose your **nginx** deployment as a service:

```bash
kubectl expose deployment nginx --type=NodePort --port=80

```

Explanation:

- `kubectl expose deployment`: The command to create a service for the deployment.
- `nginx`: The name of the deployment you want to expose.
- `-type=NodePort`: Exposes the service externally using a port on the node (your machine in this case).
- `-port=80`: The port on which the nginx service will be available inside the container. This is typically HTTP traffic.

### Step 3: Get the External IP (If Using a Cloud Provider)

If you're using a cloud provider like GKE, EKS, or AKS, Kubernetes will provision an external load balancer for the service and give you an external IP address. You can use the following command to retrieve the external IP of your service:

```bash
kubectl get services

```

Look for the **EXTERNAL-IP** column. It may take a minute or two for the external IP to be assigned. For now, proceed to the next step to check the status.

If you're using **Minikube** or a local cluster, you can use **`minikube service`** to get the URL of your service:

```bash
minikube service nginx

```

This will open the default web browser with the URL of your service.

### Step 4: Verify the Deployment and Service

To verify that your application is running, use the following commands:

- **Check the status of the pods**:
    
    ```bash
    kubectl get pods
    
    ```
    
    This command should show the `nginx` pod running.
    
- **Check the service status**:
    
    ```bash
    kubectl get services
    
    ```
    
    This command will display the service you just exposed. You should see the **nginx** service with a **NodePort** (if you're using Minikube or running locally) or an **External IP** (if using cloud providers).
    

### Step 5: Access the Application

Now that your service is exposed, open a browser and navigate to the following URL to access your deployed nginx application:

- **If using Minikube**:
    
    ```bash
    minikube service nginx
    
    ```
    
- **If using a cloud provider (GKE, EKS, AKS)**, use the EXTERNAL-IP from the `kubectl get services` command:
    
    ```
    http://<EXTERNAL-IP>:<NodePort>
    
    ```
    

You should see the default **nginx** welcome page, which confirms that your application is successfully deployed.

### Step 6: Scale the Application (Optional)

You can scale your deployment to increase the number of replicas (pods) running your application. This can help ensure high availability and load balancing.

To scale the nginx deployment to 3 replicas, run:

```bash
kubectl scale deployment nginx --replicas=3

```

Then, check the status of the pods again with:

```bash
kubectl get pods

```

You should see 3 nginx pods running.

### Step 7: Clean Up (Optional)

When you're done and want to remove the application, you can delete the deployment and the service with the following commands:

- Delete the **deployment**:
    
    ```bash
    kubectl delete deployment nginx
    
    ```
    
- Delete the **service**:
    
    ```bash
    kubectl delete service nginx
    
    ```
    

---

### Summary of Commands:

1. **Create the nginx deployment**:
    
    ```bash
    kubectl create deployment nginx --image=nginx
    
    ```
    
2. **Expose the deployment as a service**:
    
    ```bash
    kubectl expose deployment nginx --type=NodePort --port=80
    
    ```
    
3. **View the status of your resources**:
    
    ```bash
    kubectl get pods
    kubectl get services
    
    ```
    
4. **Scale the deployment (optional)**:
    
    ```bash
    kubectl scale deployment nginx --replicas=3
    
    ```
    
5. **Delete the deployment and service (optional)**:
    
    ```bash
    kubectl delete deployment nginx
    kubectl delete service nginx
    
    ```
    

---

### Conclusion

Congratulations! You’ve just deployed your first application in Kubernetes. You’ve learned how to create a deployment, expose it via a service, and even scale it. This is the foundation for working with Kubernetes, and you can build on this knowledge by deploying more complex applications and utilizing more advanced Kubernetes features.