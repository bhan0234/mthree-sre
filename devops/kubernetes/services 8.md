**Kubernetes Services**

### Introduction
Kubernetes services enable communication between various components within and outside of the application. They connect applications with other applications or users. For example, in a system where pods handle different responsibilities—such as serving a frontend, managing backend processes, or connecting to an external data source—services enable these groups of pods to communicate effectively.

Services also allow the frontend application to be accessible to users and facilitate interactions between backend and frontend pods, as well as external data sources. This enables loose coupling between microservices within an application.

### External Communication in Kubernetes
Pods in Kubernetes use internal networking for communication, but external access to applications requires additional configurations. Consider a scenario where:
- A Kubernetes node has an IP address (e.g., 192.168.1.2)
- A user's laptop is on the same network (e.g., 192.168.1.10)
- The internal pod network operates in the range 10.240.4.0/24
- A specific pod has an IP address of 10.240.4.2

Since the pod belongs to a different network, direct access from the user’s laptop is not possible. To allow external access, Kubernetes provides services that map requests to the correct pod.

### Kubernetes Service Types
Kubernetes services act as intermediaries to manage communication and external access. There are three main types of services:

1. **NodePort Service**
   - Maps an internal pod port to a port on the node.
   - Allows external users to access the application via the node’s IP and assigned port.
   - Example:
     - Pod port (target port): 80
     - Service port: 80
     - NodePort: 30008 (valid range: 30000-32767)
   - Requests to `http://<node-ip>:30008` are forwarded to the pod.
  
| Port Type          | Location               | Owned By                | Accessible From         |
|--------------------|-----------------------|-------------------------|-------------------------|
| **TargetPort (80)** | Inside the Pod (Container) | The container inside the Pod | Only from inside the Pod |
| **Service Port (80)** | Kubernetes Virtual Network | Kubernetes Service (ClusterIP, NodePort, etc.) | Only from inside the cluster |
| **NodePort (30008)** | Worker Node OS | The Worker Node itself | From outside the cluster (public access) |

2. **ClusterIP Service**
   - Provides internal communication between services within the cluster.
   - Creates a virtual IP address for services to communicate (e.g., frontend to backend communication).

3. **LoadBalancer Service**
   - Provisions an external load balancer (on supported cloud providers) to distribute traffic across multiple pods.
   - Useful for balancing load in frontend applications with multiple instances.

### Creating a NodePort Service
To create a NodePort service, a definition file (YAML) is used:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008
  selector:
    app: my-app
```

Key sections of the definition file:
- **apiVersion**: Specifies the API version (v1).
- **kind**: Defines the type (Service).
- **metadata**: Includes the service name.
- **spec**:
  - **type**: Specifies the service type (NodePort).
  - **ports**: Defines port mappings.
    - **port**: The service’s port (80).
    - **targetPort**: The pod’s port (80).
    - **nodePort**: Exposes the service on port 30008.
  - **selector**: Links the service to pods with the label `app: my-app`.
```sh
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: myapp
    type: front-end
