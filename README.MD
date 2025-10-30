# 🚀 CICD Pipeline

This project implements a **complete end-to-end CI/CD pipeline** using **GitHub Actions**, automating everything from compilation and security checks to deployment on AWS EKS (Kubernetes).

---

## 🧩 Overview

The workflow runs automatically whenever code is pushed to the **`main`** branch.  
It covers the following stages:

| Stage | Purpose |
|--------|----------|
| **1. Compile** | Compiles the Java project using Maven with JDK 17. |
| **2. Security Check** | Scans the source code and dependencies for vulnerabilities using **Trivy** and **Gitleaks**. |
| **3. Unit Testing** | Runs all unit test cases using `mvn test` to ensure code quality. |
| **4. Build & SonarQube Analysis** | Packages the app, performs static code analysis using **SonarQube/SonarCloud**, and uploads the build artifact. |
| **5. Docker Build & Push** | Builds a Docker image from the packaged JAR and pushes it to **Docker Hub**. |
| **6. Deploy to Kubernetes (AWS EKS)** | Deploys the Docker image to **AWS Elastic Kubernetes Service (EKS)** using `kubectl`. |

---

## ⚙️ Workflow Breakdown

### 🏗️ 1. **Compile**
- Sets up **JDK 17 (Temurin)**.
- Runs Maven compile.
- Ensures project dependencies and structure are valid before proceeding.

### 🔒 2. **Security Check**
- **Trivy:** Scans file system for known CVEs.
- **Gitleaks:** Detects potential hardcoded secrets in the source code.

### 🧪 3. **Testing**
- Executes `mvn test`.
- Verifies all unit tests pass successfully before build and deployment.

### 🧰 4. **Build & SonarQube Scan**
- Builds the application JAR using Maven.
- Uploads build artifact for later stages.
- Performs static code analysis via **SonarQube/SonarCloud**.
- Enforces **Quality Gate** checks to ensure code meets defined standards.

### 🐳 5. **Docker Build & Push**
- Downloads the JAR artifact.
- Builds a Docker image using `Dockerfile`.
- Logs into **Docker Hub** using secrets.
- Pushes the image with tag: `latest`.

### ☸️ 6. **Deploy to Kubernetes (AWS EKS)**
- Configures AWS CLI and Kubernetes context.
- Applies the deployment file `ds.yml` to deploy the app on EKS.

---

## 🔑 Secrets and Variables

| Name | Type | Purpose |
|------|------|----------|
| `SONAR_TOKEN` | Secret | Token for authenticating with SonarQube/SonarCloud |
| `SONAR_HOST_URL` | Variable | URL of SonarQube/SonarCloud instance |
| `DOCKERHUB_USERNAME` | Variable | Docker Hub username |
| `DOCKERHUB_TOKEN` | Secret | Docker Hub access token |
| `AWS_ACCESS_KEY_ID` | Secret | AWS Access Key |
| `AWS_SECRET_ACCESS_KEY` | Secret | AWS Secret Key |
| `EKS_KUBECONFIG` | Secret | Base64-encoded kubeconfig for EKS cluster |

---

## 📦 Tools & Technologies

- **GitHub Actions** – CI/CD Automation
- **Maven** – Build and Dependency Management
- **SonarQube / SonarCloud** – Static Code Analysis
- **Trivy & Gitleaks** – Security and Secret Scanning
- **Docker & Docker Hub** – Containerization and Image Hosting
- **AWS EKS** – Kubernetes Deployment

---

## 🧠 Notes

- The pipeline runs on a **self-hosted GitHub runner**.
- Each stage depends on the success of the previous one (`needs` keyword ensures order).
- The Docker image is tagged as `latest` — version tagging can be added later for release pipelines.

---

## 📈 Workflow Trigger

```yaml
on:
  push:
    branches: [ "main" ]
