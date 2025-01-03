### **Kubernetes (K8s) Production System Management**

#### **Purpose**
Kubernetes (K8s) is an open-source container orchestration platform designed to automate the deployment, scaling, and operation of application containers. DevOps engineers manage K8s clusters in production environments to ensure high availability, scalability, and efficient resource utilization.

---

### **Key Topics**

#### 1. **Lifecycle Management of Kubernetes Clusters**
   - **Creation**: 
     Setting up the infrastructure, deploying nodes (master and workers), and configuring K8s components.
   - **Upgradation**: 
     Keeping the cluster updated with the latest K8s version to ensure security and access to new features.
   - **Deletion**: 
     Decommissioning a cluster safely by deleting resources and cleaning up dependencies like storage and networking.

   **Tools for Lifecycle Management**:
   - **KOPS**: Automates cluster creation and upgrades (mainly on AWS).
   - **Managed Services**: AWS EKS, OpenShift, Tanzu, Rancher.

#### 2. **K8s Distributions**
   - **Amazon EKS**: Managed Kubernetes service on AWS.
   - **OpenShift**: Red Hat's Kubernetes distribution with enterprise features.
   - **Tanzu**: VMware's Kubernetes offering with additional cloud-native features.
   - **Rancher**: Simplifies K8s management for multiple clusters.

   **Example**:  
   If an issue arises on AWS EKS (e.g., pod scheduling errors), troubleshooting involves:
   - Checking node health and logs.
   - Verifying IAM roles and permissions.
   - Using `kubectl` to debug pod failures.

#### 3. **Differences**
   - **K8s vs Minikube**:  
     Minikube is a local K8s environment for testing and learning; not suitable for production.
   - **K8s vs EKS**:  
     EKS is a fully managed Kubernetes service, whereas vanilla Kubernetes requires setting up and managing control planes manually.

#### 4. **Managing Multiple Clusters in Production**
   Managing hundreds of clusters requires automation and centralized control using tools like:
   - **KOPS** for cluster setup and scaling.
   - **Rancher** for multi-cluster management.
   - **Terraform** for infrastructure as code.
   - **Prometheus and Grafana** for monitoring.

---

### **Hands-On: Kubernetes Cluster Installation Using KOPS**

#### **Prerequisites**
1. **Dependencies**:
   - Python3
   - AWS CLI
   - Kubectl
2. **Install Dependencies**:
   ```bash
   curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
   echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
   sudo apt-get update
   sudo apt-get install -y python3-pip apt-transport-https kubectl
   pip3 install awscli --upgrade
   export PATH="$PATH:/home/ubuntu/.local/bin/"
   ```

3. **Install KOPS**:
   ```bash
   curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
   chmod +x kops-linux-amd64
   sudo mv kops-linux-amd64 /usr/local/bin/kops
   ```

#### **Setup IAM Permissions**
   Assign the following permissions to the IAM user:
   - AmazonEC2FullAccess
   - AmazonS3FullAccess
   - IAMFullAccess
   - AmazonVPCFullAccess

#### **AWS CLI Configuration**
   ```bash
   aws configure
   ```

#### **Cluster Installation**
1. **Create an S3 Bucket**:
   ```bash
   aws s3api create-bucket --bucket kops-abhi-storage --region us-east-1
   ```

2. **Create the Cluster**:
   ```bash
   kops create cluster \
   --name=demok8scluster.k8s.local \
   --state=s3://kops-abhi-storage \
   --zones=us-east-1a \
   --node-count=1 --node-size=t2.micro \
   --master-size=t2.micro \
   --master-volume-size=8 --node-volume-size=8
   ```

3. **Edit Cluster Configuration**:
   ```bash
   kops edit cluster demok8scluster.k8s.local
   ```

4. **Build the Cluster**:
   ```bash
   kops update cluster demok8scluster.k8s.local --yes --state=s3://kops-abhi-storage
   ```

5. **Validate the Cluster**:
   ```bash
   kops validate cluster demok8scluster.k8s.local
   ```

---

### **Kubernetes-Zero-to-Hero Repository**
A repository for beginners aiming to simplify Kubernetes learning and implementation:
- Step-by-step guides for setting up clusters.
- Real-world examples to troubleshoot issues.
- Beginner-friendly resources for hands-on practice.

--- 

### **Conclusion**
Managing Kubernetes clusters involves a mix of manual intervention and automation using tools like KOPS, EKS, and Rancher. By understanding the lifecycle, differences between variants, and practical implementation steps, DevOps engineers ensure scalable and efficient Kubernetes environments in production.
