**Jenkins Pipeline Concepts and Jenkinsfile Overview**

### **Pipeline Concepts**
Jenkins Pipeline is a suite of plugins that enables implementing and integrating continuous delivery (CD) pipelines into Jenkins. Below are key aspects of Jenkins Pipelines:

- **Pipeline**: A user-defined model of a CD pipeline that automates the build, test, and deployment processes of an application.
- **Node**: A machine within the Jenkins environment that executes the pipeline.
- **Stage**: A logically distinct phase in the pipeline (e.g., Build, Test, Deploy).
- **Step**: A single task within a stage that instructs Jenkins on what to execute at a given point in the pipeline.

### **Jenkinsfile**
A **Jenkinsfile** is a text file that defines the pipeline as code. It can be written using **Declarative** or **Scripted** syntax.

#### **Declarative Pipeline Example**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'make'
            }
        }
        stage('Test') {
            steps {
                sh 'make check'
                junit 'reports/**/*.xml'
            }
        }
        stage('Deploy') {
            steps {
                sh 'make publish'
            }
        }
    }
}
```

#### **Scripted Pipeline Example**
```groovy
node {
    stage('Build') {
        sh 'make'
    }
    stage('Test') {
        sh 'make check'
        junit 'reports/**/*.xml'
    }
    stage('Deploy') {
        sh 'make publish'
    }
}
```

### **Explanation of Pipeline Components**
```groovy
pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'make'
            }
        }
        stage('Test'){
            steps {
                sh 'make check'
                junit 'reports/**/*.xml'
            }
        }
        stage('Deploy') {
            steps {
                sh 'make publish' //
            }
        }
    }
}
```
- **pipeline**: Defines the overall Jenkins pipeline structure.
- **agent any**: Specifies that the pipeline can run on any available agent (node) in the Jenkins environment. The agent is responsible for executing the pipeline stages.
- **options**: Defines additional pipeline options. Here, `skipStagesAfterUnstable()` ensures that if a stage fails, subsequent stages are skipped.
- **stages**: Groups multiple `stage` blocks, each representing a distinct phase of the pipeline.
- **stage('Build')**: Represents the build phase where the application is compiled.
- **stage('Test')**: Runs tests to verify application integrity.
- **stage('Deploy')**: Deploys the application to the target environment.
- **steps**: Contains individual commands to be executed within a stage.
- **sh 'make'**: Runs a shell command to build the application.
- **sh 'make check'**: Executes test commands.
- **junit 'reports/**/*.xml'**: Collects and reports test results.
- **sh 'make publish'**: Publishes the built application.

### **Freestyle Jobs vs Pipeline Jobs**
- freestyle job is not durable, when sys restarts, pipeline can resume but freestyle cant.
- freestyle has upstream and downstream which is difficult.
| Feature          | Freestyle Job | Pipeline Job |
|-----------------|--------------|-------------|
| Configuration   | UI-based      | Code-based (Jenkinsfile) |
| Flexibility     | Limited       | Highly flexible |
| Version Control | Not tracked   | Stored in SCM (e.g., Git) |
| Scripting       | No            | Yes (Declarative/Scripted) |
| Plugins         | Many required | Built-in extensibility |

### **Storage of Created Jobs**
All jobs created in Jenkins are stored under the `workspaces` directory, not `workbench`. Each job has a dedicated folder inside Jenkins' file system, typically under:
```
$JENKINS_HOME/workspaces
```
This ensures that Jenkins maintains a separate environment for each job execution.

By defining pipelines as code using Jenkinsfiles, teams can implement robust, repeatable, and version-controlled CI/CD workflows.

