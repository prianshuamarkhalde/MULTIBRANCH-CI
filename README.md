# 🚀 Advanced Multi-Branch CI/CD Pipeline

## 🌟 Overview

This project implements an automated **CI/CD + DevSecOps pipeline** for a Java Spring Boot application using modern DevOps tools and cloud-native technologies.

The pipeline automates the complete software delivery lifecycle — from **source code checkout to production deployment on Amazon EKS**. 🔄☁️

It integrates:

* 🐙 **GitHub** — Source Code Management
* 🔨 **Jenkins** — CI/CD Automation
* ☕ **Maven** — Build & Testing
* 🔍 **SonarQube** — Code Quality Analysis
* 🛡️ **OWASP Dependency-Check** — Dependency Security
* 🐳 **Docker** — Containerization
* 🔐 **Trivy** — Container Security Scanning
* 📦 **Docker Hub** — Container Registry
* ☸️ **Kubernetes** — Container Orchestration
* ☁️ **Amazon EKS** — Cloud Kubernetes Platform

---

# 🎯 Project Objectives

The main objectives of this project are:

✅ Automate the complete software delivery lifecycle
✅ Implement Continuous Integration using Jenkins
✅ Automate Maven builds and unit testing
✅ Perform static code analysis
✅ Detect vulnerable dependencies
✅ Enforce code-quality standards using Quality Gates
✅ Build and version Docker images
✅ Scan container images for vulnerabilities
✅ Push images to Docker Hub
✅ Automatically deploy applications to Amazon EKS
✅ Implement DevSecOps practices throughout the pipeline

---

# 🛠️ Technology Stack

| 🔧 Category             | 💻 Technology          |
| ----------------------- | ---------------------- |
| 📂 Source Code          | GitHub                 |
| ⚙️ CI/CD                | Jenkins                |
| ☕ Programming           | Java                   |
| 🔨 Build Tool           | Apache Maven           |
| 🔎 Code Quality         | SonarQube / SonarCloud |
| 🛡️ Dependency Security | OWASP Dependency-Check |
| 🐳 Containerization     | Docker                 |
| 🔐 Container Security   | Trivy                  |
| 📦 Container Registry   | Docker Hub             |
| ☸️ Orchestration        | Kubernetes             |
| ☁️ Cloud Platform       | AWS                    |
| 🚀 Kubernetes Service   | Amazon EKS             |
| 📜 Configuration        | YAML                   |
| 🔄 Pipeline             | Jenkinsfile            |

---

# 🏗️ Project Architecture

```text
                    👨‍💻 Developer
                         |
                         v
                  🐙 GitHub Repository
                         |
                         v
                   ⚙️ Jenkins
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
      🔨 Maven       🧪 Unit Tests   🛡️ Security
          |              |              |
          +--------------+--------------+
                         |
                         v
                  🔍 SonarQube
                         |
                         v
                  🚦 Quality Gate
                         |
                         v
                    🐳 Docker
                         |
                         v
                   🔐 Trivy Scan
                         |
                         v
                   📦 Docker Hub
                         |
                         v
                    ☁️ AWS EKS
                         |
                         v
                  ☸️ Kubernetes
                         |
                         v
                  🌐 Application
```

---

# 🔄 CI/CD Pipeline

The complete pipeline follows this workflow:

```text
👨‍💻 Developer
      ↓
🐙 GitHub
      ↓
⚙️ Jenkins
      ↓
📥 Checkout
      ↓
🔨 Maven Build
      ↓
🧪 Unit Testing
      ↓
🛡️ OWASP Dependency Check
      ↓
🔍 SonarQube Analysis
      ↓
🚦 Quality Gate
      ↓
🐳 Docker Build
      ↓
🔐 Trivy Security Scan
      ↓
📦 Docker Hub Push
      ↓
☁️ Amazon EKS
      ↓
☸️ Kubernetes Deployment
      ↓
🌐 Running Application
```

---

# 📂 Project Structure

