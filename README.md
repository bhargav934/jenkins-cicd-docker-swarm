# 🚀 End-to-End CI/CD Pipeline using Jenkins, SonarQube, Docker & Docker Swarm

## 📌 Project Overview

This project demonstrates an end-to-end Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Java web application using Jenkins. The pipeline automates the complete software delivery lifecycle, including source code checkout, code quality analysis, application build, Docker image creation, security scanning, image publishing to Docker Hub, and deployment to a Docker Swarm cluster.

The objective of this project is to implement a production-style CI/CD workflow using industry-standard DevOps tools and cloud infrastructure.

---

# 🏗 Infrastructure

The project was deployed on Amazon EC2 using four instances.

| Instance             | Type     | Purpose                                                            |
| -------------------- | -------- | ------------------------------------------------------------------ |
| Jenkins Server       | c7i-flex | Runs Jenkins and triggers the CI/CD pipeline                       |
| Docker Swarm Manager | m7i-flex | Configured as a Jenkins Agent and manages the Docker Swarm Cluster |
| Worker Node 1        | t3.micro | Runs application containers                                        |
| Worker Node 2        | t3.micro | Runs application containers                                        |



# 🖥 Architecture


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
         ┌─────────────────────┴─────────────────────┐
         │                                           │
         ▼                                           ▼
Docker Image Build                           Docker Stack Deploy
         │                                           │
         ▼                                           ▼
 Docker Hub Repository                   Docker Swarm Scheduler
                                                     │
                           ┌─────────────────────────┴─────────────────────────┐
                           ▼                                                   ▼
                 Worker Node 1 (t3.micro)                         Worker Node 2 (t3.micro)
                           │                                                   │
                           └────────────── Java Application Containers ─────────┘



# ⚙ How the Architecture Works

1. The application source code is stored in GitHub.
2. Jenkins automatically checks out the source code.
3. The Docker Swarm Manager is configured as a Jenkins Agent using SSH.
4. Jenkins executes the entire pipeline on the Swarm Manager.
5. Maven builds the Java application.
6. SonarQube performs static code analysis.
7. Docker builds images for the application and database.
8. Trivy scans the Docker images for vulnerabilities.
9. Docker images are tagged and pushed to Docker Hub.
10. Jenkins executes `docker stack deploy`.
11. The Swarm Manager schedules the containers across the worker nodes automatically.



# 🛠 Technology Stack

* AWS EC2
* Jenkins
* Git
* GitHub
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



# 🔄 CI/CD Pipeline Workflow

GitHub
   │
   ▼
Source Code Checkout
   │
   ▼
Code Quality Analysis (SonarQube)
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
Trivy Security Scan
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
Running Java Application




# 📋 Jenkins Pipeline Stages

## 1️⃣ Source Code Checkout

The pipeline starts by cloning the latest application source code from GitHub.

**Tool Used**

* Git


## 2️⃣ Code Quality Analysis

Jenkins integrates with SonarQube to perform static code analysis.

This stage checks for:

* Bugs
* Code Smells
* Vulnerabilities
* Maintainability Issues

**Tool Used**

* SonarQube


## 3️⃣ Maven Build

The application is compiled using Maven.

The build process:

* Downloads dependencies
* Compiles Java code
* Executes tests
* Generates the WAR file

The generated WAR file is copied into the Docker build context.

**Tool Used**

* Maven

---

## 4️⃣ Docker Image Build

Two Docker images are created.

* Application Image
* Database Image

These images package the complete runtime environment required by the application.

**Tool Used**

* Docker

---

## 5️⃣ Security Scan

Trivy scans both Docker images for known vulnerabilities before deployment.

This ensures only scanned images are pushed to Docker Hub.

**Tool Used**

* Trivy

---

## 6️⃣ Docker Image Tagging

Both Docker images are tagged with Docker Hub repository names.

Example:

* bhargavchandra/mynewapp:appimage
* bhargavchandra/mynewapp:dbimage

---

## 7️⃣ Docker Hub Push

Jenkins authenticates using stored Docker Hub credentials and pushes both images.

This makes the images available for deployment from any Docker host.

---

## 8️⃣ Docker Swarm Deployment

The pipeline executes:

```bash
docker stack deploy dockerstack --compose-file=compose.yml
```

The Docker Swarm Manager creates the services defined in the Compose file.

Docker Swarm automatically distributes the application containers across the worker nodes.

---

# 📁 Repository Structure


.
├── Docker-app/
│   └── Dockerfile
│
├── Docker-db/
│   └── Dockerfile
│
├── screenshots/
│   ├── EC2-Instances.png
│   ├── Jenkins-Dashboard.png
│   ├── Jenkins-Pipeline.png
│   ├── SonarQube.png
│   ├── DockerHub.png
│   └── Application.png
│
├── src/
│
├── Jenkinsfile
├── compose.yml
├── pom.xml
└── README.md


---

# 📸 Project Screenshots

## EC2 Infrastructure

Add screenshot of all EC2 instances.

---

## Jenkins Dashboard

Add Jenkins dashboard screenshot.

---

## Jenkins Pipeline

Add pipeline execution screenshot.

---

## SonarQube Dashboard

Add SonarQube analysis report.

---

## Docker Hub Repository

Add Docker Hub repository screenshot.

---

## Running Application

Add browser screenshot showing the deployed application.

---

# ✅ Project Outcomes

* Automated CI/CD pipeline using Jenkins.
* Static code analysis using SonarQube.
* Java application built with Maven.
* Docker images created automatically.
* Docker images scanned with Trivy.
* Images pushed to Docker Hub.
* Automated deployment to Docker Swarm.
* Multi-node container deployment using Docker Stack.

---

# 🚀 Future Enhancements

* Jenkins Shared Libraries
* Nexus Artifact Repository
* Kubernetes Deployment
* Amazon EKS
* Helm Charts
* Argo CD GitOps
* Prometheus Monitoring
* Grafana Dashboards
* Slack Notifications
* Email Notifications
* Automated Rollback Strategy

---

# 👨‍💻 Author

**Bhargav Chandra**

DevOps | AWS | Docker | Kubernetes | Jenkins | Terraform | Ansible | CI/CD
