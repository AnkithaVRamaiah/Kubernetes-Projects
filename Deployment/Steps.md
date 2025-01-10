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

Here are the steps you followed for practicing a deployment project using Kubernetes, with an explanation of each step and the expected output:

### 1. **Verify the Kubernetes Cluster**
   Command:
   ```
   kubectl get all
   ```
   - **Explanation**: This command shows all the resources in the Kubernetes cluster, such as services, pods, deployments, etc.
   - **Expected Output**: You should see details about your Kubernetes cluster's resources (e.g., services, pods, etc.). The output you've received shows only the default Kubernetes service (`kubernetes`) running, which is expected since you haven’t created any other resources yet.

   ```
   NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)
   service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP
   ```

### 2. **Apply Deployment Configuration**
   Command:
   ```
   kubectl apply -f <your-deployment-file>.yaml
   ```
   - **Explanation**: This command applies a deployment from the YAML configuration file. It creates a deployment and its associated resources (like Pods, ReplicaSets).
   - **Expected Output**: You should see a confirmation message indicating the creation of the deployment.
   ```
   deployment.apps/myapp created
   ```

### 3. **Check Deployment Status**
   Command:
   ```
   kubectl get deployment
   ```
   - **Explanation**: This command shows the status of the deployment, including how many pods are running and the number of replicas.
   - **Expected Output**: The output shows that the `myapp` deployment is created and 2 pods are running.
   ```
   NAME                    READY   UP-TO-DATE   AVAILABLE   AGE       
   deployment.apps/myapp   2/2     2            2           99s       
   ```

### 4. **Check Pods Status**
   Command:
   ```
   kubectl get pods
   ```
   to see details like ipaddress
   ```
   kubectl get pods -o wide
   ```
   - **Explanation**: This command shows the pods created as part of the deployment.
   - **Expected Output**: You will see the pods for `myapp`, and their status will show as "Running."
   ```
   NAME                    READY   STATUS    RESTARTS   AGE
   myapp-5d6b777b8-4qj2d   1/1     Running   0          3m45s
   myapp-5d6b777b8-h9gc7   1/1     Running   0          3m44s
   ```

### **Access Minikube's VM (SSH)**:
   
     ```bash
     minikube ssh
     ```
     
     ```bash
     curl 10.244.0.4
     ```
   - **Expected Output**:
     ```html
     <!DOCTYPE html>
     <html>
     <head>
     <title>Welcome to nginx!</title>
     ...
     </html>
     ```

 **Logout from Minikube's VM**:
   ```bash
   logout
   ```
### 5. **Delete a Pod Manually**
   Command:
   ```
   kubectl delete pod myapp-5d6b777b8-4qj2d
   ```
   - **Explanation**: This command manually deletes a specific pod.
   - **Expected Output**: You will see that the pod `myapp-5d6b777b8-4qj2d` is deleted.
   ```
   pod "myapp-5d6b777b8-4qj2d" deleted
   ```

### 6. **Watch the Pod Recreation**
   Command:
   ```
   kubectl get pods -w
   ```
   - **Explanation**: The `-w` flag is used to watch the pod status in real-time. Kubernetes automatically creates a new pod to replace the deleted one based on the deployment configuration.
   - **Expected Output**: You will see the pod deletion and the new pod creation in real-time. The newly created pod (`myapp-5d6b777b8-jw96n`) will eventually go to the "Running" state.
   ```
   NAME                    READY   STATUS     RESTARTS   AGE
   myapp-5d6b777b8-4qj2d   1/1     Running    0          3m24s
   myapp-5d6b777b8-h9gc7   1/1     Running    0          3m23s
   myapp-5d6b777b8-4qj2d   0/1     Completed  0          4m9s
   myapp-5d6b777b8-jw96n   1/1     Running    0          2s
   ```

### 7. **Delete the Deployment**
   Command:
   ```
   kubectl delete deploy myapp
   ```
   - **Explanation**: This command deletes the entire deployment, which includes deleting the associated pods and replica sets.
   - **Expected Output**: The deployment `myapp` will be deleted successfully.
   ```
   deployment.apps "myapp" deleted
   ```

### 8. **Debugging Kubernetes Resources**
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


### Summary of Outputs:
1. **`kubectl get all`** shows the Kubernetes service.
2. **`kubectl apply -f <file>`** creates the deployment.
3. **`kubectl get deployment`** confirms the deployment with running pods.
4. **`kubectl get pods`** lists the pods and their status.
5. **`kubectl delete pod <pod-name>`** deletes a pod.
6. **`kubectl get pods -w`** watches the pod recreation process.
7. **`kubectl delete deploy <deployment-name>`** deletes the deployment and all related resources.

This workflow helps in understanding Kubernetes deployments, pod management, and the self-healing nature of Kubernetes, as well as how to handle the creation, deletion, and replacement of resources.
