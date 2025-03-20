# **Kubernetes Deployments - Detailed Notes** 🚀

## **Introduction** 🏗️

Kubernetes **Deployments** are a key feature that allows you to manage, scale, and upgrade applications efficiently. They provide a higher-level abstraction over **Pods** and **ReplicaSets**, ensuring seamless application lifecycle management.

## **Why Use Deployments?** 🤔

Deployments provide several advantages over directly managing Pods or ReplicaSets:

### **1️⃣ Scaling Applications Efficiently** 📈

- In production environments, a single instance (Pod) of an application is not enough.
- Deployments allow you to run multiple instances, ensuring high availability and load distribution.
- Kubernetes automatically handles the scaling based on resource usage.

### **2️⃣ Rolling Updates for Seamless Upgrades** 🔄

- When a new version of an application is available in a Docker registry, it should be deployed smoothly.
- Rolling updates replace old versions **one at a time** to ensure minimal downtime.
- This prevents service interruptions for users.

### **3️⃣ Rollback in Case of Failures** 🛑

- If an update introduces a bug, you can **rollback** to the previous stable version.
- This ensures quick recovery from failures.

### **4️⃣ Batch Updates (Pause & Resume Feature)** ⏸️▶️

- Sometimes multiple changes need to be made (e.g., upgrading the web server version, adjusting resource allocation, and scaling the environment).
- Deployments allow you to apply updates **all at once** instead of executing each change immediately.
- This ensures consistency and better control over updates.

## **Pods, ReplicaSets, and Deployments - Understanding the Hierarchy** 🏛️

| **Component**   | **Purpose**                                                                                |
| --------------- | ------------------------------------------------------------------------------------------ |
| **Pods**        | Smallest deployable unit in Kubernetes, contains one or more containers.                   |
| **ReplicaSets** | Ensures that a specified number of **Pod replicas** are always running.                    |
| **Deployments** | Manages **ReplicaSets** and provides advanced features like rolling updates and rollbacks. |

- **Pods** contain application containers.
- **ReplicaSets** ensure a defined number of Pod replicas are running.
- **Deployments** manage ReplicaSets, enabling easy upgrades and rollbacks.

## **Creating a Deployment** 📝

A Deployment is defined using a **YAML configuration file**, similar to ReplicaSets but with `kind: Deployment`.

### **Deployment YAML Structure** 🏷️

### **Explanation of the YAML File** 📌

- `apiVersion: apps/v1` → Specifies the API version.
- `kind: Deployment` → Defines the object type as a **Deployment**.
- `metadata` → Contains the deployment name and labels.
- `spec` → Defines the desired state:
  - `replicas: 3` → Ensures **three instances** of the application are running.
  - `selector` → Matches labels to link ReplicaSet and Pods.
  - `template` → Defines the Pod specification inside the Deployment.
  - `containers` → Specifies container details (name, image, exposed port).

## **Deploying the Application in Kubernetes** ⚙️

### **Step 1: Apply the Deployment Configuration** 🏗️

Run the following command to create the deployment:

### **Step 2: Verify the Deployment** ✅

Check if the deployment was successfully created:

### **Step 3: Check ReplicaSets** 🔄

Since deployments create ReplicaSets automatically, verify them with:

### **Step 4: View Running Pods** 🏃‍♂️

Deployments ultimately create Pods, which can be checked using:

## **Key Features of Deployments** ✨

### **1️⃣ Rolling Updates** 🚀

- Ensures zero downtime while upgrading applications.
- Kubernetes replaces Pods gradually instead of all at once.
- Command to update an image:
- Monitor the update progress:

### **2️⃣ Rollback to a Previous Version** 🔙

- If an update fails, rollback using:
- Check the rollout history:

### **3️⃣ Scaling the Deployment** 📊

- Increase or decrease the number of running instances:

### **4️⃣ Pause and Resume a Deployment** ⏸️▶️

- Pause an ongoing deployment to make multiple changes before applying:
- Resume the deployment after making changes:

## **All Deployment-Related Commands** 🔥
1. code 

  ```
  kubectl create -f deployment-definition.yml
  kubectl get deployments
  ```

### **Checking All Created Resources** 🧐

To see all created Kubernetes objects:

`kubectl get all`

This will list Deployments, ReplicaSets, and Pods in the cluster.

## **Conclusion** 🎯

- **Deployments** simplify application management in Kubernetes.
- They provide **scalability, rolling updates, rollback, and pause/resume features**.
- Using `kubectl` commands, we can efficiently manage deployments.

With these features, **Kubernetes Deployments** ensure high availability and seamless updates for applications in production environments. 🚀🔥



# Kubernetes Deployments: Updates & Rollbacks 🚀

## 🎯 Introduction
This guide covers updates and rollbacks in Kubernetes deployments, explaining rollouts, versioning, and deployment strategies with examples and commands.

---
## 🔄 Rollouts & Versioning
When you create a **deployment**, it triggers a **rollout**. Each rollout creates a **new deployment revision**.

### 📌 How Rollouts Work:
1. **Initial Deployment** → Revision **1**
2. **Update Application** (e.g., new container version) → Revision **2**
3. **Further Updates** → Creates new revisions

🔍 **Check rollout status:**
```sh
kubectl rollout status deployment/<deployment-name>
```

