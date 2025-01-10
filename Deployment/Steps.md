### Project Steps to Deploy and Manage a Kubernetes Application 

### **Step 1: Create a Deployment YAML File**
1. **Create a file named `deployment.yaml`:**
   This file defines the Kubernetes Deployment, including the configuration for the Pods that will run in your cluster.

```yaml
apiVersion: apps/v1  # Specifies that the Deployment is part of the 'apps' API group
kind: Deployment     # Defines this resource as a Deployment
metadata:
  name: myapp        # The name of the deployment (can be anything)
spec:
  replicas: 3        # Number of Pods to run
  selector:
    matchLabels:
      app: myapp     # Match Pods with label 'app: myapp'
  template:
    metadata:
      labels:
        app: myapp   # Assign label 'app: myapp' to each Pod
    spec:
      containers:
      - name: nginx   # The name of the container
        image: nginx  # The Docker image to use for the container
        ports:
        - containerPort: 80  # Expose port 80 on the container
```

2. **Apply the Deployment using kubectl**:
   Use the following command to apply the deployment configuration:
   ```bash
   kubectl apply -f deployment.yaml
   ```

---

### **Step 2: Verify Resources**
1. **Check the status of all resources**:
   Use this command to see all resources running in the cluster:
   ```bash
   kubectl get all
   ```

2. **Check the Pods specifically**:
   ```bash
   kubectl get pods -o wide
   ```

   **Expected Output:**
   ```bash
   NAME                    READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
   myapp-5d6b777b8-dn6sw   1/1     Running   0          2m7s    10.244.0.8   minikube   <none>           <none>
   myapp-5d6b777b8-ff2fb   1/1     Running   0          4m14s   10.244.0.7   minikube   <none>           <none>
   myapp-5d6b777b8-pwj8m   1/1     Running   0          4m14s   10.244.0.6   minikube   <none>           <none>
   ```

---

### **Step 3: Test Access to the Application**
1. **Access the Pods to verify the application is running**:
   SSH into the Minikube VM and curl the Pod's IP address:
   ```bash
   minikube ssh
   curl 10.244.0.6
   ```

   **Expected Output:**
   ```html
   <!DOCTYPE html>
   <html>
   <head>
   <title>Welcome to nginx!</title>
   </head>
   <body>
   <p>If you see this page, the nginx web server is successfully installed and working. Further configuration is required.</p>
   <p>Thank you for using nginx.</p>
   </body>
   </html>
   ```

2. **Exit Minikube SSH session**:
   ```bash
   logout
   ```

---

### **Step 4: Delete and Observe ReplicaSet and Pod Behavior**
1. **Delete a Pod**:
   Use the following command to delete one of the Pods:
   ```bash
   kubectl delete pod myapp-5d6b777b8-pwj8m
   ```

2. **Watch Pod Status**:
   Use the following command to continuously watch the status of Pods:
   ```bash
   kubectl get pods -w
   ```

   **Expected Output**:
   After deleting the Pod, a new Pod will be created automatically by the ReplicaSet:
   ```bash
   NAME                    READY   STATUS              RESTARTS   AGE
   myapp-5d6b777b8-ff2fb   1/1     Running             0          7m1s
   myapp-5d6b777b8-pcf42   0/1     ContainerCreating   0          12s
   ```

3. **Verify all Pods**:
   After a new Pod is created, you can verify the updated list of Pods:
   ```bash
   kubectl get pods
   ```

   **Expected Output**:
   ```bash
   myapp-5d6b777b8-dn6sw   1/1     Running   0          5m36s   10.244.0.8   minikube   <none>           <none>
   myapp-5d6b777b8-ff2fb   1/1     Running   0          7m43s   10.244.0.7   minikube   <none>           <none>
   myapp-5d6b777b8-pcf42   1/1     Running   0          54s     10.244.0.9   minikube   <none>           <none>
   ```

---

### **Step 5: Clean Up the Deployment**
1. **Delete the Deployment**:
   If you want to remove the Deployment and all related resources (such as Pods), use the following command:
   ```bash
   kubectl delete deployment myapp
   ```

   **Expected Output**:
   ```bash
   deployment.apps "myapp" deleted
   ```

---

### **Step 6: Debugging Kubernetes Resources**
1. **Describe the Pod for more details**:
   Use this command to get detailed information about a specific Pod:
   ```bash
   kubectl describe pod <pod-name>
   ```

2. **View the logs of a Pod**:
   Use this command to view the logs of a Pod:
   ```bash
   kubectl logs <pod-name>
   ```

   This can help you troubleshoot any issues that arise with the containers.

---

### **Conclusion**
- You have successfully deployed a basic Kubernetes application using a Deployment and verified its functionality.
- You’ve learned how to manage Pods and ReplicaSets and handled failures by observing the automatic recreation of Pods by the ReplicaSet.
- You can also troubleshoot issues using `kubectl describe` and `kubectl logs`.

By following these steps, you can easily replicate the process and explore further Kubernetes features and resource management techniques.
