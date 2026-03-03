#####################################################

**🌟 Chai Kafe – Full DevOps CI/CD Implementation**

#####################################################

Chai Kafe is a Spring Boot-based web application that demonstrates a complete DevOps CI/CD pipeline using:

GitHub (Source Control)

Maven (Build Tool)

Docker (Containerization)

AWS EC2 (Server)

AWS ECR (Container Registry)

Jenkins (CI/CD Automation)

IAM Role (Secure AWS Access)

Amazon EKS (Kubernetes)

ArgoCD (GitOps Deployment)

AWS ALB (Ingress Controller)

AWS ACM (SSL Certificate)

Route53 (Domain Management)

##########################################
**🏗️ Architecture**
##########################################

#**Developer → GitHub → Jenkins → Docker → AWS ECR → ArgoCD → EKS → ALB → Route53 → ACM (HTTPS)**
#**🛠️ Step 1: Application Development**

Created Spring Boot application
Added dependencies:

Spring Web

Spring Boot Actuator

Thymeleaf

Created simple café portal endpoints:

/ → Home Page

/menu → Displays available menu items

/about → About Chai Kafe

/contact → Contact information

Verified locally using:

http://localhost:8080
http://localhost:8080/menu
http://localhost:8080/about
http://localhost:8080/contact

#**🐳 Step 2: Dockerization**
Basic Dockerfile
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY target/task-manager-api-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
Multi-Stage Dockerfile (Optimized Production Build)

Stage 1 – Build

FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

Stage 2 – Runtime

FROM eclipse-temurin:17-jre-jammy
WORKDIR /app
COPY --from=builder /app/target/task-manager-api-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]

#**☁️ Step 3: AWS Setup**
EC2 Configuration

Amazon Linux

Docker installed

Jenkins installed

Git installed

IAM Role attached to EC2

IAM Role Permissions:

AmazonEC2ContainerRegistryFullAccess

Verified IAM:

aws sts get-caller-identity
**📦 Step 4: AWS ECR**

Created repository:

aws ecr create-repository --repository-name chai-kafe-app

Logged in:

aws ecr get-login-password --region us-east-1 \
| docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

#**🔄 Step 5: Jenkins CI/CD Pipeline**

Created Jenkinsfile with stages:

Clone Repository

Maven Build

SonarQube Scan

Quality Gate Validation

Docker Build

Trivy Image Scan

ECR Login

Docker Tag

Docker Push

Update Kubernetes Deployment YAML (GitOps)

ArgoCD Sync

Pipeline fully automated.

#**☸️ Phase 6: Kubernetes Deployment (Amazon EKS)**

Manual initial deployment of image to EKS.

Created:

Kubernetes Namespace → chai-kafe

Deployment (2 replicas)

Service (ClusterIP)

Deployment Highlights:

2 replicas for high availability

Port 8080 container

Service exposes port 80 internally

#**🌐 Phase 7: Ingress + ALB Setup (Production Grade)**

Installed AWS Load Balancer Controller using Helm.

Created Kubernetes Ingress:

Internet-facing ALB

Target type: IP

HTTP (80) + HTTPS (443)

SSL redirect enabled

ACM certificate attached

ALB created:

k8s-chaikafe-chaikafe-xxxxx.us-east-1.elb.amazonaws.com
#**🔐 Phase 8: SSL + Domain Configuration**

Domain: chaikafe.in

Created ACM certificate

DNS validation completed

Route53 A (Alias) record mapped to ALB

HTTPS enabled

Application accessible at:

https://chaikafe.in
https://www.chaikafe.in
#**🔍 Phase 9: Application Testing on EKS**

Verification Commands:

kubectl get pods -n chai-kafe
kubectl get svc -n chai-kafe
kubectl get ingress -n chai-kafe
kubectl logs -f deploy/chai-kafe-app -n chai-kafe

External Access:

Accessed via ALB DNS

Verified HTTP → HTTPS redirect

Verified SSL working properly




