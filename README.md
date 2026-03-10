# 🌟 Chai Kafe – Full DevOps CI/CD Implementation

Chai Kafe is a **Spring Boot-based web application** demonstrating a **complete production-grade DevOps CI/CD pipeline** using modern cloud-native tools and GitOps deployment practices.

The project showcases the full lifecycle of application delivery — from development to automated deployment on Kubernetes using AWS infrastructure.

---

# 🚀 Tech Stack

This project integrates multiple DevOps tools and cloud services:

| Category | Tools Used |
|--------|-------------|
| Source Control | GitHub |
| Build Tool | Maven |
| Containerization | Docker |
| CI/CD Automation | Jenkins |
| Container Registry | AWS ECR |
| Cloud Infrastructure | AWS EC2 |
| Kubernetes | Amazon EKS |
| GitOps | ArgoCD |
| Ingress | AWS ALB Controller |
| SSL Certificate | AWS ACM |
| DNS Management | Route53 |
| Security Scanning | Trivy |
| Code Quality | SonarQube |

---

# 🏗️ Architecture

```
Developer
   ↓
GitHub
   ↓
Jenkins CI/CD Pipeline
   ↓
Docker Image Build
   ↓
AWS ECR (Container Registry)
   ↓
ArgoCD GitOps Deployment
   ↓
Amazon EKS (Kubernetes)
   ↓
AWS ALB Ingress Controller
   ↓
Route53 DNS
   ↓
ACM SSL (HTTPS)
```

---

# 🛠 Step 1: Application Development

Developed a **Spring Boot web application** representing a café portal.

### Dependencies Used

- Spring Web
- Spring Boot Actuator
- Thymeleaf

### Application Endpoints

| Endpoint | Description |
|--------|-------------|
| `/` | Home Page |
| `/menu` | Displays available menu items |
| `/about` | About Chai Kafe |
| `/contact` | Contact information |

### Local Testing

Application verified locally:

```
http://localhost:8080
http://localhost:8080/menu
http://localhost:8080/about
http://localhost:8080/contact
```

---

# 🐳 Step 2: Dockerization

The application was containerized using Docker.

### Basic Dockerfile

```dockerfile
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/task-manager-api-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
```

---

### Multi-Stage Dockerfile (Optimized Production Build)

**Stage 1 – Build**

```dockerfile
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests
```

**Stage 2 – Runtime**

```dockerfile
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app
COPY --from=builder /app/target/task-manager-api-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
```

---

# ☁️ Step 3: AWS Infrastructure Setup

### EC2 Configuration

- Amazon Linux
- Docker installed
- Jenkins installed
- Git installed
- IAM Role attached to EC2

### IAM Role Permissions

```
AmazonEC2ContainerRegistryFullAccess
```

### IAM Verification

```
aws sts get-caller-identity
```

---

# 📦 Step 4: AWS ECR Setup

Created an AWS Elastic Container Registry repository.

### Create Repository

```
aws ecr create-repository --repository-name chai-kafe-app
```

### Login to ECR

```
aws ecr get-login-password --region us-east-1 \
| docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

# 🔄 Step 5: Jenkins CI/CD Pipeline

A fully automated **Jenkins pipeline** was implemented.

### Pipeline Stages

1. Clone GitHub Repository
2. Maven Build
3. SonarQube Code Scan
4. Quality Gate Validation
5. Docker Image Build
6. Trivy Security Scan
7. ECR Authentication
8. Docker Image Tagging
9. Docker Push to ECR
10. Update Kubernetes Deployment YAML
11. ArgoCD GitOps Sync

The pipeline ensures **automated build, security validation, and deployment**.

---

# ☸️ Step 6: Kubernetes Deployment (Amazon EKS)

The application is deployed to **Amazon EKS cluster**.

### Kubernetes Resources Created

- Namespace → `chai-kafe`
- Deployment
- Service

### Deployment Configuration

- 2 replicas for high availability
- Container port: 8080
- Service type: ClusterIP

---

# 🌐 Step 7: Ingress Setup with AWS ALB

Installed **AWS Load Balancer Controller** using Helm.

### Ingress Configuration

- Internet-facing ALB
- Target Type: IP
- HTTP (80) + HTTPS (443)
- Automatic HTTP → HTTPS redirect
- ACM SSL certificate attached

Example ALB:

```
k8s-chaikafe-chaikafe-xxxxx.us-east-1.elb.amazonaws.com
```

---

# 🔐 Step 8: SSL + Domain Configuration

Domain configured using Route53.

### Domain

```
chaikafe.in
```

### Configuration

- Created ACM SSL certificate
- DNS validation completed
- Route53 Alias record mapped to ALB

### Application Access

```
https://chaikafe.in
https://www.chaikafe.in
```

HTTPS is fully enabled.

---

# 🔍 Step 9: Kubernetes Verification

### Kubernetes Commands Used

```
kubectl get pods -n chai-kafe
kubectl get svc -n chai-kafe
kubectl get ingress -n chai-kafe
kubectl logs -f deploy/chai-kafe-app -n chai-kafe
```

### Verification Performed

- Pod status validation
- Service connectivity check
- Ingress routing verification
- HTTP → HTTPS redirect validation
- SSL certificate verification

---

# 🎯 Key DevOps Features Implemented

- Complete CI/CD automation
- Docker containerization
- GitOps deployment with ArgoCD
- Kubernetes orchestration using Amazon EKS
- Infrastructure security scanning
- Code quality validation
- SSL-secured production deployment
- Domain routing using Route53

---

# 📌 Project Outcome

The **Chai Kafe DevOps implementation** demonstrates a production-ready DevOps workflow with automated build, security validation, container deployment, and GitOps-driven Kubernetes delivery.

This project represents a **modern cloud-native DevOps pipeline** suitable for scalable microservice deployments.

---
