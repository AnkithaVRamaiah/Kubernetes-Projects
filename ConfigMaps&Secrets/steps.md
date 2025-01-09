### **Project: Managing Configurations with ConfigMap and Secrets in Kubernetes**

In this project, we will manage application configurations using **ConfigMaps** and **Secrets** in Kubernetes. We will demonstrate how to store configuration data in a ConfigMap, update it, and securely handle sensitive data using Secrets. We'll also highlight problems and solutions associated with environment variable management and volume mounts.

---

### **Step-by-Step Implementation**

#### **1. Creating a ConfigMap**

We'll start by creating a **ConfigMap** to store our application’s configuration data, such as the database host and port.

- **Create a `config.map.yaml` file:**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  database_host: "db.example.com"
  database_port: "5432"
```

- **Apply the ConfigMap using `kubectl`:**

```bash
kubectl apply -f config.map.yaml
```

- **Verify the ConfigMap:**

```bash
kubectl get cm
```

---

#### **2. Creating a Deployment**

Next, we'll create a **Deployment** that uses the ConfigMap to configure the environment variables of the containers.

- **Create a `deployment.yaml` file:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-backend-app
        env:
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: database_host
        - name: DB_PORT
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: database_port
```

- **Apply the Deployment:**

```bash
kubectl apply -f deployment.yaml
```

- **Verify the Pods:**

```bash
kubectl get pods
```

---

#### **3. Accessing the Environment Variables**

To check the environment variables inside the running container, use the following commands:

- **Execute into the Pod:**

```bash
kubectl exec -it <pod-name> -- /bin/bash
```

- **Check the environment variables:**

```bash
env | grep DB_
```

---

#### **4. Updating the ConfigMap**

If you need to change the database port, update the **ConfigMap** and reapply it. However, environment variables in running containers won’t update automatically; you need to recreate the pods or use volume mounts for dynamic updates.

- **Modify the `config.map.yaml` to change the port:**

```yaml
data:
  database_port: "5433"
```

- **Reapply the ConfigMap:**

```bash
kubectl apply -f config.map.yaml
```

- **Recreate the Pods to apply the changes:**

```bash
kubectl delete pod <pod-name>
```

- **Verify the changes:**

```bash
kubectl get pods
kubectl exec -it <new-pod-name> -- /bin/bash
env | grep DB_PORT
```

---

#### **5. Using Volume Mounts for Dynamic Updates**

To avoid restarting the pods for every configuration change, we can use **volume mounts** to map the ConfigMap as a file inside the container.

- **Modify the `deployment.yaml` to include a volume mount:**

```yaml
spec:
  containers:
  - name: my-container
    image: my-backend-app
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
      readOnly: true
  volumes:
  - name: config-volume
    configMap:
      name: app-config
```

- **Apply the updated Deployment:**

```bash
kubectl apply -f deployment.yaml
```

- **Check the mounted files:**

```bash
kubectl exec -it <pod-name> -- /bin/bash
ls /etc/config
cat /etc/config/database_port
```

- **Update the ConfigMap and verify the changes without restarting the pod:**

```bash
kubectl apply -f config.map.yaml
cat /etc/config/database_port
```

---

#### **6. Handling Sensitive Data with Secrets**

Sensitive data like usernames and passwords should be stored in **Secrets**.

- **Create a `secret.yaml` file:**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: bXlfdXNlcg==  # base64 encoded "my_user"
  password: c2VjdXJlX3Bhc3M=  # base64 encoded "secure_pass"
```

- **Apply the Secret:**

```bash
kubectl apply -f secret.yaml
```

- **Verify the Secret:**

```bash
kubectl get secrets
```

- **Decode the Secret value:**

```bash
echo "bXlfdXNlcg==" | base64 --decode
```

---

### **Problems and Solutions**

#### **Problem 1: Environment Variable Updates**
- **Issue:** Environment variables do not update in running containers after changing the ConfigMap.
- **Solution:** Use volume mounts to dynamically update configuration files without restarting the pod.

#### **Problem 2: Managing Sensitive Data**
- **Issue:** Storing sensitive data in ConfigMaps is not secure.
- **Solution:** Use Secrets to store sensitive data, which are encrypted at rest and can be managed using RBAC for access control.

---

### **Conclusion**

This project demonstrates the effective use of **ConfigMaps** and **Secrets** in Kubernetes for managing configuration and sensitive data. By using volume mounts and Secrets, you can dynamically update configurations and enhance the security of your application.