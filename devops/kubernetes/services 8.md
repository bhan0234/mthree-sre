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
