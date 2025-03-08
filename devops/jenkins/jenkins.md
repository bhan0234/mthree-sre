**Jenkins CLI & Architecture Overview**

---

## What is Jenkins?
Jenkins is an open-source automation server used for Continuous Integration (CI) and Continuous Deployment (CD). It helps developers automate the building, testing, and deployment of applications.

### **Why Use Jenkins?**
✅ Automates repetitive tasks – No need to manually build and deploy code.  
✅ Integrates with everything – Works with Git, Docker, Kubernetes, AWS, etc.  
✅ Runs on any platform – Windows, Linux, Mac, EC2, Kubernetes, etc.  
✅ Supports plugins – 1800+ plugins to extend functionality.  

---

## **How Jenkins Works?**
1️⃣ **Developer Pushes Code** → Code is committed to GitHub/GitLab/Bitbucket.  
2️⃣ **Jenkins Detects Changes** → It automatically pulls the latest code.  
3️⃣ **Jenkins Builds the Code** → It compiles the source code.  
4️⃣ **Jenkins Runs Tests** → Ensures the new changes don’t break the app.  
5️⃣ **Jenkins Deploys the App** → Pushes the built application to a server or cloud.  

---

## **Jenkins Architecture**
Jenkins follows a **Master-Agent** architecture:

### **1️⃣ Master Node (Controller)**
The **Master Node** is responsible for:
✅ Managing the Jenkins UI (Web Dashboard).  
✅ Scheduling and triggering build jobs.  
✅ Assigning jobs to Agent Nodes.  
✅ Monitoring and collecting build results.  
✅ Managing plugins, security, and configurations.  

In a basic setup, the **master acts as both Master + Agent**.

### **2️⃣ Agent Nodes (Workers/Build Executors)**
Agent nodes execute the actual tasks such as:
✅ Fetching the source code from GitHub/GitLab.  
✅ Running build scripts (e.g., `mvn clean install`, `npm build`).  
✅ Running test cases.  
✅ Deploying the application to servers.  

Agents connect to the master and only run builds when assigned by the master.

| Master (Controller) | Agent (Worker) |
|----------------------|---------------|
| Manages jobs        | Executes jobs |
| Assigns builds      | Runs builds   |
| Handles UI and configurations | Fetches code, builds, tests, and deploys |
| Stores logs, results, and artifacts | Reports back to Master |

#### **Example Jenkins Setup:**
🖥 **Master (Controller)** → Runs Jenkins UI, schedules jobs.  
📌 **Agent 1 (Linux)** → Builds Java applications.  
📌 **Agent 2 (Windows)** → Builds .NET applications.  
📌 **Agent 3 (Mac)** → Runs UI tests on iOS apps.  

### **How Agents Connect to Master?**
Agents can connect to the master in two ways:
1️⃣ **SSH** → The master connects to agents using SSH (Secure Shell).  
2️⃣ **JNLP (Java Web Start)** → The agent connects to the master using Java Network Launch Protocol.  

---

## **Jenkins Components**
🔹 **Jobs** – Tasks that Jenkins executes (e.g., build, test, deploy).  
🔹 **Pipelines** – Automates CI/CD workflows with code.  
🔹 **Plugins** – Extend Jenkins features (e.g., Git plugin, Docker plugin).  
🔹 **Build Triggers** – Decide when Jenkins should run (e.g., after a Git commit).  
🔹 **Workspace** – Directory where Jenkins stores code for each job.  

---

## **Jenkins as an Automation Server**
Jenkins is an automation server because it listens for triggers and runs jobs automatically. It automates CI/CD pipelines but can also automate other tasks. It behaves like a central controller for software automation.

---

## **Servers and Their Roles**
A **server** is a computer or system that provides resources, services, or data to other devices (clients) over a network. Servers can handle requests and perform tasks such as hosting websites, managing databases, or running applications.

### **Examples of Servers:**
- **Web Server (e.g., Apache, Nginx)** → Serves websites.
- **Database Server (e.g., MySQL, PostgreSQL)** → Stores and manages data.
- **Application Server (e.g., Tomcat, Node.js)** → Runs business logic for applications.

---

## **Conclusion**
Jenkins is a powerful automation tool for CI/CD that enables teams to automate build, test, and deployment processes. It uses a **Master-Agent architecture** to distribute workloads efficiently. By utilizing the **Jenkins CLI**, users can interact with Jenkins remotely and automate administrative tasks efficiently.

---
