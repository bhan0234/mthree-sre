# Jenkins CLI

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
