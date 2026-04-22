# Securing Docker CI/CD Pipelines with Jenkins & Docker Scout

This project demonstrates how insecure Docker and CI/CD practices can lead to **secret leaks and supply chain risks**, and how to fix them using simple modern DevSecOps techniques.

---

## Overview

* 🔴 An **insecure pipeline** (to demonstrate real vulnerabilities)
* 🟢 A **secure pipeline** (with proper DevSecOps practices)

The goal is to show:

> A working pipeline is not necessarily a secure pipeline.

---

## Architecture

```text
Developer → GitHub → Jenkins Pipeline → Docker Image → Security Scan → Run Container
```

---

## Tech Stack

* Jenkins (CI/CD)
* Docker (Containerization)
* Docker Scout (Security Scanning)
* GitHub (Source Control)

---

## Project Structure

```text
├── insecure-app/        # Vulnerable Docker setup
│   ├── Dockerfile
│   ├── .env            # ❌ Secret exposed
│
├── secure-app/          # Fixed secure setup
│   ├── Dockerfile
│   ├── .dockerignore   # ✅ Prevents secret leakage
│
├── Jenkinsfile-insecure
├── Jenkinsfile-secure
```

---

## ❌ Insecure Practices Demonstrated

* Hardcoded secrets in Dockerfile
* Sensitive files copied into images
* No validation of third-party images
* Blind execution of containers

---

## Demonstration (Insecure)

Secrets can be extracted from image:

```bash
docker history insecure-app
docker run --rm insecure-app cat /app/.env
```

---

## Security Improvements

* Removed hardcoded secrets
* Used `.dockerignore`
* Jenkins credentials for secret management
* Added image scanning using Docker Scout
* Prevented execution of vulnerable images

---

## 🔍 Docker Scout Integration

The pipeline scans images before execution:

```bash
docker scout cves nginx:latest \
  --only-severity critical \
  --exit-code
```

* Fails pipeline if critical vulnerabilities exist
* Ensures safer container execution

---

## ⚙️ How to Run

### 1. Clone Repository

```bash
git clone https://github.com/nightcodex7/insecure_cicd_testing
cd insecure_cicd_testing
```

---

### 2. Setup Jenkins

* Install Jenkins
* Install Docker
* Add credentials:

  * `db-password-id` (Secret Text)
  * `ncxDocker` (DockerHub Username/Password)

---

### 3. Run Pipelines

* Use `Jenkinsfile-insecure` → observe vulnerabilities
* Use `Jenkinsfile-secure` → observe secure execution

---

## Key Learnings

* Containers are not secure by default
* Secrets should never be baked into images
* CI/CD pipelines are a major attack surface
* Always scan images before running

---

## Medium Article

Full detailed explanation (with screenshots):
https://medium.com/@nightcode_x/from-insecure-docker-pipelines-to-devsecops-securing-ci-cd-with-jenkins-docker-scout-c179e8e754bf

---

## Support

If you found this useful:

* ⭐ Star the repo
* Share your feedback
