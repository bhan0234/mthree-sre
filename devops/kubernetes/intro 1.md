**Kubernetes Notes**

## **Container Orchestration**
- **Definition:** The process of automatically deploying and managing containers.
- **Why is it needed?**
  - Manage multiple containers and their dependencies (e.g., databases, backend services).
  - Scale applications up or down based on demand.
  - Ensure high availability by distributing containers across multiple nodes.
  - Load balancing traffic among containers.
  - Automate deployment and scaling of services.

### **Container Orchestration Tools**
- **Docker Swarm:** Easy to set up but lacks advanced features.
- **Apache Mesos:** Powerful but complex to set up.
- **Kubernetes:** Most popular, supports complex deployments, and is widely adopted across cloud providers (AWS, Azure, GCP).

## **Advantages of Kubernetes**
- **High Availability:** Application remains running even if some nodes fail.
- **Scalability:** Increase/decrease instances of applications dynamically.
- **Resource Efficiency:** Optimize hardware utilization.
- **Declarative Configuration:** Use configuration files to manage infrastructure.
- **Portability:** Works across on-premises, hybrid, and cloud environments.

---

## **Kubernetes Architecture**
### **Nodes**
- **Definition:** A machine (physical or virtual) where Kubernetes is installed and containers are launched.
- **Worker Node:** Runs application workloads.
- **Master Node:** Manages worker nodes and ensures cluster functionality.
- **Cluster:** A set of nodes working together to ensure application availability and resource distribution.

### **Master Node Components**
1. **API Server:** Acts as the front end for Kubernetes.
   - Handles requests from CLI tools (kubectl) and UI dashboards.
2. **Etcd:** Distributed key-value store that stores cluster state and configuration.
3. **Scheduler:** Assigns newly created containers (pods) to nodes based on resource availability.
4. **Controllers:** Monitor cluster state and handle automatic recovery of failed nodes/containers.

### **Worker Node Components**
1. **Kubelet:** Agent that ensures containers are running on the node as expected.
2. **Container Runtime:** Software that runs the containers (e.g., Docker, containerd, CRI-O).
3. **Kube Proxy:** Manages networking and load balancing within the cluster.

---

## **How Master and Worker Nodes Interact**
- Master assigns workloads to worker nodes.
- Worker nodes report health and status to the master.
- All cluster state and configuration are stored in **etcd**.

### **Command Line Tool: kubectl pronounced Cube Control or Cube CTL)**
- **kubectl run**: Deploys an application.
- **kubectl cluster-info**: Provides cluster information.
- **kubectl get nodes**: Lists all nodes in the cluster.

These fundamental commands will be expanded upon as we explore Kubernetes further.

---

This document provides a foundational understanding of Kubernetes and its architecture. Future sections will dive deeper into setting up a Kubernetes cluster, managing deployments, and scaling applications effectively.

