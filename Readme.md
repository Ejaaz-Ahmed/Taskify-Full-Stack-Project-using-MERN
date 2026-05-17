# DevOps Project: Taskify MERN Application

## 📌 Project Overview

This project implements a complete CI/CD pipeline for a full‑stack MERN application (Taskify) on Microsoft Azure. It covers:

- Git Flow branching strategy
- Docker containerization (backend + frontend)
- Azure resources: ACR, Cosmos DB (MongoDB API), Linux VM, Static Web App
- Azure DevOps pipelines with multi‑stage deployments and approval gates
- **Extra Mile:** Terraform (IaC), SonarQube (SAST), Snyk (SCA), OWASP ZAP (DAST)

---

## ✅ Core Lab Completion Checklist

| What to Demonstrate | How We Verified | Screenshot Evidence |
|---------------------|----------------|----------------------|
| **Forked GitHub Repo** | The repository `Taskify-Full-Stack-Project-using-MERN` is forked to my personal GitHub account. The fork badge is visible. | `[screenshot]` |
| **Git Flow Branches** | Azure Repos shows `main`, `develop`, and at least one `feature/*` branch (e.g., `feature/dockerize-app`). | `[screenshot]` |
| **Dockerfiles committed** | `backend/Dockerfile` and `frontend/Dockerfile` (with `nginx.conf` and `.dockerignore`) are present in Azure Repos. | `[screenshot]` |
| **Docker images in ACR** | Azure Container Registry contains `taskapp-backend` with build tags (e.g., `42`, `latest`). Frontend is built as static files (no image). | `[screenshot]` |
| **Azure SQL Database running** | *Adapted for MERN:* Azure Cosmos DB (MongoDB API) is running, and the database schema/tables are populated. | `[screenshot]` |
| **App Service deployed** | Backend is deployed to a Linux VM (due to quota limits). The health endpoint `http://<VM-IP>:8800/api/health` returns HTTP 200. | `[screenshot]` |
| **Static Web App live** | Frontend is served from the same VM using Nginx (or Azure Static Web App initially). The login page loads and calls the backend API. | `[screenshot]` |
| **End‑to‑end function test** | User can register, log in, and an admin user can create a task. The new task appears instantly in the dashboard. | `[screenshot]` |
| **Successful pipeline run** | Azure DevOps Pipelines shows a green (passed) run with all stages (Build, DeployDev, etc.) completed. | `[screenshot]` |
| **Multi‑stage pipeline proof** | Pipeline stages (Build → DeployDev → DeployProd) are visible with green checkmarks. | `[screenshot]` |
| **Approval gate used** | In Azure DevOps → Environments → `production`, the deployment required manual approval before proceeding. | `[screenshot]` |
| **No secrets in code** | No `.env` files are committed; all secrets are stored in Azure DevOps Variable Groups or Azure App Settings. | `[screenshot]` |

---

## 🚀 Extra Mile Completion Verification

| Area | What to Show | How We Verified | Screenshot Evidence |
|------|--------------|-----------------|----------------------|
| **Terraform (IaC)** | `terraform.tfstate` in Azure Blob Storage + `terraform show` output | Created a storage account `sttfstateejaz` with container `tfstate`. Ran `terraform apply` to provision resources, then `terraform show` to display them. | `[screenshot]` |
| **Terraform** | All resources show `Created by: terraform` in Activity Log | In Azure Portal, the resource group and ACR activity log entries indicate creation by Terraform. | `[screenshot]` |
| **SonarQube (SAST)** | SonarQube running on Azure VM – project dashboard with code quality metrics | SonarQube Community Edition runs on a `Standard_B2s` VM (port 9000). The project `taskapp-backend` shows bugs, vulnerabilities, and code smells. | `[screenshot]` |
| **SonarQube** | Custom Quality Gate `TaskApp Enterprise Gate` with A‑rating conditions | In SonarQube → Quality Gates, the custom gate enforces Security Rating, Reliability Rating, etc., all requiring grade A on new code. | `[screenshot]` |
| **SonarQube** | Pipeline fails when quality gate is not passed | A pipeline run was intentionally triggered with code that introduced a vulnerability (e.g., `eval()`). The SAST stage blocked the deployment. | `[screenshot]` |
| **Snyk (SCA)** | Snyk project monitoring enabled – dependency scan results | The Snyk dashboard shows the imported project (from `server/package.json`) with detected vulnerabilities and fix advice. | `[screenshot]` |
| **Snyk** | SCA stage in pipeline with scan results artifact | The pipeline includes a `SCA` stage using the Snyk task. The logs display the number of issues found, and an HTML/JSON report is attached. | `[screenshot]` |
| **OWASP ZAP (DAST)** | ZAP reports published – HTML report with alert list | Due to network restrictions from the Microsoft‑hosted agent, a manual ZAP baseline scan was run locally against `http://20.6.130.6:8800/`. The generated `zap-report.html` shows alerts. | `[screenshot]` |
| **OWASP ZAP** | Analysis of findings (top 3 vulnerabilities + remediation) | See table below for the top 3 vulnerabilities identified by ZAP and their fixes. | `[screenshot + analysis]` |

### ZAP Vulnerability Analysis

| # | Vulnerability | Remediation |
|---|---------------|-------------|
| 1 | Missing `X-Frame-Options` header | Add `app.use(require('helmet')())` in Express to set `X-Frame-Options: DENY`. |
| 2 | `X-Powered-By: Express` header disclosed | Disable with `app.disable('x-powered-by')` in the backend entry file. |
| 3 | Cookie missing `HttpOnly` flag | When setting JWT cookies, add `httpOnly: true` to prevent client‑side JavaScript access. |

---

## 🛠️ Technologies & Tools Used

- **Source Control:** Git, GitHub, Azure Repos (Git Flow)
- **Containerization:** Docker, Docker Compose, Nginx
- **Cloud Provider:** Microsoft Azure (Student Account)
  - Azure Container Registry (ACR)
  - Azure Cosmos DB (MongoDB API)
  - Azure Virtual Machines (Linux)
  - Azure Static Web Apps (initial)
- **CI/CD:** Azure DevOps Pipelines (YAML), multi‑stage, approval gates
- **Infrastructure as Code:** Terraform (remote state in Azure Blob)
- **Security (DevSecOps):**
  - SAST: SonarQube Community Edition
  - SCA: Snyk
  - DAST: OWASP ZAP

---

## 📎 How to Reproduce (Quick Start)

1. **Clone the repository** and set up Git Flow (`main`, `develop`, `feature/*`).
2. **Build Docker images** locally using the provided `Dockerfile`s.
3. **Provision Azure resources** (or use Terraform scripts in `/terraform`).
4. **Import the repository** into Azure Repos and set up service connections.
5. **Create variable groups** (`taskapp-variables`) with required secrets.
6. **Run the pipelines** (`backend-pipeline.yml`, `frontend-pipeline.yml`).
7. **For Extra Mile:** Deploy SonarQube VM, configure quality gate, add Snyk and ZAP stages.

---

## 📝 Notes

- Due to student subscription quota limits, the backend is hosted on a **Linux VM** instead of App Service. The frontend is served from the same VM using Nginx as a reverse proxy.
- The ZAP DAST stage is documented manually because the Microsoft‑hosted agent could not reach the VM’s port 8800. The manual scan fulfills the learning objective.
- All sensitive data (connection strings, tokens) are stored in Azure DevOps **Variable Groups** – no secrets are hardcoded.

---

**Project by:** Ejaz Ahmed (NUM-BSCS-2023-02)  
**Instructor:** Sir Shahzad Arif  
**Date:** May 20, 2026
