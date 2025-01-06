### Project Name: **Kubernetes Pod Deployment on Minikube**

### Prerequisites:
1. **Install kubectl**:
   - Follow the installation guide for kubectl based on your operating system:
     - **Windows**: [Install kubectl on Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/#install-kubectl-binary-on-windows-via-direct-download-or-curl)
     - **macOS/Linux**: [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
   
2. **Install Minikube**:
   - Minikube is a tool that allows you to run Kubernetes clusters locally.
   - Download and install Minikube:
     - **Windows**: [Minikube Installation](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download)
     - **macOS/Linux**: [Minikube Installation](https://minikube.sigs.k8s.io/docs/start/)
   
### Steps to Create and Practice the Project:

1. **Check Versions**:
   - Verify if `kubectl` and `minikube` are installed correctly:
     ```bash
     kubectl version --client
     minikube version
     ```

2. **Start Minikube Cluster**:
   - By default, Minikube will use the Docker driver to create the virtual machine and start the Kubernetes cluster.
   - To start the Minikube cluster, run:
     ```bash
     minikube start
     ```
   - Optionally, you can specify memory and the driver (e.g., `hyperv`, `virtualbox`):
     ```bash
     minikube start --memory=4096 --driver=virtualbox
     ```

3. **Check Cluster Status**:
   - Verify that the Kubernetes cluster is up and running:
     ```bash
     kubectl get nodes
     ```

4. **Create the Pod**:
   - In the repository, you will find the `pod.yml` file. Ensure that it is correctly defined.
   - To create the Pod, run:
     ```bash
     kubectl create -f pod.yml
     ```

5. **Check Pod Status**:
   - To check if the Pod is running, execute:
     ```bash
     kubectl get pods
     ```

6. **Get Pod Details**:
   - For more details about the Pod, run:
     ```bash
     kubectl get pods -o wide
     ```

7. **Access the Application**:
   - To access the application inside the Pod, first log in to the Kubernetes cluster using Minikube SSH:
     ```bash
     minikube ssh
     ```
   - Then, use `curl` to check if the application is running by accessing the Pod's IP address:
     ```bash
     curl <POD_IP>
     ```

8. **Delete the Pod**:
   - To delete the Pod, use the following command:
     ```bash
     kubectl delete pod <pod-name>
     ```

9. **Debug the Pod**:
   - To debug the Pod, use the following commands:
     - **Describe the Pod**:
       ```bash
       kubectl describe pod <pod-name>
       ```
     - **View Pod Logs**:
       ```bash
       kubectl logs <pod-name>
       ```

### Summary
This project demonstrates how to create a Kubernetes Pod using Minikube. By following the steps above, you can set up a local Kubernetes environment, deploy a Pod, and manage its lifecycle. You can also troubleshoot and debug applications running inside the Pod using Kubernetes commands. 

