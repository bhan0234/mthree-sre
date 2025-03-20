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

## Pipeline Syntax Summary

This document summarizes the syntax for Jenkins Pipelines, focusing on Declarative and Scripted approaches.

**Key Concepts:**

*   **Step:** The fundamental unit of work in a Pipeline.
*   **Pipeline:** A series of connected steps defining a continuous delivery process.
*   **Jenkinsfile:** A text file containing the Pipeline definition, stored in source control.

**Two Syntaxes:**

1.  **Declarative Pipeline:** A more structured and opinionated syntax. Requires the "Pipeline: Declarative Plugin".
2.  **Scripted Pipeline:** A more flexible, Groovy-based DSL.

**Declarative Pipeline**

*   Enclosed within a `pipeline { ... }` block.
*   Uses Sections, Directives, and Steps.
*   No semicolons as statement separators (each statement on a new line).
*   Property references treated as no-argument method invocations.

**Declarative Pipeline Sections:**

*   **`agent`:** Specifies where the Pipeline or stage will execute (e.g., `any`, `none`, `label`, `docker`, `dockerfile`, `kubernetes`). Supports options like `label`, `customWorkspace`, `reuseNode`, and `args`.
```
Tells Jenkins to find a worker node that has the label kaniko.
If no agent with the label kaniko is available, Jenkins will wait until one is free.
“kaniko” likely refers to a node configured for building Docker images using Kaniko (a tool for building container images in Kubernetes without needing Docker
  agent {
      label 'kaniko'
  }

```
*   **`post`:** Defines steps to run after Pipeline or stage completion, based on conditions like `always`, `changed`, `fixed`, `regression`, `aborted`, `failure`, `success`, `unstable`, `unsuccessful`, and `cleanup`.
```
post {
    always {
        // Runs always, no matter what
    }
    success {
        // Runs only if the pipeline succeeds
    }
    failure {
        // Runs only if the pipeline fails
    }
    unstable {
        // Runs if the pipeline is marked as unstable
    }
    changed {
        // Runs if the build status has changed from the previous run
    }
}
```
*   **`stages`:** Contains a sequence of `stage` directives representing distinct parts of the delivery process.
*   **`steps`:** Defines a series of actions to be executed within a `stage`.

**Declarative Pipeline Directives:**

*   **`environment`:** Defines environment variables for the Pipeline or stage. Can use the `credentials()` helper to access pre-defined Jenkins credentials (Secret Text, Secret File, Username and Password, SSH with Private Key).
'''
environment {
    APP_ENV = 'production'
    DB_PASS = credentials('db-password')
}
'''
*   **`options`:** Configures Pipeline-specific options, such as `buildDiscarder`, `checkoutToSubdirectory`, `disableConcurrentBuilds`, `disableResume`, `newContainerPerStage`, `overrideIndexTriggers`, `preserveStashes`, `quietPeriod`, `retry`, `skipDefaultCheckout`, `skipStagesAfterUnstable`, `timeout`, `timestamps`, `parallelsAlwaysFailFast`, `disableRestartFromStage`. Stage-level options are more limited (e.g., `retry`, `timeout`, `timestamps`, `skipDefaultCheckout`).
  ```
pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
        heckoutToSubdirectory('my-source-code')    -Clones repo into a subdirectory

    }
    stages {
        stage('Build') {
            options {
                timeout(time: 10, unit: 'MINUTES')  // Timeout after 10 minutes
                 retry(2)  // Retry the deploy stage twice on failure
            }
            steps {
                echo 'Building the application...'
            }
        }
```
*   **`parameters`:** Defines parameters a user should provide when triggering the Pipeline. Available parameter types: `string`, `text`, `booleanParam`, `choice`, `password`. Values accessible via the `params` object.
*   **`triggers`:** Defines automated ways to re-trigger the Pipeline (e.g., `cron`, `pollSCM`, `upstream`).
    *   `cron` syntax:  `MINUTE HOUR DOM MONTH DOW`. Uses `H` (hash) for even load distribution.  Supports operators like `*`, `M-N`, `M-N/X`, `*/X`, `A,B,…​,Z`. Also supports `@yearly`, `@annually`, `@monthly`, `@weekly`, `@daily`, `@midnight`, `@hourly`.
*   **`stage`:** Defines a stage within the `stages` section. Must contain a `steps` section or other stage-specific directives (`agent`, `tools`, `input`, `when`, `stages`, `parallel`, or `matrix`).
*   **`tools`:** Defines tools to automatically install and add to the PATH (e.g., `maven`, `jdk`, `gradle`).
*   **`input`:** Prompts for user input, pausing the stage execution. Configuration options include `message`, `id`, `ok`, `submitter`, `submitterParameter`, and `parameters`.
*   **`when`:** Conditionally executes a stage based on specified criteria. Supports conditions like `branch`, `buildingTag`, `changelog`, `changeset`, `changeRequest`, `environment`, `equals`, `expression`, `tag`, `not`, `allOf`, `anyOf`, `triggeredBy`.  `beforeAgent`, `beforeInput`, and `beforeOptions` can be used to control when `when` is evaluated.
*   **Sequential Stages:** `Stages` within a `stage` are executed sequentially.
*   **`parallel`:** Contains nested `stage` directives to be executed in parallel.  `failFast true` aborts all parallel stages if one fails.  Can also be set as an option at the pipeline definition to force all parallel stages to fail fast.
*   **`matrix`:** Defines a multi-dimensional matrix of name-value combinations to be run in parallel.
    *   `axes`: Defines the values for each axis in the matrix.
    *   `stages`: Defines the list of stages to run sequentially in each cell.
    *   `excludes`: Allows excluding invalid cells from the matrix.
    *   Supports stage-level directives under matrix itself: `agent`, `environment`, `input`, `options`, `post`, `tools`, `when`.

**Special Declarative Pipeline Steps:**

*   **`script`:** Executes a block of Scripted Pipeline code within a Declarative Pipeline.  Use sparingly; prefer shared libraries for complex logic.

**Scripted Pipeline**

*   A general-purpose DSL built with Groovy.
*   Serially executed from top to bottom.
*   Uses Groovy's flow control (e.g., `if/else`, `try/catch`).

**Scripted Pipeline Steps:**

*   Uses the same steps as Declarative Pipeline, without any syntax-specific steps.
*   Comprehensive listing of steps available in the Pipeline Steps reference.

**Syntax Comparison**

| Feature        | Declarative Pipeline               | Scripted Pipeline                   |
|----------------|------------------------------------|------------------------------------|
| Structure      | More structured, opinionated      | More flexible, Groovy-based        |
| Syntax         | Simplified, less Groovy knowledge | Requires Groovy knowledge           |
| Error Handling | Implicit, more limited           | Explicit `try/catch/finally` blocks |
| Use Cases      | Simpler, common workflows         | Complex, custom workflows           |

This summary provides a foundation for understanding Pipeline syntax. Refer to the Pipeline Steps reference and the official Jenkins documentation for more in-depth information.
