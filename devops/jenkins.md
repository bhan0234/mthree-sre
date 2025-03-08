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


## Overview
Jenkins CLI (Command-Line Interface) allows users to interact with a Jenkins server from a terminal, enabling automation and remote execution of Jenkins commands.

## Prerequisites
- A running Jenkins instance
- Java installed on the client machine
- Jenkins CLI JAR file (downloadable from `http://<jenkins-url>/jnlpJars/jenkins-cli.jar`)
- Proper authentication (API token or SSH access)

## Connecting to Jenkins CLI
Run the following command to connect:
```
java -jar jenkins-cli.jar -s http://<jenkins-url>/ -auth <user>:<token> <command>
```
Alternatively, using SSH:
```
ssh -p <port> <user>@<jenkins-url> help
```

## Common Commands

### 1. General Information
- **Help**: `java -jar jenkins-cli.jar -s <jenkins-url> help`
- **Version**: `java -jar jenkins-cli.jar -s <jenkins-url> version`
- **Who Am I**: `java -jar jenkins-cli.jar -s <jenkins-url> who-am-i`

### 2. Managing Jobs
- **List Jobs**: `java -jar jenkins-cli.jar -s <jenkins-url> list-jobs`
- **Create Job**: `java -jar jenkins-cli.jar -s <jenkins-url> create-job <job-name> < config.xml`
- **Build Job**: `java -jar jenkins-cli.jar -s <jenkins-url> build <job-name>`
- **Delete Job**: `java -jar jenkins-cli.jar -s <jenkins-url> delete-job <job-name>`
- **Enable/Disable Job**: `enable-job <job-name>`, `disable-job <job-name>`

### 3. Managing Plugins
- **List Plugins**: `java -jar jenkins-cli.jar -s <jenkins-url> list-plugins`
- **Install Plugin**: `java -jar jenkins-cli.jar -s <jenkins-url> install-plugin <plugin-name>`
- **Uninstall Plugin**: `java -jar jenkins-cli.jar -s <jenkins-url> uninstall-plugin <plugin-name>`
- **Restart Jenkins**: `java -jar jenkins-cli.jar -s <jenkins-url> restart`

### 4. Managing Nodes
- **List Nodes**: `java -jar jenkins-cli.jar -s <jenkins-url> list-nodes`
- **Add Node**: `java -jar jenkins-cli.jar -s <jenkins-url> create-node <node-name>`
- **Delete Node**: `java -jar jenkins-cli.jar -s <jenkins-url> delete-node <node-name>`

### 5. Managing Users
- **List Users**: `java -jar jenkins-cli.jar -s <jenkins-url> list-users`
- **Create User**: `java -jar jenkins-cli.jar -s <jenkins-url> create-user <user-name>`
- **Delete User**: `java -jar jenkins-cli.jar -s <jenkins-url> delete-user <user-name>`

### 6. Backup and Restore
- **Export Jobs**: `java -jar jenkins-cli.jar -s <jenkins-url> get-job <job-name> > backup.xml`
- **Restore Job**: `java -jar jenkins-cli.jar -s <jenkins-url> create-job <job-name> < backup.xml`

### Security and Authentication
- Use API Tokens instead of passwords.
- Secure Jenkins CLI with SSH or token-based authentication.
- Restrict CLI access to authorized users.

### **Using Jenkins CLI Client**
The CLI client uses a Java JAR file to communicate with Jenkins:
```bash
java -jar jenkins-cli.jar -s http://20.127.124.114:8080/ -auth mike:11f2ac3217dc7c01abe3a9d3c4bababe5d -webSocket list-jobs
```
### **Explanation:**
- `java -jar jenkins-cli.jar` → Runs the Jenkins CLI tool.
- `-s http://20.127.124.114:8080/` → Specifies the Jenkins server URL.
- `-auth mike:11f2ac3217dc7c01abe3a9d3c4bababe5d` → Provides authentication details (user: API token/password).
- `-webSocket` → Enables WebSocket connection for better communication.
- `list-jobs` → Lists all jobs configured in Jenkins.

