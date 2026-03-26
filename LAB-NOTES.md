# 📄 Task 1 – Hello World Workflow

## 🔹 Purpose of the Workflow

The purpose of this workflow is to:

* Validate that GitHub Actions is properly set up in the repository
* Execute a simple job on every code push
* Provide a basic understanding of how workflows, jobs, and steps function

---

## 🔹 Key Configuration Details

* **Workflow File Location:**
  `.github/workflows/hello.yml`

* **Workflow Name:**
  `Hello World Workflow`

* **Trigger Event:**

  ```yaml
  on:
    push:
      branches:
        - '**'
  ```

  * Runs on every push to any branch

* **Job Configuration:**

  ```yaml
  jobs:
    hello-job:
      runs-on: ubuntu-latest
  ```

  * Uses a GitHub-hosted Ubuntu runner

* **Steps Executed:**

  ```yaml
  steps:
    - name: Print Hello Message
      run: echo "Hello GitHub Actions!"
  ```

  * Executes a simple shell command to print a message in logs

---

## 🔹 How to Verify Success

### ✅ 1. Using GitHub Actions Logs

* Navigate to the **Actions tab** in the repository
* Select **Hello World Workflow**
* Open the latest workflow run
* Click on the job: `hello-job`

✔️ Verify output:

<img width="1920" height="619" alt="T1-1 1" src="https://github.com/user-attachments/assets/aeb45e2c-312b-466a-8c2f-dea82bb2c137" />



<img width="1823" height="760" alt="T1-1 2" src="https://github.com/user-attachments/assets/d73e047a-1457-4e75-a60d-944573f74ae9" />

---

## 🔹 Summary

This workflow demonstrates a basic CI setup.

---

# 📄 Task 2 – Docker Build & Push Workflow

## 🔹 Purpose of the Workflow

This workflow automates the process of:

* Building a Docker image from the application source code
* Tagging the image with both `latest` and a unique commit SHA
* Pushing the image to Docker Hub

It ensures that every code change is containerized and ready for deployment, forming a key part of a CI/CD pipeline.

---

## 🔹 Key Configuration Details

### 📁 Files Added

* `.dockerignore` → Excludes unnecessary files from the build context
* `Dockerfile` → Defines a multi-stage Docker build process
* `.github/workflows/docker.yml` → GitHub Actions workflow file

---

### ⚙️ Workflow Configuration

* **Workflow Name:**
  `Docker Build & Push`

* **Trigger Event:**

  ```yaml
  on:
    push:
      branches:
        - main
  ```

  * Executes on every push to the `main` branch

---

### 🏗️ Job Configuration

```yaml
jobs:
  docker:
    runs-on: ubuntu-latest
```

* Uses a GitHub-hosted Ubuntu runner

---

### 🔄 Steps Overview

1. **Checkout Code**

   ```yaml
   uses: actions/checkout@v4
   ```

2. **Login to Docker Hub**

   ```yaml
   uses: docker/login-action@v3
   ```

   * Authenticates using GitHub Secrets

3. **Extract Commit SHA**

   ```yaml
   git rev-parse --short HEAD
   ```

   * Generates a unique tag for versioning

4. **Build Docker Image**

   ```bash
   docker build -t <username>/my-app:latest \
                -t <username>/my-app:<commit-sha> .
   ```

5. **Push Docker Image**

   ```bash
   docker push <username>/my-app:latest
   docker push <username>/my-app:<commit-sha>
   ```

---

### 🐳 Docker Configuration

* **Tags Used:**

  * `latest` → Always points to the newest version
  * `<commit-sha>` → Ensures traceability of builds

---

## 🔹 Secrets Used and Why

### 🔐 Required Secrets

* `DOCKER_USERNAME`
* `DOCKER_PASSWORD`

### 📌 Purpose

* Authenticate securely with Docker Hub
* Prevent exposing credentials in code
* Enable pushing images to the Docker registry

---

## 🔹 How to Verify Success

### ✅ 1. GitHub Actions Logs

* Go to the **Actions tab**
* Open **Docker Build & Push workflow**

<img width="1887" height="650" alt="T2-2 1" src="https://github.com/user-attachments/assets/8a110b60-83bb-44fc-9706-c308c709d13a" />



<img width="1840" height="866" alt="T2-2 2" src="https://github.com/user-attachments/assets/532e63fd-ab48-4f03-935e-b186f9e3d550" />



<img width="1834" height="848" alt="T2-2 3" src="https://github.com/user-attachments/assets/63fd13a6-c56f-4c81-9233-7c2692537ea6" />

---

### ✅ 2. Docker Hub Verification

Check your repository on Docker Hub

<img width="1874" height="773" alt="T2-2 4" src="https://github.com/user-attachments/assets/900c662d-7d52-4bd2-bd8f-ed0be2a3195f" />

---

## 🔹 Summary

This workflow:

* Automates Docker image creation and publishing
* Uses secure authentication via GitHub Secrets
* Implements versioning using commit SHA
* Enables consistent and repeatable container builds
---

# 📄 Task 3 – Shared Workflow Repository & Release 

## 🔹 Purpose of the Workflow

This task introduces a **reusable GitHub Actions workflow** to centralize Docker CI/CD logic across multiple repositories.

The workflow:

* Promotes **code reuse** and avoids duplication of CI/CD configurations
* Enables **standardized Docker build and push processes**
* Supports **environment-based tagging** (staging and production)
* Uses versioned releases for **controlled and stable workflow usage**

---

## 🔹 Key Configuration Details

### 📁 Workflow Repository Setup

* **Workflow File:**
  `.github/workflows/docker-ci.yml`

* **Trigger Type:**

  ```yaml
  on:
    workflow_call:
  ```

  * Enables the workflow to be reused by other repositories

---

### 🔧 Inputs & Secrets

```yaml
inputs:
  image_name:
    required: true
    type: string
  tag:
    required: true
    type: string

secrets:
  DOCKER_USERNAME:
    required: true
  DOCKER_PASSWORD:
    required: true
```

* Accepts dynamic values for image name and tag
* Uses secrets securely for authentication

---

### 🏷️ Versioning via Release

* A release is created in the Workflow Repository:

  ```
  v1.0.0
  ```
* Ensures:

  * Stability of workflow usage
  * Version control for CI/CD logic
  * Safe upgrades in future versions

---

### 🔗 App Repository Integration

* **Caller Workflow File:**
  `.github/workflows/call-docker.yml`

* **Trigger:**

  ```yaml
  on:
    push:
      branches:
        - develop
        - main
  ```

---

### 🔄 Reusable Workflow Call

```yaml
uses: <username>/<workflow-repo>/.github/workflows/docker-ci.yml@v1.0.0
```

---

## 🔹 Secrets Used and Why

### 🔐 Required Secrets

* `DOCKER_USERNAME`
* `DOCKER_PASSWORD`

### 📌 Purpose

* Authenticate with Docker Hub
* Allow secure image push operations
* Prevent exposure of credentials in workflow code

---

## 🔹 How to Verify Success

### ✅ 1. GitHub Actions Logs
l

---

### ✅ 2. Docker Hub Verification

Check your repository on Docker Hub

---

## 🔹 Summary

This task implements:

* A **centralized reusable CI/CD workflow**
* Version-controlled workflow usage via release tags
* Environment-based Docker image tagging
* Secure handling of credentials

It significantly improves scalability and maintainability of CI/CD pipelines across multiple repositories.


