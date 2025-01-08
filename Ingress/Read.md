---
# Simplified
---

Before Kubernetes, developers and DevOps engineers deployed applications on virtual machines (VMs) or physical servers. On top of these VMs, they often used load balancers provided by companies like Nginx, F5, and others. These load balancers offered a variety of capabilities, including load balancing, sticky sessions, path-based routing, and host-based routing. 

When Kubernetes came into the picture, it introduced features like auto-scaling, auto-healing, and load balancing for services. However, even though Kubernetes provides load balancing, it uses a round-robin algorithm by default. This basic load balancing isn't as sophisticated as the enterprise-level load balancers offered by third-party providers, and cloud providers charge for using static IP addresses with services like LoadBalancer. This led to some concerns and complaints from users about the lack of advanced load balancing features and the costs involved.

To address these challenges, Kubernetes introduced **Ingress**. Ingress provides a way to manage external access to services within a Kubernetes cluster, offering capabilities like path-based and host-based routing, and it allows for more advanced routing rules that were previously handled by enterprise load balancers.

However, Kubernetes itself does not have built-in, enterprise-level load balancing capabilities. Instead, it relies on third-party load balancers like Nginx, F5, and others to create an **Ingress controller**. The Ingress controller is responsible for handling the routing of external traffic into the Kubernetes cluster based on the rules specified in the Ingress resource.

In this setup:
1. The **Ingress controller** is deployed inside the Kubernetes cluster. It can be provided by third-party vendors like Nginx or F5, or even open-source options.
2. The **Ingress resource**, which is a Kubernetes object (typically defined in a YAML file), specifies the routing rules and other configurations, such as which services should receive traffic and how to route it (e.g., based on URL paths or hostnames).

The **Ingress controller** continuously watches the **Ingress resources** and implements the necessary routing and load balancing based on the configurations defined in the YAML file. This allows Kubernetes to provide more sophisticated load balancing and routing features, without incurring the extra costs of static IP addresses or relying on the default round-robin mechanism of the Kubernetes service-type LoadBalancer.

---