---

## **Servers and Their Roles**
A **server** is a computer or system that provides resources, services, or data to other devices (clients) over a network. Servers can handle requests and perform tasks such as hosting websites, managing databases, or running applications.

### **Examples of Servers:**
- **Web Server (e.g., Apache, Nginx)** → Serves websites.
- **Database Server (e.g., MySQL, PostgreSQL)** → Stores and manages data.
- **Application Server (e.g., Tomcat, Node.js)** → Runs business logic for applications.

---

# Jenkins Backup and Restore Guide


## 1. Introduction
Having reliable backups of your Jenkins controller is crucial for:
- Disaster recovery
- Restoring older configurations
- Recovering lost or corrupted files


## 2. Creating a Backup
### Filesystem Snapshots
Filesystem snapshots provide high consistency and are faster than live backups. Supported by:
- Linux LVM
- Linux btrfs
- Solaris ZFS
- FreeBSD ZFS
- OpenZFS on Linux
- Cloud providers and storage devices

### Plugins for Backup
Jenkins offers backup plugins:
- Navigate to **Manage Jenkins** > **Plugins** > **Available**
- Search for "backup"
- Use "thinBackup Plugin" (actively maintained)

### Writing a Shell Script for Backups
A shell script can automate backups:
1. Create a backup directory (e.g., `/mnt/backup`)
2. Use cron to schedule periodic backups
3. Store backups on a separate filesystem or remote storage
4. Include timestamps to prevent overwriting

## 3. Backing Up the Controller Key Separately
- **DO NOT** include the controller key in backups.
- It is located in `$JENKINS_HOME/secrets/hudson.util.Secret`.
- It encrypts sensitive data; store it securely and separately.
- The `master.key` file should be backed up separately for full restoration.

## 4. Files to Back Up
### $JENKINS_HOME
Backing up the full `$JENKINS_HOME` ensures complete recovery.

### Configuration Files
- Stored in `$JENKINS_HOME/*.xml`
- Main file: `config.xml`
- Can be stored in an SCM repository

### ./jobs Subdirectory
- Stores job-related data
- **./builds/** - Contains build records
- **./builds/archive/** - Stores archived artifacts (can be large)
- **./workspace/** - Contains checked-out files (can be excluded)
- **./plugins/*.hpi** and **./plugins/*.jpi** - Plugin packages

## 5. Files That May Not Need Backup
Some files can be re-downloaded, reducing backup size:
- **./war** - Download the latest Jenkins WAR file.
- **./cache** - Contains downloaded tools.
- **./tools** - Can be re-extracted.
- **./plugins/xxx** - Auto-populated on restart.

## 6. Validating a Backup
Ensure the backup is valid before relying on it:
1. Restore the backup to a test location (`/mnt/backup-test`).
2. Set Jenkins home: `export JENKINS_HOME=/mnt/backup-test`.
3. Start Jenkins: `java -jar jenkins.war --httpPort=9999`.

## 7. Summary
- Use filesystem snapshots or shell scripts for backups.
- Keep the controller key separate for security.
- Prioritize configuration files, jobs, and plugins in backups.
- Validate backups regularly to ensure recovery success.

## 8. Configuring Jenkins Home
To configure the Jenkins home directory:
- Default: `/var/lib/jenkins`
- Change it by modifying the environment variable:
  ```bash
  export JENKINS_HOME=/new/path/to/jenkins_home
  ```
- Ensure permissions are correctly set:
  ```bash
  chown -R jenkins:jenkins /new/path/to/jenkins_home
  ```
- Restart Jenkins after modification:
  ```bash
  systemctl restart jenkins
  ```

---

## **Conclusion**
Jenkins is a powerful automation tool for CI/CD that enables teams to automate build, test, and deployment processes. It uses a **Master-Agent architecture** to distribute workloads efficiently. By utilizing the **Jenkins CLI**, users can interact with Jenkins remotely and automate administrative tasks efficiently.

---
