When we run `kubectl get pods`, we are interacting with the Kubernetes cluster. But what happens behind the scenes? How does `kubectl` retrieve information about the Pods in the cluster? In this article, we'll walk through the internal process of how `kubectl` works and connects with Kubernetes resources like Pods.

### How `kubectl` Connects to the Cluster.

### Step 1: **User Executes the Command**

When you run the command `kubectl get pods`, you're making a request to the Kubernetes cluster to fetch the list of Pods running in a specific namespace (by default, the `default` namespace). The command itself is executed in your terminal or command prompt and is part of the `kubectl` command-line tool, which is a client for interacting with a Kubernetes cluster.

### Step 2: **The kubectl Configuration**

Before executing the command, `kubectl` needs to know how to reach the Kubernetes API server and which cluster to interact with. This is where **kubeconfig** comes into play. A `kubeconfig` file typically resides in `~/.kube/config` (on the client machine) and contains all the necessary details, such as:

- **API server URL**: Where `kubectl` should send the requests (e.g., `https://api-server.example.com`).
- **Authentication credentials**: How to authenticate with the API server (using a token, username/password, or certificate).
- **Context**: Specifies which cluster, namespace, and user configuration `kubectl` should use.

### Step 3: **API Server Communication**

After `kubectl` determines the correct API server from the kubeconfig, it sends an HTTP/HTTPS request to the Kubernetes **API server** to retrieve the Pods. The request looks like this:

```
GET /api/v1/namespaces/default/pods

```

This request is made via **HTTP** or **HTTPS**, depending on the configuration, and the URL is specified in the kubeconfig.

- **API Server**: The central management point for all Kubernetes operations. It receives requests, processes them, and returns responses. The API server is a RESTful interface for interacting with cluster resources like Pods, Services, Deployments, etc.
- The API server authenticates the request, checks the permissions of the user (via RBAC - Role-Based Access Control), and then retrieves the requested resource.

### Step 4: **The API Server Retrieves Information**

Once the request reaches the API server, it:

- Looks for the list of Pods in the `default` namespace.
- Queries the **etcd** database, which stores all the configuration data and state information of the Kubernetes cluster, including details about all Pods.

### Step 5: **API Server Returns the Response**

After retrieving the necessary information from `etcd`, the API server returns the list of Pods in a structured JSON format:

```json
{
  "kind": "PodList",
  "items": [
    {
      "metadata": {
        "name": "pod-1",
        "namespace": "default"
      },
      "status": "Running"
    },
    {
      "metadata": {
        "name": "pod-2",
        "namespace": "default"
      },
      "status": "Running"
    }
  ]
}

```

The API server sends this response back to the `kubectl` tool.

### Step 6: **kubectl Displays the Output**

`kubectl` then parses the JSON response and presents the list of Pods in a human-readable format in your terminal:

```
NAME      STATUS
pod-1     Running
pod-2     Running

```

At this point, the `kubectl` command has completed the process, and you can see the status of the Pods in your cluster.

### Diagram: How `kubectl get pods` Works

Below is a high-level diagram showing the flow of communication between `kubectl`, the API server, and the Pods:

```
+--------------------+        +-------------------+        +-------------------+
|                    |        |                   |        |                   |
|   User Executes     |        |    kubectl        |        |   Kubernetes API  |
|   'kubectl get     +------> |   Sends Request   +------> |    Server         |
|   pods' Command     |        |   to API Server   |        |                   |
|                    |        |                   |        |                   |
+--------------------+        +-------------------+        +-------------------+
                                                                 |
                                                                 |
                                                                 v
                                                       +---------------------+
                                                       |     etcd Database   |
                                                       |  (Stores Cluster    |
                                                       |  State and Resources|
                                                       +---------------------+
                                                                 |
                                                                 v
                                                       +---------------------+
                                                       | API Server Returns  |
                                                       |    Response with    |
                                                       |      Pod List       |
                                                       +---------------------+
                                                                 |
                                                                 v
                                                        +----------------------+
                                                        |   kubectl Displays   |
                                                        |   Pod Information    |
                                                        |   on Terminal        |
                                                        +----------------------+

```

### Explanation of the Diagram

1. **User Executes the Command**: The user runs `kubectl get pods` in the terminal.
2. **kubectl Sends Request to API Server**: `kubectl` constructs an API request using the kubeconfig and sends it to the API server.
3. **API Server Queries etcd**: The API server fetches information from the `etcd` database where Kubernetes stores the current state of resources like Pods, Deployments, etc.
4. **API Server Returns Response**: After retrieving data from `etcd`, the API server sends the response back to `kubectl`.
5. **kubectl Displays Pod Information**: `kubectl` parses the response and displays the relevant Pod information in the terminal.

### Summary

When you run `kubectl get pods`, `kubectl` communicates with the Kubernetes API server, which fetches data about the Pods from the `etcd` database and returns it to `kubectl`. The `kubectl` tool then formats the response and presents the information in a human-readable format.

This process ensures that `kubectl` abstracts the complexity of interacting directly with Pods, APIs, and the underlying infrastructure, making it a simple yet powerful tool for managing Kubernetes resources.

### Key Takeaways

- **kubeconfig** plays a critical role in specifying how `kubectl` interacts with the cluster.
- The **API server** is the central point for managing cluster resources.
- **etcd** stores the state of the Kubernetes cluster and is queried by the API server to retrieve resource information.
- `kubectl` formats and displays the output in a way that's easy for humans to understand.
