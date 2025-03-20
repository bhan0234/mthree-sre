# ✨ Kubernetes Controllers - Notes ✨

## 👋 Welcome to Kubernetes Controllers!

**Instructor:** Mom

---

## 💡 What are Kubernetes Controllers?

Controllers act as the **brain** of Kubernetes.
They **monitor** Kubernetes objects and **respond** accordingly to maintain the desired system state.

---

## 🌟 Replication Controller

### **Purpose:**

- Ensures that the specified number of pods are running at all times.
- Helps achieve **high availability** by running multiple instances of a pod.
- If a pod fails, it **automatically** brings up a new one.
- **Load Balancing:** Distributes traffic across multiple pods.
- **Scaling:** Deploys additional pods when demand increases.

---

## 📝 Differences Between Replication Controller & Replica Set

| Feature                  | Replication Controller | Replica Set (Recommended) |
| ------------------------ | ---------------------- | ------------------------- |
| **Version**              | Older                  | Newer (Recommended)       |
| **API Version**          | v1                     | apps/v1                   |
| **Selector Requirement** | Optional               | Mandatory ⚠️              |
| **Matching Pods**        | Assumed automatically  | Explicitly defined        |

---

## 🤦 Creating a Replication Controller

1. **Define a YAML file** (e.g., `rc-definition.yaml`).
2. Include four sections:
   - **API Version**
   - **Kind**
   - **Metadata**
   - **Spec**
3. Under `spec`, define:
   - `replicas` (Number of desired pods)
   - `template` (Pod specification, including metadata, labels, and container details)
4. **Deploy using:**
   ```sh
   kubectl create -f rc-definition.yaml
   ```
5. **Verify the creation:**
   ```sh
   kubectl get replicationcontrollers
   kubectl get pods
   ```
6. code
   ```sh
   apiVersion: apps/v1
   kind: ReplicaSet
   metadata:
     name: myapp-replicaset
     labels:
       app: myapp
       type: front-end
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: myapp
         type: front-end
     template:
       metadata:
         name: myapp-pod
         labels:
           app: myapp
           type: front-end
       spec:
         containers:
           - name: nginx-container
             image: nginx


   apiVersion: v1
   kind: ReplicationController
   metadata:
     name: myapp-rc
     labels:
       app: myapp
       type: front-end
   spec:
     replicas: 3
     selector:
       app: myapp
       type: front-end
     template:
       metadata:
         name: myapp-pod
         labels:
           app: myapp
           type: front-end
       spec:
         containers:
           - name: nginx-container
             image: nginx
      ```
---

## 💡 Replica Set - The Modern Replacement

- Works **similarly** to Replication Controller but with enhancements.
- Requires a **selector** to match pods.
- Allows **label-based selection** of existing pods.

---

## 🔍 Creating a Replica Set

1. **Define a YAML file** (e.g., `replicaset-definition.yaml`).
2. Use `apps/v1` as the **API version**.
3. Define a **selector** under `spec` to identify pods.
4. **Deploy using:**
   ```sh
   kubectl create -f replicaset-definition.yaml
   ```
5. **Verify the creation:**
   ```sh
   kubectl get replicasets
   kubectl get pods
   ```
6. **edit**
- a temporary file opens with extra fields, u can modify the replicas there
   ```sh
   kubectl edit replicaset <name> 
   ```
---

## 🌟 Importance of Labels & Selectors

- **Labels** help Kubernetes identify and group resources.
- **Selectors** ensure that replica sets and controllers monitor the right pods.

Example:

```yaml
selector:
  matchLabels:
    app: my-app
```

---

## 📊 Scaling the Replica Set

**Scenario:**

- We start with **three** replicas.
- We decide to scale up to **six** replicas.

### Methods to Scale a Replica Set

#### 📝 Updating the Replica Count in the Definition File

1. Modify the number of replicas in the definition file to `6`.
2. Run the following command:
   ```sh
   kubectl replace -f replica-set-definition.yaml
   ```
   This updates the replica set to have six replicas.

#### 🛠️ Using the `kubectl scale` Command

1. Run the following command:
   ```sh
   kubectl scale --replicas=6 -f replica-set-definition.yaml
   ```
   **OR**
   ```sh
   kubectl scale --replicas=6 rs/my-replicaset
   kubectl scale --replicas=6 replicaset/my-replicaset
   ```
   **Note:** Using the file name as input will **not update** the number of replicas in the file.
   The definition file will still show **three** replicas even though the replica set is scaled to **six**.

#### ⚙️ Automatic Scaling (Advanced)

- There are options for **automatically scaling** the replica set based on load.
- This is an **advanced topic** that will be covered later.
- if u try to add a new one, replica set automatically deletes them.
---

## ✨ Summary of Commands

| Command                                                          | Description                                                                               |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `kubectl create -f <file>`, `edit rs`                            | Creates a replica set (or any other Kubernetes object) from the provided definition file. |
| `kubectl get rs`                                                 | Lists all replica sets.                                                                   |
| `kubectl delete rs <replica-set-name>`                           | Deletes the specified replica set.                                                        |
| `kubectl replace -f <file>`                                      | Updates the replica set based on the updated definition file.                             |
| `kubectl scale --replicas=<count> replicaset/<replica-set-name>` | Scales the replica set directly from the command line without modifying the file.         |

---

## 📊 Summary

- **Replication Controllers & Replica Sets** ensure **high availability** and **scalability**.
- **Replica Sets** are the **modern standard** with more flexibility.
- **Labels & Selectors** play a crucial role in resource management.

**Happy Kubernetes Learning!** 🚀

