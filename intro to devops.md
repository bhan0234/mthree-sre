# CI/CD Overview

## Continuous Integration (CI)
CI focuses on integrating code changes frequently and automatically.

### CI Stages:
- **PLAN:** Agile methodologies like Scrum are used to manage features and user stories.
- **CODE:** Version control systems (like Git) ensure frequent, small commits and code reviews.
- **BUILD:** Automated build process compiles the code into a deployable artifact (e.g., JAR, Docker image) using tools like Maven, Gradle, or npm.
- **TEST:** Automated testing ensures code quality:
  - **Unit Tests**: Verify individual code units.
  - **Integration Tests**: Check interactions between modules.
  - **System/End-to-End Tests**: Validate entire system functionality.
  - **Regression Tests**: Ensure new code doesn’t break existing features.

## Continuous Delivery/Deployment (CD)
CD automates the release and deployment process.

### CD Stages:
- **RELEASE:** Includes creating release notes, assigning a version number, and requiring manual or automated approvals.
- **DEPLOY:** Uses tools like Kubernetes, Docker Swarm, or cloud-native services to manage deployments.
  - **Rolling Deployments**: Gradual rollout to minimize disruption.
  - **Rollbacks**: Easy reversion if issues occur.
- **MONITOR:** Application performance and health monitoring using Prometheus, Grafana, or cloud-based monitoring services.
- **FEEDBACK:** User feedback (surveys, analytics, support tickets) informs future development.

## Output of CI/CD
A tested, versioned, deployable package automatically deployed to a cloud environment (AWS, Azure, GCP).

---

# Agile Development Phases
1. **Forming**: Team introductions and project planning.
2. **Storming**: Conflicts and idea clashes as the team aligns.
3. **Norming**: Establishing common workflows and collaboration methods.
4. **Performing**: Efficient and productive execution.
5. **Adjourning**: Project completion and team transition.

---

# Software Environments
- **DEV**: Development environment.
- **TEST**: Testing environment.
- **STAGING**: Pre-production for controlled testing.
- **PRODUCTION**: Live environment for end-users.

---

# DevOps Pipelines
- **Build**: Compile code into an executable or deployable artifact.
- **Test**: Execute unit, integration, and system tests.
- **Package**: Create installers, container images, or other deployable artifacts.
- **Deployment**: Automate deployments to cloud or on-prem infrastructure.
- **Validation**: Monitor performance, logs, and automated checks.

### Continuous Pipelines
- **Continuous Integration (CI):** Frequent code integration, automated builds, and tests.
- **Continuous Delivery (CD):** Automates release up to production deployment.
- **Continuous Deployment:** Fully automated deployment to production after CI/CD stages pass.

---

# DevOps Tools
### Docker: The Standardized Cake Pan
- Packages applications into containers ensuring consistent execution across environments.

### Kubernetes: The Cake Factory Manager
- Orchestrates containerized applications across multiple servers.

### Terraform & Ansible: The Factory Builders
- **Terraform:** Provisions infrastructure as code (IaC).
- **Ansible:** Automates configuration and deployment.

### Jenkins: The Construction Foreman
- Automates build, test, and deployment processes in CI/CD pipelines.

---

# Idempotency in DevOps
Ensures repeated operations have the same result without unintended side effects. Used in:
- **Terraform** (Infrastructure provisioning)
- **Ansible** (Configuration management)
- **Docker** (Container layering)

---

# AUFS (Advanced Multi-Layered Unification Filesystem)
1. **Base Image (Read-Only)**: Immutable base layer (e.g., Ubuntu, Alpine Linux).
2. **Layered Images (Read-Only)**: Incremental changes added to the base.
3. **Containers (Read-Write)**: Unique writable layers per container.
4. **AUFS Union Mount**: Merges multiple read-only layers, with copy-on-write for modifications.

---

# DevOps Workflow
1. **Sprint**: Development cycle for features, bug fixes, or improvements.
2. **Release**: Code review, testing, and packaging.
3. **Deploy**: Automated deployment to production.
4. **Monitor**: Performance and stability tracking.
5. **Incident Handling**: Debugging and quick resolution of issues.
6. **Postmortem Analysis**: Root cause analysis and prevention measures.

---

# APIs in DevOps
APIs enable automation by integrating CI/CD pipelines, monitoring, repositories, and workflows.

### Deployment Automation:
- **Pipeline Service** checks workflow approvals and schedules releases.
- **Software Repository** ensures artifacts are available for deployment.

---

# Collaboration & Feedback
Effective communication is crucial in DevOps:
- **Tracking Issues:** Git, Jira, Trello
- **Communication Tools:** Slack, HipChat, Email, SMS
- **Incident Management:** PagerDuty, OpsGenie

---

This document provides an overview of CI/CD, Agile processes, DevOps tools, and infrastructure automation concepts essential for modern software development and deployment.
