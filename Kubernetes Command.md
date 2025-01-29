🌟 Essential Kubernetes Troubleshooting Commands for DevOps Engineers

1. Pod Management and Debugging

✅ kubectl get pods --all-namespaces

Check the statuses of all pods across namespaces.

✅ kubectl describe pod <pod_name>

Dive into detailed pod configurations and events.

✅ kubectl logs <pod_name> -c <container_name>

View logs of a specific container to diagnose runtime issues.

✅ kubectl exec -it <pod_name> -- /bin/sh

Access a container's shell for deeper debugging.

✅ kubectl delete pod <pod_name> --grace-period=0 --force

Forcefully remove stuck or crashed pods.

✅ kubectl debug <pod_name>

Spin up an ephemeral container to troubleshoot issues directly.

2. Node Management

🖥️ kubectl get nodes

Check the health and status of all cluster nodes.

🖥️ kubectl drain <node_name> --ignore-daemonsets

Evacuate pods safely during node maintenance.

🖥️ kubectl cordon <node_name>

Mark a node as unschedulable to prevent new pod scheduling.

🖥️ kubectl uncordon <node_name>

Revert a node back to schedulable status.

🖥️ kubectl delete node <node_name>

Remove failed nodes from the cluster.

🖥️ kubectl taint nodes <node_name> key=value:NoSchedule

Use taints to block pod scheduling on a problematic node.

3. Monitoring and Resource Management

📊 kubectl top nodes

Monitor node resource usage and identify bottlenecks.

📊 kubectl top pods --all-namespaces

Identify resource-heavy pods in the cluster.

📊 kubectl get componentstatuses

Check the health of core components like etcd and kube-scheduler.

4. Deployment and Rollbacks

🔄 kubectl rollout undo deployment <deployment_name>

Quickly roll back to a stable deployment version.

🔄 kubectl apply -f <backup.yaml>

Restore cluster configurations from a backup.

5. Network and Service Debugging

🌐 kubectl get ingress

Verify the statuses of ingress resources.

🌐 kubectl get endpoints <service_name>

Ensure services are connected to the correct endpoints.

🌐 kubectl port-forward <pod_name> <local_port>:<remote_port>

Debug network issues by forwarding a local port to a pod.

🌐 kubectl proxy

Start a proxy to the Kubernetes API for advanced debugging.

6. Events and Configurations

⚙️ kubectl get events --sort-by='.metadata.creationTimestamp'

Review recent events for errors.

⚙️ kubectl describe <resource_type> <resource_name>

Dive deep into the details of any Kubernetes resource.

⚙️ kubectl edit <resource_type> <resource_name>

Make real-time configuration changes for quick fixes.