```
###commands for nodeport
```
kubectl create -f services-def.yaml
kubectl get svc
```
### Service Discovery and Load Balancing
- When a service is created, Kubernetes automatically assigns a **ClusterIP**.
- Using labels and selectors, the service identifies matching pods.
- Requests are automatically balanced across selected pods using a random algorithm.

### Services Across Multiple Nodes
- When multiple pods are distributed across different nodes, Kubernetes ensures that the NodePort service is available on all nodes.
- The service spans the entire cluster, allowing access through any node’s IP and the assigned NodePort.
- If pods are added or removed, Kubernetes updates the service automatically.

### Summary
- Kubernetes services facilitate communication between pods and external users.
- **NodePort** services expose applications externally using a fixed port range.
- **ClusterIP** services allow internal communication between services.
- **LoadBalancer** services provide external access with load distribution.
- Kubernetes automatically updates services as pods are added or removed, ensuring flexibility and adaptability.

![Image](https://github.com/user-attachments/assets/7281a126-992b-4bef-bacf-550b3a0661f3)

![Image](https://github.com/user-attachments/assets/e0f8d17e-e706-4a8c-ac1e-2138b089cb88)

![Image](https://github.com/user-attachments/assets/ac3f79f5-e483-40ec-ab7d-987207ce0097)

![Image](https://github.com/user-attachments/assets/341f5d1e-b201-4a45-a129-45dae0fa245d)

![Image](https://github.com/user-attachments/assets/0688beab-dbba-49a3-a941-9bf64c15593d)

![Image](https://github.com/user-attachments/assets/29c8ede5-6ced-45ff-9a31-2d35689d5ce8)

![Image](https://github.com/user-attachments/assets/c16a322b-5112-42be-b70e-3ce2f7a821e2)





# 📌 Understanding Kubernetes ClusterIP Service

## 🎉 Introduction
Welcome to this lecture! In this session, we will explore **Kubernetes Service ClusterIP** and how it enables seamless communication within a Kubernetes cluster.

A full-stack web application typically consists of multiple components running across different pods:
- 🌐 **Frontend Web Server**
- 🔧 **Backend Server**
- 📦 **Key-Value Store (e.g., Redis)**
- 🗄️ **Persistent Database (e.g., MySQL)**

Each of these components needs a way to communicate with the others in a **dynamic** and **scalable** manner. This is where **Kubernetes services** come into play.

---

## 🤔 Problem with Direct Pod Communication

Every pod in Kubernetes is assigned an **IP address**. However, there are challenges:

1. **Pods are ephemeral** – They can be terminated and replaced at any time, leading to new IP assignments.
2. **Dynamic Scaling** – If the backend scales from 2 to 5 pods, how does the frontend know which ones to connect to?
3. **Load Balancing** – Which backend pod should a frontend pod communicate with?

Relying on pod IPs is **not feasible** for internal communication in a microservices architecture. Instead, we use **Kubernetes Services** to solve this.

---

## 🚀 What is a Kubernetes Service?

A **Service** in Kubernetes groups a set of pods and provides a **single interface** to access them. It acts as a stable network endpoint, even when individual pods change.

For example:
- A **backend service** groups all backend pods together and exposes a single access point.
- A **Redis service** enables backend pods to connect to the key-value store consistently.

With services, **scalability and reliability** are significantly improved because the frontend, backend, and databases can scale independently.

---

## 🔥 ClusterIP: The Default Kubernetes Service

### 🔹 What is ClusterIP?
A **ClusterIP** service allows pods to communicate with each other inside the cluster using a stable IP address and DNS name.

- Each **service** is assigned an **IP address** within the cluster.
- Other pods **inside the cluster** can access it using:
  - **Cluster IP** (e.g., `10.100.1.25`)
  - **Service name** (e.g., `backend-service`)

### 🔹 How Does ClusterIP Work?
When a request is sent to a ClusterIP service:
1. Kubernetes routes the request to one of the backend pods.
2. It uses **round-robin load balancing** (by default) to distribute requests.
3. Internal services can communicate reliably without worrying about individual pod IPs.

---

## 🛠️ Creating a ClusterIP Service (YAML Definition)
We define services using a YAML file. Below is an example of a **ClusterIP service** for the backend:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend-service  # Service name
spec:
  type: ClusterIP        # Default type (optional)
  selector:
    app: backend        # Selects pods with this label
  ports:
    - port: 80          # Service port
      targetPort: 80    # Pod's port
```

### 🔍 Breakdown of the YAML File:
- **apiVersion: v1** → Specifies the API version.
- **kind: Service** → Defines this resource as a Kubernetes service.
- **metadata.name** → Names the service (`backend-service`).
- **spec.type: ClusterIP** → Specifies the service type.
- **spec.ports** → Defines port mappings:
  - **port** → The port that other pods will use to access the service.
  - **targetPort** → The port on the actual pod that will receive traffic.
- **spec.selector** → Links the service to backend pods using labels.

---

## 🏃‍♂️ Deploying the Service
Once the YAML file is ready, deploy the service using the following commands:

1️⃣ **Create the Service**:
```sh
kubectl apply -f backend-service.yaml
```

2️⃣ **Verify the Service**:
```sh
kubectl get services
```

