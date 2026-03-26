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

This workflow demonstrates a basic CI setup where:

* A workflow is triggered on every push
* A job runs on a virtual machine
* A simple command is executed and verified via logs

It serves as the **first step toward building a complete CI/CD pipeline**.