```text
MULTIBRANCH-CI/
│
├── 📁 src/
│   └── ☕ Application Source Code
│
├── 🐳 Dockerfile
├── ⚙️ Jenkinsfile
├── 🔨 pom.xml
├── ☸️ deployment.yaml
├── 🌐 service.yaml
├── 🔍 sonar-project.properties
└── 📖 README.md
```

---

# 📌 Important Files

### ⚙️ Jenkinsfile

Contains the complete CI/CD pipeline configuration.

It defines all automated stages including:

🔹 Build
🔹 Testing
🔹 Security Scanning
🔹 SonarQube Analysis
🔹 Docker Build
🔹 Trivy Scan
🔹 Docker Hub Push
🔹 EKS Deployment

---

### 🔨 pom.xml

Maven project configuration containing:

* ☕ Java configuration
* 📦 Application dependencies
* 🧪 Testing configuration
* 📊 JaCoCo code coverage
* 🔨 Build plugins

---

### 🐳 Dockerfile

Used to create the container image for the Spring Boot application.

```dockerfile
FROM eclipse-temurin:21-jre

COPY target/*.jar app.jar

ENTRYPOINT ["java","-jar","/app.jar"]
```

---

### ☸️ deployment.yaml

Defines the Kubernetes Deployment and application replicas.

The deployment runs:

```yaml
replicas: 3
```

This provides multiple application instances for improved availability. 🚀

---

### 🌐 service.yaml

Creates a Kubernetes `LoadBalancer` service.

```yaml
type: LoadBalancer
```

This allows the application to be exposed externally through AWS. ☁️

---

# 🔨 Pipeline Stages

## 1️⃣ Checkout 📥

Jenkins retrieves the source code from GitHub.

```groovy
stage('Checkout') {
    steps {
        checkout scm
    }
}
```

---

## 2️⃣ Build 🔨

Maven builds the Spring Boot application.

```bash
mvn clean package
```

The generated `.jar` file is later used to create the Docker image.

---

## 3️⃣ Unit Testing 🧪

Automated tests are executed using Maven.

```bash
mvn test
```

This ensures that the application passes functional tests before deployment.

---

## 4️⃣ OWASP Dependency-Check 🛡️

OWASP Dependency-Check scans application dependencies for known vulnerabilities.

```text
📦 Dependencies
      ↓
🛡️ OWASP Scan
      ↓
⚠️ Vulnerabilities
      ↓
📊 Security Report
```

This helps identify vulnerable third-party libraries before deployment.

---

## 5️⃣ SonarQube Analysis 🔍

SonarQube performs static code analysis.

It identifies:

🐞 Bugs
🔐 Vulnerabilities
🧹 Code Smells
🔥 Security Hotspots
📊 Maintainability Issues
📈 Code Quality Problems

---

## 6️⃣ Quality Gate 🚦

After SonarQube analysis, Jenkins waits for the Quality Gate result.

```groovy
timeout(time: 5, unit: 'MINUTES') {
    waitForQualityGate abortPipeline: true
}
```

If the Quality Gate fails ❌, the pipeline stops.

If it passes ✅, the pipeline continues.

---

## 7️⃣ Docker Build 🐳

Jenkins creates a Docker image containing the Spring Boot application.

Example:

```bash
docker build -t prianshuamarkhalde/devsecops-nexus:latest .
```

The image is tagged using the Jenkins build number.

```text
prianshuamarkhalde/devsecops-nexus:<BUILD_NUMBER>
```

This provides unique versions for different pipeline executions. 🔢

---

## 8️⃣ Trivy Security Scan 🔐

Trivy scans the Docker image for security vulnerabilities.

```bash
trivy image \
    --severity HIGH,CRITICAL \
    --exit-code 0 \
    "${IMAGE_NAME}"
```

This adds an additional security layer before publishing the container image. 🛡️

---

## 9️⃣ Docker Hub Push 📦

The Docker image is pushed to Docker Hub.

Two tags are maintained:

```text
🏷️ BUILD_NUMBER
🏷️ latest
```

Example:

```text
prianshuamarkhalde/devsecops-nexus:25
prianshuamarkhalde/devsecops-nexus:latest
```

---

# ☁️ Amazon EKS Deployment

After successful CI/CD and security checks, Jenkins deploys the application to **Amazon EKS**.

