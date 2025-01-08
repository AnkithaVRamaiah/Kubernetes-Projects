### Step 1: Install Minikube
1. Ensure you have Minikube installed on your machine. You can download and install it from the [Minikube installation guide](https://minikube.sigs.k8s.io/docs/start/).

### Step 2: Start Minikube
```bash
minikube start
```
This command starts a local Kubernetes cluster on Minikube.

### Step 3: Enable the Nginx Ingress Controller
1. Minikube provides a simple way to enable the Nginx Ingress Controller using the following command:
   ```bash
   minikube addons enable ingress
   ```

### Step 4: Verify the Ingress Controller Installation
1. Verify that the Nginx Ingress Controller is running:
   ```bash
   kubectl get pods -n kube-system
   ```
   Look for a pod with a name similar to `nginx-ingress-controller`.

### Step 5: Create a Deployment for Nginx
1. Create a YAML file named `nginx-deployment.yaml` to define the Nginx Deployment:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-deployment
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: nginx
     template:
       metadata:
         labels:
           app: nginx
       spec:
         containers:
         - name: nginx
           image: nginx:latest
           ports:
           - containerPort: 80
   ```

2. Apply the deployment:
   ```bash
   kubectl apply -f nginx-deployment.yaml
   ```

### Step 6: Expose the Nginx Deployment with a Service
1. Create a YAML file named `nginx-service.yaml` for the Nginx Service:
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: nginx-service
   spec:
     selector:
       app: nginx
     ports:
     - protocol: TCP
       port: 80
       targetPort: 80
     type: ClusterIP
   ```

2. Apply the service:
   ```bash
   kubectl apply -f nginx-service.yaml
   ```

### Step 7: Create an Ingress Resource
1. Create a YAML file named `nginx-ingress.yaml` for the Ingress resource:
   ```yaml
   apiVersion: networking.k8s.io/v1
   kind: Ingress
   metadata:
     name: nginx-ingress
     annotations:
       nginx.ingress.kubernetes.io/rewrite-target: /
   spec:
     rules:
     - host: nginx.local
       http:
         paths:
         - path: /
           pathType: Prefix
           backend:
             service:
               name: nginx-service
               port:
                 number: 80
   ```

2. Apply the Ingress resource:
   ```bash
   kubectl apply -f nginx-ingress.yaml
   ```

### Step 8: Add Host Entry
1. Since you're using Minikube, add the host entry to your `/etc/hosts` file to resolve `nginx.local` to Minikube's IP:
   ```bash
   echo "$(minikube ip) nginx.local" | sudo tee -a /etc/hosts
   ```

### Step 9: Access the Nginx Application
1. Open a browser and go to `http://nginx.local` to see the Nginx welcome page.

