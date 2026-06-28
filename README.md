# 🚀 End-to-End CI/CD Pipeline using Jenkins, SonarQube, Docker & Docker Swarm

## 📌 Project Overview

This project demonstrates a complete end-to-end Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Java web application using Jenkins. The pipeline automates the complete software delivery lifecycle, starting from source code checkout to automated deployment on a Docker Swarm cluster.

The project integrates code quality analysis, containerization, security scanning, image publishing, and container orchestration to simulate a real-world DevOps workflow.

---

# 🎯 Project Objectives

* Automate the software delivery process.
* Perform static code analysis before deployment.
* Build a deployable Java application using Maven.
* Containerize both application and database.
* Scan Docker images for vulnerabilities.
* Push Docker images to Docker Hub.
* Deploy the application automatically to a Docker Swarm Cluster.
* Demonstrate a production-style CI/CD workflow.

---

# ☁ Infrastructure

The complete project was deployed on Amazon EC2.

| Server               | Instance Type | Purpose                                                            |
| -------------------- | ------------- | ------------------------------------------------------------------ |
| Jenkins Server       | **c7i-flex**  | Hosts Jenkins and triggers the CI/CD pipeline                      |
| Docker Swarm Manager | **m7i-flex**  | Configured as a Jenkins Agent and manages the Docker Swarm Cluster |
| Worker Node 1        | **t3.micro**  | Runs application containers                                        |
| Worker Node 2        | **t3.micro**  | Runs application containers                                        |

---

# 🏗 Architecture


                        GitHub Repository
                               │
                               ▼
                    Jenkins Server (c7i-flex)
                               │
                      SSH Connection
                               │
                               ▼
             Docker Swarm Manager (m7i-flex)
                               │
                  Jenkins Pipeline Execution
                               │
      ┌────────────────────────┴────────────────────────┐
      │                                                 │
      ▼                                                 ▼
Docker Image Build                              Docker Stack Deploy
      │                                                 │
      ▼                                                 ▼
 Docker Hub Repository                    Docker Swarm Scheduler
                                                     │
                   ┌─────────────────────────────────┴─────────────────────────────────┐
                   ▼                                                                   ▼
          Worker Node 1 (t3.micro)                                      Worker Node 2 (t3.micro)
                   │                                                                   │
                   └──────────────── Java Application Containers ──────────────────────┘


---

# ⚙ Project Workflow

1. Source code is stored in GitHub.
2. Jenkins pulls the latest source code.
3. Jenkins executes the pipeline on the Docker Swarm Manager node through SSH.
4. SonarQube performs static code analysis.
5. Maven compiles the application and generates a WAR file.
6. The WAR file is copied into the Docker build context.
7. Docker builds the Application and Database images.
8. Trivy scans both images for security vulnerabilities.
9. Docker images are tagged and pushed to Docker Hub.
10. Jenkins executes Docker Stack deployment.
11. Docker Swarm automatically schedules the containers across the worker nodes.

---

# 🛠 Technology Stack

* AWS EC2
* Linux
* Git
* GitHub
* Jenkins
* Jenkins Agents
* Maven
* SonarQube
* Docker
* Docker Hub
* Docker Swarm
* Docker Stack
* Trivy
* Java
* Apache Tomcat
* MySQL

---

# 🔄 CI/CD Pipeline Workflow


GitHub Repository
        │
        ▼
Source Code Checkout
        │
        ▼
Code Quality Analysis
        │
        ▼
Maven Build
        │
        ▼
Generate WAR File
        │
        ▼
Docker Image Build
        │
        ▼
Trivy Image Scan
        │
        ▼
Docker Image Tag
        │
        ▼
Docker Hub Push
        │
        ▼
Docker Stack Deploy
        │
        ▼
Docker Swarm Cluster
        │
        ▼
Application Running


---

# 📋 Jenkins Pipeline Stages

## 1. Source Code Checkout

* Jenkins clones the latest source code from GitHub.
* Git is used as the Source Code Management system.

---

## 2. Code Quality Analysis

* Jenkins integrates with SonarQube.
* Static code analysis is performed.
* Code smells, bugs and vulnerabilities are identified before build.

Command Executed:

```bash
mvn clean verify sonar:sonar -Dsonar.projectKey=docker-node
```

---

## 3. Maven Build

The application is built using Maven.

Tasks performed:

