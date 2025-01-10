### **Project Name: Kubernetes Service Exposure and Load Balancing**

### **Project Summary:**

In this project, we will explore and practice various Kubernetes networking concepts, including **service exposure**, **service discovery**, and **load balancing**. The goal is to understand how applications are accessed within and outside a Kubernetes cluster, how service discovery works, and how Kubernetes distributes traffic across multiple replicas of an application.

#### **Key Concepts Covered:**
1. **Service Exposure**:
   - We will create a Kubernetes **Deployment** and expose it using three different types of services: **ClusterIP**, **NodePort**, and **LoadBalancer**. These services will allow us to access the application both within the cluster and externally, depending on the type of service.
   
2. **Service Discovery**:
   - By modifying the **selector** in the service configuration, we will explore how Kubernetes services discover and route traffic to pods. This will help in understanding how services rely on label selectors for traffic routing.

3. **Load Balancing**:
   - Using multiple replicas of the application, we will observe how Kubernetes load balances the traffic across different pods. We will also visualize the traffic flow using the **Kubeshark** tool, which will help us understand the round-robin load balancing method employed by Kubernetes.

---
# STEPS

---

### **1. Accessing the Application using Different Types of NodePorts**

In this section, we will explore how to expose your application using three different service types: **ClusterIP**, **NodePort**, and **LoadBalancer**. Each of these service types serves a different purpose for exposing your app within and outside the Kubernetes cluster.

#### **Steps:**

1. **Create a Kubernetes Cluster**:
   - If you don't have a Kubernetes cluster already, create one using tools like **Minikube**, **Kind**, or use cloud providers like **AWS EKS**, **Google GKE**, or **Azure AKS**.
   - Example (Minikube):
     ```bash
     minikube start
     ```

2. **Write the `deployment.yaml` File for the Application**:
   - Create a deployment that defines the app and its replicas.

3. **Create the Deployment**:
   - Apply the `deployment.yaml` file to create the deployment.
   ```bash
   kubectl apply -f deployment.yaml
   ```

4. **Expose the Application Using ClusterIP**:
   - Create a service to expose the application internally within the cluster using **ClusterIP** (default).
   
   - Apply the service:
     ```bash
     kubectl apply -f service.yaml
     ```
   - **Access**: This service will be accessible only within the cluster. You cannot access it from outside the cluster.

5. **Expose the Application Using NodePort**:
   - Change the service type to **NodePort** to expose the service on a specific port on each node.
   
   - Apply the service:
     ```bash
     kubectl apply -f service.yaml
     ```
   - **Access**: You can access the service using the node's IP and the specified node port, like `http://<node-ip>:30007`. This is typically used for development or testing environments.

6. **Expose the Application Using LoadBalancer**:
   - Update the service to use a **LoadBalancer** type, which exposes the service to the outside world with an external IP.
   
   - Apply the service:
     ```bash
     kubectl apply -f service.yaml
     ```
   - **Access**: You can access the application using the external IP address assigned to the service:
     ```bash
     kubectl get svc myapp-service
     ```
   - The external IP will be listed once it is provisioned, and you can use it to access your app from outside the cluster.

---

### **2. Service Discovery**

In this section, we will focus on how Kubernetes services discover and communicate with pods.

#### **Steps:**

1. **Create a Service** (ClusterIP, NodePort, or LoadBalancer):
   - Follow the steps from the **Accessing the Application** section to create a service. You will need to expose your application via a service first.

2. **Change the Selector Name in the Service**:
   - In the `service.yaml` file, change the `selector` field to an incorrect label name (for example, change `app: myapp` to `app: wrongapp`).
   - Example:
     ```yaml
     selector:
       app: wrongapp
     ```
   - Apply the change:
     ```bash
     kubectl apply -f service.yaml
     ```

3. **Observe the Effect on Service Discovery**:
   - The service will no longer find any pods because the selector does not match any of the existing pods. Therefore, the service will not be able to route traffic to any pods, and the application will be inaccessible.

4. **Correct the Selector**:
   - Fix the selector back to the correct label name, which is `app: myapp` in this case.
   - Example:
     ```yaml
     selector:
       app: myapp
     ```
   - Apply the correction:
     ```bash
     kubectl apply -f service.yaml
     ```

5. **Verify Service Discovery**:
   - After correcting the selector, try accessing the application again. The service should now be able to find the pods and route traffic successfully.

---

### **3. Load Balancing**

In this section, we will explore how Kubernetes load balancing works for services with multiple replicas.

#### **Steps:**

1. **Deploy Multiple Replicas**:
   - Ensure that your deployment has multiple replicas. For example, you can set the `replicas` field in your `deployment.yaml` to 2 or more.
   - Example `deployment.yaml`:
     ```yaml
     spec:
       replicas: 2
     ```

2. **Expose the Application Using a Service**:
   - Expose the application using a **ClusterIP**, **NodePort**, or **LoadBalancer** as explained earlier in the Access section.

3. **Test Load Balancing**:
   - If your service has multiple pods (replicas), Kubernetes will distribute the traffic between the available pods using round-robin load balancing.
   - Access the application repeatedly (e.g., by refreshing the page or making multiple requests).
   - **Expected Behavior**: 
     - First request → Pod A (replica 1)
     - Second request → Pod B (replica 2)
     - Third request → Pod A (replica 1)
     - Fourth request → Pod B (replica 2)

4. **Monitor Traffic Flow Using Kubeshark**:
   - Install **Kubeshark** to visualize how the traffic is being routed to the pods.
   - Follow the instructions to install Kubeshark:
     ```bash
     kubectl apply -k github.com/kubeshark/kubeshark/kustomize/default
     ```
   - Once Kubeshark is running, you can view the real-time traffic distribution between pods and observe the round-robin load balancing behavior.

---

### **Conclusion**

This project will give you a good understanding of how to:
- Expose applications using different types of services (`ClusterIP`, `NodePort`, `LoadBalancer`).
- Explore service discovery and how selectors work to route traffic to pods.
- Understand Kubernetes load balancing and how traffic is distributed among multiple replicas.

By completing each of these steps, you will have a solid foundation in Kubernetes networking concepts, including how services are exposed, how traffic flows, and how Kubernetes handles load balancing.