First, Jenkins updates the Kubernetes configuration:

```bash
aws eks update-kubeconfig \
    --region us-east-1 \
    --name my-eks-cluster
```

Then Kubernetes manifests are applied:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

# ☸️ Kubernetes Configuration

The application is deployed inside the Kubernetes namespace:

```text
myapp
```

### Deployment

The application uses:

```text
🔢 Replicas: 3
🏷️ App Label: nexus-app
```

Multiple replicas provide better availability and allow Kubernetes to distribute workloads across nodes.

### Service

The application is exposed through:

```text
🌐 Service Type: LoadBalancer
```

AWS can provision an external endpoint through the Kubernetes LoadBalancer integration.

---

# 🔐 DevSecOps Integration

Security is integrated throughout the CI/CD lifecycle rather than being performed only at the end.

```text
💻 Source Code
      ↓
🔨 Build
      ↓
🧪 Unit Testing
      ↓
🛡️ Dependency Scan
      ↓
🔍 SonarQube
      ↓
🚦 Quality Gate
      ↓
🐳 Docker Build
      ↓
🔐 Trivy Scan
      ↓
📦 Container Registry
      ↓
☁️ Amazon EKS
```

This approach helps identify vulnerabilities and quality issues early in the development lifecycle. 🚀

---

# ⚙️ Jenkins Configuration

The pipeline uses a declarative Jenkinsfile.

The Docker image is dynamically tagged using the Jenkins build number:

```groovy
environment {
    IMAGE_NAME = "prianshuamarkhalde/devsecops-nexus:${BUILD_NUMBER}"
}
```

This ensures that every successful build can have its own identifiable container version. 🏷️

---

# 🔑 Required Jenkins Credentials

The pipeline requires appropriate credentials to access external services.

### 🐳 Docker Hub

Credential ID:

```text
credentials-dockerhub
```

### ☁️ AWS

AWS credentials must allow Jenkins to interact with Amazon EKS.

### 🔍 SonarQube

SonarQube must be configured in Jenkins with the appropriate server URL and authentication.

---

# 📋 Prerequisites

Before running the project, make sure the following tools are installed:

```text
🐙 Git
☕ Java
🔨 Maven
⚙️ Jenkins
🐳 Docker
🔍 SonarQube
🛡️ OWASP Dependency-Check
🔐 Trivy
☁️ AWS CLI
☸️ kubectl
📦 Docker Hub
```

You also need:

* ☁️ AWS account
* ☸️ Amazon EKS cluster
* 🔑 Required IAM permissions
* 🔐 Jenkins credentials
* 🐳 Docker Hub account

---

# 🚀 Running the Project

## 1️⃣ Clone Repository

```bash
git clone https://github.com/prianshuamarkhalde/MULTIBRANCH-CI.git
```

```bash
cd MULTIBRANCH-CI
```

---

## 2️⃣ Build Application

```bash
mvn clean package
```

---

## 3️⃣ Run Tests

```bash
mvn test
```

---

## 4️⃣ Build Docker Image

```bash
docker build -t prianshuamarkhalde/devsecops-nexus:latest .
```

---

## 5️⃣ Run Docker Container

```bash
docker run -p 80:80 prianshuamarkhalde/devsecops-nexus:latest
```

---

# ☸️ Deploy to Kubernetes

Configure access to the EKS cluster:

```bash
aws eks update-kubeconfig \
    --region us-east-1 \
    --name my-eks-cluster
```

Apply the deployment:

```bash
kubectl apply -f deployment.yaml
```

Apply the service:

```bash
kubectl apply -f service.yaml
```

---

# 🔎 Verify Kubernetes Deployment

### 📦 Check Pods

```bash
kubectl get pods -n myapp
```

### 🚀 Check Deployment

```bash
kubectl get deployments -n myapp
```

### 🌐 Check Service

```bash
kubectl get svc -n myapp
```

### 📋 Check Logs

```bash
kubectl logs -n myapp <pod-name>
```

### 🔍 Describe Deployment

```bash
kubectl describe deployment nexus-deployment -n myapp
```

---