🔍 This will show the **ClusterIP** assigned to the service, allowing other pods to communicate with it.

3️⃣ **Access the Service from Another Pod**:
```sh
//inside the cluster they are called directly by the name of the service, but in the nodeport we shpuld use the ip of the node
curl http://backend-service:80
```

---

## 💡 Why Use ClusterIP?
✅ **Ensures stable communication** between microservices.
✅ **Handles load balancing** automatically.
✅ **Simplifies service discovery** using DNS names.
✅ **Enhances security** by restricting access to within the cluster.

---

## 🎯 Summary
- **Pods have dynamic IPs**, making direct communication unreliable.
- **Kubernetes Services** provide a stable entry point for communication.
- **ClusterIP** is the default service type, enabling internal communication.
- **Services use labels and selectors** to route traffic to the correct pods.
- **DNS names** can be used instead of Cluster IPs for better reliability.

🚀 With ClusterIP, your microservices architecture becomes **more resilient, scalable, and manageable!** 🎉

---
# 📌 IP Cluster

![IP Cluster 1](https://github.com/user-attachments/assets/3b33aa2b-dca9-4850-97c8-c73cb1d66270)

![IP Cluster 2](https://github.com/user-attachments/assets/acee8879-5dd6-43f0-9f20-60045970593c)



---

# 🚀 Load Balancer Type Service

## 🌐 Introduction

The **Load Balancer** type service in Kubernetes allows external users to access applications using a single URL instead of multiple NodePort IP combinations. This provides an efficient way to manage and distribute traffic across multiple nodes.

---

## 🏗️ Understanding Load Balancer Services

### ✅ NodePort Services Recap

Previously, we learned about **NodePort** services, which expose applications on a high port across all worker nodes in a cluster. However, NodePort services come with a challenge:

- You must manually specify the **IP**\*\*:Port\*\* combination for external users.

- If a cluster has four nodes, users may have **four different IPs** for an application.  voter and result model, voter pods are only on 2 nodes, and the other result are on 2 other ports. But can acces using any ip of the 4 along with the speciific port no.

- Even if pods are running on only **two nodes**, the service is accessible from all four nodes.

- This approach lacks a **single, user-friendly URL** for end users.

### 🔄 The Role of a Load Balancer

To provide a single URL such as **voting.example.com** or **result.example.com**, a **Load Balancer** is used. This component:

- Distributes traffic evenly across multiple pods.
- Offers a **static endpoint (DNS)** instead of multiple IPs.
- Ensures **high availability and reliability** for applications.

---

## ⚙️ How Load Balancers Work

### 🌍 External Load Balancer Setup

One way to implement a load balancer is by setting up an external **Virtual Machine (VM)** and installing a load balancing tool such as:

- **HAProxy** 🛠️
- **NGINX** 🌐
- **Traefik** 🚦

This VM will handle the routing of traffic to the worker nodes.

### ☁️ Cloud Provider Load Balancer

If using **Google Cloud (GCP), AWS, or Azure**, Kubernetes integrates **natively** with cloud load balancers:

- These cloud providers **automatically configure** a load balancer.
- They provide a **public IP and DNS** for external access.
- The load balancer manages **failover and scaling**.

To enable this in Kubernetes, simply set the service type to:

### ⚠️ Unsupported Environments

If running Kubernetes in **VirtualBox** or other environments that do not support cloud load balancers:

- Setting **type: LoadBalancer** behaves the same as **NodePort**.
- The service is still exposed on high ports but lacks external load balancing.

---

## 🏁 Conclusion

- **NodePort** services expose applications via IP\:Port, making access cumbersome.
- **Load Balancer** services provide a **single entry point (URL)** for external users.
- **Cloud Load Balancers** handle traffic distribution automatically, ensuring scalability.
- In unsupported environments, **LoadBalancer services fall back to NodePort behavior**.

# ⚖️ Load Balancing

![Load Balancing 1](https://github.com/user-attachments/assets/f04a32a7-79de-48b8-af6b-e084e38ac687)

![Load Balancing 2](https://github.com/user-attachments/assets/5cf067b5-dade-4136-856f-15ff6853848b)




