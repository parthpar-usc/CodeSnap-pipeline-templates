
###  `README.md` - CI/CD Workflows for CodeSnap-Frontend 

# CI/CD Workflows for CodeSnap-Frontend

This repository includes  GitHub Actions workflows  to automate the development and deployment process. The workflows cover:

- Code quality checks (Linting, Unit Testing)
- Building and testing the application
- Deploying to Staging (Integration Environment)
- Deploying to Production automatically

## Workflow Overview

| Workflow Name               | File Name            | Trigger Event             | Purpose 
|-----------------------------|----------------------|---------------------------|---------
| Code Check                  | `code-check.yml`     | PR to `dev`, `main`       | Runs ESLint and Unit Tests before merging 
| Build & Test                | `build-test.yml`     | Push to `dev` or `main`   | Builds the application and runs test cases 
| Integration Deployment      | `integration.yml`    | Push to `dev`             | Deploys the latest `dev` branch build to Staging 
| Production Deployment       | `deployment.yml`     | Push to `main`            | Deploys the latest `main` branch build to Production 

## 1. Code Check Workflow

File: `.github/workflows/code-check.yml`  
Purpose:  
- Runs ESLint for code style checks.
- Runs unit tests before merging PRs.
- Automatically assigns reviewers to PRs.

### Trigger:
- Runs on every Pull Request (PR) targeting `dev`, `main`, or `feature/*`.

### Steps:
1. Lints the code using ESLint.
2. Runs unit tests.
3. Assigns reviewers automatically.

### Run Locally:

npm run lint && npm test


## 2. Build & Test Workflow

File: `.github/workflows/build-test.yml`  
Purpose:
- Ensures the code builds successfully before deployment.
- Runs test cases  after building .

### Trigger:
- Runs  on every push  to `dev` or `main`.

### Steps:
1. Checks out the latest code.
2. Builds the application (`npm run build`).
3. Runs unit tests after the build.
4. If successful, allows deployment.

### Run Locally:
npm install
npm run build
npm test


---

## 3. Integration Deployment (Staging)

 File:  `.github/workflows/integration.yml`  
 Purpose:   
- Deploys the  latest `dev` branch build  to  Staging .
- Ensures only  successful builds  are deployed.

### Trigger:
- Runs  on every push to `dev`  (after passing Build & Test).

### Steps:
1. Waits for the  Build & Test workflow to complete .
2. If successful, triggers  deployment to Staging .
3. Calls Railway's  Staging Deploy Hook .

### Required GitHub Secrets:
| Secret Name       | Description |
|-------------------|-------------|
| `DEV_DEPLOY_HOOK` | Deployment Hook for Staging |

### Run Deployment Manually:

curl -X POST $DEV_DEPLOY_HOOK

---

## 4. Production Deployment Workflow

 File:  `.github/workflows/deployment.yml`  
 Purpose:   
- Deploys the  latest `main` branch build  to  Production .
- Ensures only  successful builds  are deployed.

### Trigger:
- Runs  on every push to `main`  (after passing Build & Test).

### Steps:
1. Waits for the  Build & Test workflow to complete .
2. If successful, triggers  deployment to Production .
3. Calls Railway's  Production Deploy Hook .

### Required GitHub Secrets:
| Secret Name       | Description |
|-------------------|-------------|
| `PROD_DEPLOY_HOOK` | Deployment Hook for Production |

### Run Deployment Manually:
```sh
curl -X POST $PROD_DEPLOY_HOOK
```

---

## Setup & Configuration

### 1. Add GitHub Secrets for Deployment
1.  Go to GitHub Repository → Settings → Secrets → Actions .
2. Add:
   - `DEV_DEPLOY_HOOK` → Staging deployment URL (from Railway).
   - `PROD_DEPLOY_HOOK` → Production deployment URL (from Railway).

### 2. Setup Branch Protection Rules
- Protect `main` & `dev` branches under  GitHub → Settings → Branches .
- Enable  "Require status checks to pass before merging" .
- Add  Code Check, Build & Test as required checks .

---

## CI/CD Flow

| Step | Event | Workflow Triggered |
|------|------------------|----------------------|
| 1️⃣ | Pull Request to `dev` or `main` | Runs  Code Check  |
| 2️⃣ | Merge PR into `dev` | Runs  Build & Test  → Deploys to  Staging  |
| 3️⃣ | Merge `dev` into `main` | Runs  Build & Test  → Deploys to  Production  |

---

## Common Issues & Fixes

### 1. GitHub Actions Not Triggering
 Possible Causes: 
- Workflows might not be pushed to the correct branch.
- Branch protection rules may block execution.

 Fix: 
```sh
git fetch --all
git pull origin main
```

---

### 2. "Secrets Not Found" in Deployment
 Possible Causes: 
- GitHub Secrets are missing or misconfigured.

 Fix: 
- Go to  GitHub → Settings → Secrets  and ensure `DEV_DEPLOY_HOOK` and `PROD_DEPLOY_HOOK` exist.

---

### 3. ESLint or Tests Failing
 Possible Causes: 
- Code quality issues or failing tests.

 Fix: 
```sh
npm run lint
npm test
```

---

## Final Notes
- CI/CD automates testing and deployment, reducing manual work.
- Always  test locally before pushing  to avoid pipeline failures.
- For debugging, check  GitHub Actions → Actions logs .