* Dependency download
* Compilation
* Unit Test Execution
* WAR File Generation

Generated Artifact

```
target/*.war
```

The generated WAR file is copied into the Docker build context.

---

## 4. Docker Image Build

Two Docker images are built.

* Application Image
* Database Image

This ensures both services are packaged independently.

---

## 5. Security Scan using Trivy

Both Docker images are scanned using Trivy before deployment.

Commands Used

```bash
trivy image appimage

trivy image dbimage
```

This validates the images for known vulnerabilities before publishing.

---

## 6. Docker Image Tagging

Images are tagged for Docker Hub.

```
bhargavchandra/mynewapp:appimage

bhargavchandra/mynewapp:dbimage
```

---

## 7. Docker Hub Push

Jenkins authenticates with Docker Hub using stored credentials and pushes both images automatically.

---

## 8. Docker Swarm Deployment

Deployment is performed using Docker Stack.

Command Executed

```bash
docker stack deploy dockerstack --compose-file=compose.yml
```

Docker Swarm automatically schedules the containers across available worker nodes.

---

# 🧩 Challenges Faced & Debugging

During the implementation of this project, several practical issues were encountered and resolved.

## 1. Trivy Version Compatibility Issue

### Problem

The default Trivy package available through the operating system repository was an older version and lacked newer features and vulnerability database updates.

### Resolution

* Removed the existing Trivy installation.
* Downloaded the latest stable release from the official GitHub releases.
* Installed the updated binary manually.
* Verified the installation using:

```bash
trivy --version
```

After upgrading, the image scans completed successfully with the latest vulnerability database.

---

## 2. Docker Hub Authentication

### Problem

Docker image push failed due to authentication errors.

### Resolution

* Created Docker Hub credentials inside Jenkins.
* Used Jenkins `withDockerRegistry()` to authenticate securely.

---

## 3. Jenkins Agent Configuration

### Problem

Pipeline required Docker commands that were unavailable on the Jenkins server.

### Resolution

Configured the Docker Swarm Manager as a Jenkins Agent using SSH.

The pipeline executes directly on the manager node where Docker Engine and Docker Swarm are installed.

---

## 4. Docker Swarm Deployment

### Problem

Initially the application containers were not distributed correctly.

### Resolution

Verified:

* Swarm Cluster Status
* Overlay Network
* Service Replicas
* Docker Stack Deployment

After validation Docker Swarm successfully distributed the replicas across the worker nodes.

---

## 5. SonarQube Integration

Configured Jenkins with SonarQube using:

* Jenkins Global Tool Configuration
* SonarQube Server Configuration
* Authentication Token

This enabled automatic code quality analysis during every pipeline execution.

---

# 📂 Repository Structure


├── Docker-app/
│   └── Dockerfile
│
├── Docker-db/
│   └── Dockerfile
│
├── screenshots/
│
├── src/
│
├── Jenkinsfile
├── compose.yml
├── pom.xml
└── README.md


---

# ✅ Project Outcomes

* Successfully implemented an end-to-end CI/CD pipeline.
* Automated code checkout from GitHub.
* Integrated SonarQube for static code analysis.
* Built the Java application using Maven.
* Generated deployable WAR artifacts.
* Built Docker images for both application and database.
* Performed vulnerability scanning using Trivy.
* Published Docker images to Docker Hub.
* Deployed the application to a Docker Swarm Cluster.
* Configured Jenkins to execute the pipeline on a remote Docker Swarm Manager through SSH.
* Docker Swarm automatically scheduled containers across multiple worker nodes.
* Improved deployment consistency by automating the entire build and release workflow.
* Gained hands-on experience troubleshooting infrastructure, security scanning, and container orchestration issues.

---

# 🚀 Future Enhancements

* Integrate Nexus Repository Manager.
* Deploy to Kubernetes using Amazon EKS.
* Implement GitOps using Argo CD.
* Use Helm Charts for deployments.
* Integrate Prometheus and Grafana for monitoring.
* Configure Slack and Email notifications.
* Add automated rollback strategies.
* Implement Blue-Green and Canary deployments.

---

# 👨‍💻 Author

**Bhargav Chandra**

Software Engineer | DevOps Engineer | AWS | Jenkins | Docker | Kubernetes | Terraform | Ansible | CI/CD

---

## ⭐ If you found this project helpful, feel free to star the repository.
