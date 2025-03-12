

### **📌 README.md – CI/CD Workflows for CodeSnap**
```md
# 🚀 CodeSnap CI/CD Workflows

This repository includes **GitHub Actions workflows** to automate:
- ✅ **Code quality checks** (Linting, Unit Testing).
- ✅ **Building and testing the application** before deployment.
- ✅ **Deploying to Staging (Integration Environment)**.
- ✅ **Deploying to Production** automatically on updates.

---

## 📌 Workflow Overview

| **Workflow Name**  | **File Name** | **Triggers On** | **Purpose** |
|--------------------|-------------|----------------|-------------|
| **Code Check** | `code-check.yml` | On Pull Requests to `dev`, `main`, or `feature/*` | Runs ESLint and Unit Tests to ensure code quality before merging |
| **Build & Test** | `build-test.yml` | On push to `dev` or `main` | Builds the application and runs test cases |
| **Integration Deployment (Staging)** | `integration.yml` | On push to `dev` | Deploys the latest `dev` branch build to Staging |
| **Production Deployment** | `deployment.yml` | On push to `main` | Deploys the latest `main` branch build to Production |

---

## 📌 How Each Workflow Works

### **✅ 1. Code Check Workflow**
**📍 File:** `.github/workflows/code-check.yml`  
**🔹 Purpose:**  
- Runs **ESLint** for code style checks.
- Runs **unit tests** before merging PRs.
- Automatically assigns reviewers to PRs.

**📌 When It Runs:**
- Every time a **Pull Request (PR) is opened** for `dev`, `main`, or `feature/*` branches.

**💻 What It Does:**
1. Lints the code using ESLint.
2. Runs unit tests.
3. Assigns the reviewer automatically.

**📝 Run Locally:**
```bash
npm run lint && npm test
```

---

### **✅ 2. Build & Test Workflow**
**📍 File:** `.github/workflows/build-test.yml`  
**🔹 Purpose:**  
- Ensures that the code **builds successfully** before deployment.
- Runs test cases **after building**.

**📌 When It Runs:**
- Every push to `dev` or `main`.

**💻 What It Does:**
1. Checks out the latest code.
2. Builds the application (`npm run build`).
3. Runs unit tests after the build.
4. If successful, the workflow completes, allowing deployment.

**📝 Run Locally:**
```bash
npm install
npm run build
npm test
```

---

### **✅ 3. Integration Deployment (Staging)**
**📍 File:** `.github/workflows/integration.yml`  
**🔹 Purpose:**  
- Deploys the **latest `dev` branch build** to **Staging**.
- Ensures only **successful builds** are deployed.

**📌 When It Runs:**
- **Every push to `dev`** (after passing Build & Test).

**💻 What It Does:**
1. Waits for the **Build & Test workflow to complete**.
2. If successful, triggers **deployment to Staging**.
3. Calls Railway's **Staging Deploy Hook**.

**🔗 GitHub Secrets Required:**
- **`DEV_DEPLOY_HOOK`** → Deployment Hook for Staging.

**📝 Run Deployment Manually (Locally):**
```bash
curl -X POST $DEV_DEPLOY_HOOK
```

---

### **✅ 4. Production Deployment Workflow**
**📍 File:** `.github/workflows/deployment.yml`  
**🔹 Purpose:**  
- Deploys the **latest `main` branch build** to **Production**.
- Ensures only **successful builds** are deployed.

**📌 When It Runs:**
- **Every push to `main`** (after passing Build & Test).

**💻 What It Does:**
1. Waits for the **Build & Test workflow to complete**.
2. If successful, triggers **deployment to Production**.
3. Calls Railway's **Production Deploy Hook**.

**🔗 GitHub Secrets Required:**
- **`PROD_DEPLOY_HOOK`** → Deployment Hook for Production.

**📝 Run Deployment Manually (Locally):**
```bash
curl -X POST $PROD_DEPLOY_HOOK
```

---

## **🔧 Setup & Configuration**
To ensure that GitHub Actions run smoothly, follow these steps:

### **1️⃣ Add GitHub Secrets for Deployment**
1. **Go to GitHub Repository → Settings → Secrets → Actions**.
2. Add:
   - **`DEV_DEPLOY_HOOK`** → Staging deployment URL (from Railway).
   - **`PROD_DEPLOY_HOOK`** → Production deployment URL (from Railway).

### **2️⃣ Setup Branch Protection Rules (Recommended)**
- **Protect `main` & `dev` branches** under GitHub → Settings → Branches.
- Enable **"Require status checks to pass before merging"**.
- Add **Code Check, Build & Test as required checks**.

---

## 🚀 **CI/CD Flow**
1️⃣ **Pull Request to `dev` or `main`** → Runs **Code Check**  
2️⃣ **Merge PR into `dev`** → Runs **Build & Test** → Deploys to **Staging**  
3️⃣ **Merge `dev` into `main`** → Runs **Build & Test** → Deploys to **Production**  

---

## ❓ **Common Issues & Fixes**
### **1️⃣ Issue: GitHub Actions not triggering?**
🔹 **Possible Causes:**
- Workflows might not be pushed to the correct branch.
- Branch protection rules may block execution.

🛠 **Fix:**
- Ensure `.github/workflows/*.yml` exists in the correct branch (`dev` or `main`).
- Run:
  ```bash
  git fetch --all
  git pull origin main
  ```

---

### **2️⃣ Issue: "Secrets not found" in Deployment?**
🔹 **Possible Causes:**
- GitHub Secrets are missing or misconfigured.

🛠 **Fix:**
- Go to **GitHub → Settings → Secrets** and ensure `DEV_DEPLOY_HOOK` and `PROD_DEPLOY_HOOK` exist.

---

### **3️⃣ Issue: ESLint or Tests Failing?**
🔹 **Possible Causes:**
- Code quality issues or failing tests.

🛠 **Fix:**
- Run locally before pushing:
  ```bash
  npm run lint
  npm test
  ```

---

## 📜 **Final Notes**
- CI/CD automates testing and deployment, reducing manual work.
- **Always test locally** before pushing to avoid pipeline failures.
- For debugging, check **GitHub Actions → Actions logs**.

---

🎉 **Now your CodeSnap-Frontend repo has a fully automated CI/CD pipeline!** 🚀  
For issues, check the logs in GitHub Actions or run the workflows locally. ✅  
```

---

## **📌 Summary**
✅ **Clearly defines each GitHub Actions workflow template** (Code Check, Build & Test, Integration & Deployment).  
✅ **Explains when each workflow runs and why it is important**.  
✅ **Includes setup instructions for GitHub Secrets & Branch Protection**.  
✅ **Documents Common Errors & Fixes**.  
✅ **Provides local commands to run the workflows manually**.  

🎯 **This README provides a professional, structured CI/CD guide for your team!** 🚀  
Let me know if you'd like any modifications. 😊