📜 **View rollout history:**
```sh
kubectl rollout history deployment/<deployment-name>
```

---
## 📌 Deployment Strategies
Kubernetes supports two main deployment strategies:

### ❌ 1. Recreate Strategy
- **How it works?**
  - Stops all old replicas first
  - Deploys new replicas afterward
- **Problem?** ⚠ Application **downtime** between old and new versions

### 🔄 2. Rolling Update Strategy (**Default**)
- **How it works?**
  - Takes down old replicas **one by one**
  - Brings up new replicas **gradually**
- ✅ Ensures **zero downtime** during updates

📝 **By default, Kubernetes uses Rolling Update strategy.**

---
## 🔧 Updating Deployments
Updates can include:
- Updating **Docker image version**
- Changing **labels**
- Modifying **replica count**

### 🏗️ Update using a Deployment YAML file:
Modify `deployment.yaml` and apply changes:
```sh
kubectl apply -f deployment.yaml
```

### 🔥 Quick Update via CLI:
Update the container image directly:
```sh
kubectl set image deployment/<deployment-name> <container-name>=<new-image>
```
⚠ **Warning:** This method doesn't update the deployment YAML file.

🔎 **View Deployment Details:**
```sh
kubectl describe deployment <deployment-name>
```

---
## 📌 Understanding Replica Sets
When a deployment is created:
1. **A ReplicaSet is automatically created** to manage pods
2. **On updates, a new ReplicaSet is created** and old ones are scaled down

🔍 **List ReplicaSets:**
```sh
kubectl get replicasets
```

📌 Example:
Before an update → Old ReplicaSet has `5` pods
After an update → Old ReplicaSet scaled to `0`, new one has `5` pods

---
## 🔙 Rolling Back a Deployment
If an update fails, **rollback** to a previous revision.

### 🔄 Undo Last Deployment Change:
```sh
kubectl rollout undo deployment/<deployment-name>
```

### 🔄 Rollback to a Specific Revision:
```sh
kubectl rollout undo deployment/<deployment-name> --to-revision=<revision-number>
```

🔎 **Check ReplicaSets before & after rollback:**
```sh
kubectl get replicasets
```

---
## 🚀 Creating a Deployment (Alternative)
Instead of using a YAML file, create a deployment directly:
```sh
kubectl run <deployment-name> --image=<image-name>
```
```
// directly running an image creates a deployment by itself
kubectl run nginx --image=nginx
deployment "nginx" created
```
💡 **Best Practice:** Use a YAML file to manage deployments in a version-controlled repository.

---
## 📝 Summary of Commands 📜
| Action | Command |
|---------|----------------------------------------|
| **Create Deployment** | `kubectl create -f deployment.yaml --record` records this change-cause|
| **List Deployments** | `kubectl get deployments` |
| **Update Deployment (YAML)** | `kubectl apply -f deployment.yaml` |
| **Update Deployment (CLI)** | `kubectl set image deployment/<deployment-name> <container-name>=<new-image>` |
| **Check Rollout Status** | `kubectl rollout status deployment/<deployment-name>` |
| **View Rollout History** | `kubectl rollout history deployment/<deployment-name>` |
| **Rollback Deployment** | `kubectl rollout undo deployment/<deployment-name>` |
| **Rollback to Specific Revision** | `kubectl rollout undo deployment/<deployment-name> --to-revision=<revision-number>` |
| **View Deployment Details** | `kubectl describe deployment <deployment-name>` |
| **List ReplicaSets** | `kubectl get replicasets` |
| record is deprecated | `kubectl set image deployment nginx-deployment nginx=nginx:1.26.3` |  
|                      | `kubectl annotate deployment nginx-deployment kubernetes.io/change-cause="Updated container image to v2"` |  
---
## 🎯 Conclusion
- **Kubernetes manages rollouts & rollbacks seamlessly**
- **Use Rolling Updates** to ensure zero downtime
- **Monitor ReplicaSets** during updates
- **Use Rollout History** to track and rollback changes

🚀 **Mastering these deployment strategies ensures smooth application upgrades & stability!**

## Deployment Process

### 1. Initial Deployment
![Initial Deployment](https://github.com/user-attachments/assets/6db13b76-a07e-44a0-b4d4-d68f9edc6173)

### 2. Deployment Strategy
![Deployment Strategy](https://github.com/user-attachments/assets/376ac806-f59c-4840-99db-ffe7d3e689e3)

### 3. Rolling Update
![Rolling Update](https://github.com/user-attachments/assets/f124e2ed-8b92-48a7-9718-fceff42c371c)

### 4. Recreate Strategy
![Recreate Strategy](https://github.com/user-attachments/assets/2fd527a0-4f7a-4c5f-a12a-c0f02c7b3368)

### 5. Replica Set
![Replica Set](https://github.com/user-attachments/assets/1ce2988c-370e-409e-9eea-e88649272021)

### 6. Rollback Process
![Rollback Process](https://github.com/user-attachments/assets/bbbad44d-7da1-4e26-8bd9-53f88c06e37b)

### 7. Undo Rollout
![Undo Rollout](https://github.com/user-attachments/assets/0e7d70fe-9259-4f08-98de-7683c1d67679)
---