# 📊 Monitoring Commands

Useful commands for checking the application:

```bash
kubectl get pods -n myapp
```

```bash
kubectl get svc -n myapp
```

```bash
kubectl get deployments -n myapp
```

```bash
kubectl get nodes
```

```bash
kubectl get all -n myapp
```

---

# 🌟 Key Features

| 🚀 Feature                 | ✅ Implementation |
| -------------------------- | ---------------- |
| 🔄 Continuous Integration  | Jenkins          |
| 🐙 Source Control          | GitHub           |
| 🔨 Automated Build         | Maven            |
| 🧪 Automated Testing       | JUnit/Maven      |
| 🛡️ Dependency Scanning    | OWASP            |
| 🔍 Code Analysis           | SonarQube        |
| 🚦 Quality Gate            | SonarQube        |
| 🐳 Containerization        | Docker           |
| 🔐 Image Scanning          | Trivy            |
| 📦 Image Registry          | Docker Hub       |
| ☸️ Container Orchestration | Kubernetes       |
| ☁️ Cloud Deployment        | Amazon EKS       |
| 🌐 Application Exposure    | LoadBalancer     |

---

# 💡 Benefits

### ⚡ Automation

Reduces manual effort by automating build, testing, security, and deployment.

### 🛡️ Security

Security scanning is integrated directly into the CI/CD pipeline.

### 🔄 Faster Delivery

Automated pipelines allow faster and more reliable software releases.

### 📦 Consistency

Docker provides consistent application environments across development and deployment.

### ☁️ Scalability

Amazon EKS and Kubernetes provide scalable container orchestration.

### 🚦 Quality Control

SonarQube Quality Gates prevent low-quality code from progressing through the pipeline.

---

# 🔮 Future Enhancements

The project can be extended with:

* 🌐 GitHub Webhooks
* 🏗️ Terraform for Infrastructure as Code
* 🤖 Ansible for Configuration Management
* 📊 Prometheus & Grafana Monitoring
* 📜 Centralized Logging
* 🔐 Kubernetes Secrets
* 📈 Horizontal Pod Autoscaling
* 🌍 Kubernetes Ingress
* 🔒 HTTPS/TLS
* 🔄 Blue-Green Deployment
* 🐤 Canary Deployment
* 🔥 Argo CD / GitOps
* ☁️ AWS ECR
* 🔔 Slack/Email Notifications
* ↩️ Automated Rollback
* 📦 SBOM Generation

---

# 🎓 Project Learning Outcomes

Through this project, the following DevOps and DevSecOps concepts are demonstrated:

✅ CI/CD Pipeline Automation
✅ Jenkins Pipeline Development
✅ Git & GitHub Integration
✅ Maven Build Automation
✅ Automated Testing
✅ Static Code Analysis
✅ Dependency Vulnerability Scanning
✅ Docker Containerization
✅ Container Security
✅ Kubernetes Deployment
✅ Amazon EKS
✅ Cloud Deployment
✅ DevSecOps Practices

---

# 🏆 Conclusion

This project demonstrates a complete **end-to-end DevSecOps CI/CD pipeline** using modern DevOps tools and cloud-native technologies.

The integration of:

**🐙 GitHub → ⚙️ Jenkins → 🔨 Maven → 🛡️ OWASP → 🔍 SonarQube → 🐳 Docker → 🔐 Trivy → 📦 Docker Hub → ☁️ Amazon EKS**

creates an automated, secure, scalable, and reliable software delivery workflow. 🚀

The project showcases practical implementation of **Continuous Integration, Continuous Delivery, DevSecOps, Containerization, Kubernetes, and Cloud Computing**.

---

# 👨‍💻 Author

## **Prianshu Amar Khalde**

🎓 B.Tech Computer Science & Engineering
💻 DevOps / DevSecOps Project
☁️ Cloud & Kubernetes Enthusiast

### 🔗 GitHub

https://github.com/prianshuamarkhalde

### 📂 Project Repository

https://github.com/prianshuamarkhalde/MULTIBRANCH-CI

---

# ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub!

**Built with ❤️ using DevOps & DevSecOps practices 🚀**
