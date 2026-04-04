# E-Commerce Application — End-to-End DevSecOps Project

A complete DevSecOps project demonstrating end-to-end deployment of a Java-based E-Commerce Application using Jenkins, Docker, Kubernetes, and AWS — with full CI/CD pipeline from GitHub push to live deployment.

---

## Architecture Overview

> Full pipeline: GitHub → Jenkins → Docker → Docker Hub → Kubernetes (EKS) → Live App

---

## Tech Stack

| Tool | Purpose | Server |
|------|---------|--------|
| GitHub | Source Code Management | Cloud |
| Jenkins | CI/CD Automation | AWS EC2 - Jenkins Server |
| Maven | Build Tool (Java) | Jenkins Server |
| SonarQube | Static Code Analysis (SAST) | AWS EC2 - SonarQube Server |
| Trivy | Container & Filesystem Security Scan | Jenkins Server |
| Docker | Containerization | AWS EC2 - Jenkins Server |
| Docker Hub | Docker Registry | Cloud |
| AWS EKS | Kubernetes Cluster | Cloud |
| Kubernetes | Container Orchestration | EKS Cluster |
| AWS EC2 | Cloud Infrastructure | AWS |

---

## CI/CD Pipeline Flow

```
Step 1: Developer pushes code to GitHub
            ↓
Step 2: GitHub Webhook triggers Jenkins automatically
            ↓
Step 3: Jenkins pulls latest code from GitHub
            ↓
Step 4: Maven builds the Java application
        mvn clean package
        • BUILD SUCCESS → Continue pipeline
        • BUILD FAILURE → Pipeline stops
            ↓
Step 5: SonarQube — Static Code Analysis
        • Scans source code for bugs, vulnerabilities & code smells
        • Quality Gate PASSED → Continue pipeline
        • Quality Gate FAILED → Pipeline stops
            ↓
Step 6: Trivy — Filesystem Security Scan
        trivy fs --format table -o trivy-fs-report.html .
        • Scans project files & dependencies for known CVEs
        • HIGH/CRITICAL issues flagged before build
            ↓
Step 7: Jenkins builds Docker image
        docker build -t ecommerce-app:${BUILD_NUMBER}
            ↓
Step 8: Trivy — Docker Image Security Scan
        trivy image --format table -o trivy-image-report.html ecommerce-app:${BUILD_NUMBER}
        • Scans built image layers for vulnerabilities
            ↓
Step 9: Image pushed to Docker Hub
            ↓
Step 10: Jenkins connects to AWS EKS cluster
            ↓
Step 11: Kubernetes deploys latest image
        kubectl apply -f deployment-service.yaml
            ↓
Step 12: App is accessible via Kubernetes Service / Load Balancer
        App is LIVE!
```

---

## Infrastructure Setup

**Jenkins Server**
- OS: Ubuntu 24.04 LTS
- Cloud: AWS EC2
- Installed: Jenkins, Java 17, Maven, Docker, AWS CLI, kubectl, Trivy
- Port: 8080

**SonarQube Server**
- OS: Ubuntu 24.04 LTS
- Cloud: AWS EC2
- Installed: SonarQube, Java 17
- Port: 9000

**EKS Cluster**
- Cluster Name: ecommerce-cluster
- Region: us-east-1
- Worker Nodes: 3 × c7i-flex.large

---

## Security — DevSecOps Tools

### 🔍 SonarQube — Static Code Analysis (SAST)

SonarQube inspects the Java source code **before** the Docker image is built.

| What it checks | Example |
|----------------|---------|
| Bugs | Null pointer dereferences, logic errors |
| Vulnerabilities | SQL injection, hardcoded credentials |
| Code Smells | Dead code, poor naming, complexity |
| Code Coverage | % of code covered by tests |



---

### 🛡️ Trivy — Container & Filesystem Security Scan

Trivy scans for CVEs (Common Vulnerabilities & Exposures) at **two stages** in the pipeline.

| Scan Type | When | Command |
|-----------|------|---------|
| Filesystem Scan | After Maven build | `trivy fs --format table -o trivy-fs-report.html .` |
| Docker Image Scan | After Docker build | `trivy image --format table -o trivy-image-report.html <image>` |

**What Trivy detects:**
- OS package vulnerabilities (Ubuntu, Alpine, etc.)
- Application dependency vulnerabilities (Maven, npm, pip)
- Misconfigurations in Dockerfile
- Exposed secrets and credentials in code

**Severity Levels:**
```
CRITICAL  →  Must fix before deployment
HIGH      →  Should fix before deployment
MEDIUM    →  Fix in next sprint
LOW       →  Monitor and track
```


---

## Project Structure

```
Ecommerce-App-Kastro/
│
├── src/
│   └── main/
│       ├── java/                  # Java source code
│       └── webapp/                # JSP frontend files
├── Dockerfile                     # Docker configuration
├── deployment-service.yaml        # Kubernetes deployment & service
├── pom.xml                        # Maven build configuration
└── README.md
```

---

## Key Learnings

- Setting up Jenkins on AWS EC2 from scratch
- GitHub Webhook integration for auto-trigger on every push
- Building a Java/Maven application in a CI/CD pipeline
- Integrating SonarQube for static code analysis and Quality Gate enforcement
- Installing and configuring Trivy for filesystem and Docker image vulnerability scanning
- Dockerizing a Java web application (WAR deployment)
- Pushing Docker images to Docker Hub
- Writing Kubernetes deployment and service manifests
- Zero-downtime rolling deployments with Kubernetes
- Connecting Java app to MySQL database on cloud
- Implementing DevSecOps — shifting security left in the pipeline

---

## Screenshots

> **Jenkins — Pipeline Success**
> *<img width="1343" height="609" alt="Screenshot_3" src="https://github.com/user-attachments/assets/105bbd92-b375-441b-9933-39c98763693a" />*

> **SonarQube — Code Analysis Report**
> *<img width="1357" height="684" alt="Screenshot_1" src="https://github.com/user-attachments/assets/39bb0a0e-079c-4e80-a9fa-64f966464a37" />*

> **EKS Cluster — Running Pods**
> *<img width="1343" height="353" alt="Screenshot_4" src="https://github.com/user-attachments/assets/97030b40-2cbd-49ad-89fe-3ea5d0ddfdb6" />*

> **App Live on Browser**
> *<img width="1348" height="691" alt="Screenshot_6" src="https://github.com/user-attachments/assets/6c4439ed-0644-4aef-ba91-dd353b978536" />*


---

### Deployed by: Akash Ahirwar — DevOps Engineer
