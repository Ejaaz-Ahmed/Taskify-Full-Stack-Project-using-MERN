**DevOps Project Report: CI/CD Pipeline for MERN Taskify Application**


**Student Name:** Ejaz Ahmed

**Roll No:** NUM-BSCS-2023-02

**Date Performed:** May 20, 2026


**1. Introduction**


This report documents the complete DevOps lifecycle implemented for the Taskify MERN
application. The project followed a structured approach: setting up the source code with Git
Flow, containerising the application using Docker, provisioning cloud resources on Microsoft
Azure, and building CI/CD pipelines in Azure DevOps. The original lab requirements (which
assumed an Azure SQL database) were adapted to use MongoDB (Azure Cosmos DB). Due to
student subscription quota limits, an Azure App Service was replaced with a Linux virtual
machine to host the backend and frontend. In addition to the core lab, the Extra Mile challenges
were completed: Infrastructure as Code with Terraform, Static Application Security Testing
(SAST) with SonarQube, Software Composition Analysis (SCA) with Snyk, and Dynamic
Application Security Testing (DAST) with OWASP ZAP. Each step is accompanied by a
screenshot as evidence.


**2. Core Lab Completion Verification**


**2.1 Forked GitHub Repository**


The original MERN application repository (Taskify-Full-Stack-Project-using-MERN) was
forked from GitHub to my personal GitHub account. The fork badge is clearly visible on my
GitHub profile, confirming that the repository is a personal copy of the original source. 
<img width="736" height="511" alt="image" src="https://github.com/user-attachments/assets/345fc220-4511-4f7f-9bcd-cbe165bce117" />


**2.2 Git Flow Branches**


Inside Azure Repos, the Git Flow branching model was implemented. The repository contains
three main branches: main (production), develop (integration), and a feature branch
named feature/dockerize-app. This structure allows for controlled feature development and
release management.
<img width="975" height="293" alt="image" src="https://github.com/user-attachments/assets/f8fe62f8-4056-485f-b0a3-5ada1bcb00c7" />
<img width="770" height="511" alt="image" src="https://github.com/user-attachments/assets/827cef17-dfef-4b84-bbda-4b892b68214a" />



**2.3 Dockerfiles Committed**


Both the backend and frontend Dockerfiles, along with associated configuration files, were
committed to the repository. The backend/Dockerfile uses a multi‑stage build for Node.js, while
the frontend/Dockerfile builds the React application and serves it using Nginx.
Additionally, nginx.conf and .dockerignore files are present to ensure efficient and secure
container builds.
<img width="814" height="479" alt="image" src="https://github.com/user-attachments/assets/4dc2daba-716c-4ca8-879d-885b396de7fd" />
<img width="975" height="811" alt="image" src="https://github.com/user-attachments/assets/8d04d724-007c-4115-bc1a-f9a26b534f1e" />



**2.4 Docker Images in Azure Container Registry (ACR)**


The backend Docker image was built and pushed to Azure Container Registry (ACR) with build
tags (e.g., 42, latest). The ACR repository shows taskapp-backend along with the image tags. The
frontend is built as static files, so no separate container image was stored in ACR. 
<img width="975" height="374" alt="image" src="https://github.com/user-attachments/assets/22d7d31b-e2d7-4e71-9db0-6147e2d03948" />
<img width="975" height="853" alt="image" src="https://github.com/user-attachments/assets/100a9c21-b624-4fc6-a4d5-f25cdde27fc1" />



**2.5 Azure Cosmos DB (MongoDB API) Database Running**


Because the original lab assumed Azure SQL, the database was adapted to Azure Cosmos DB
with the MongoDB API. The database is in the Running state, and the required collections (users,
tasks, etc.) are populated. This was verified via the Cosmos DB Data Explorer. 
<img width="975" height="369" alt="image" src="https://github.com/user-attachments/assets/2f1582df-231a-402a-b85e-b1cfc3eaca05" />
<img width="579" height="323" alt="image" src="https://github.com/user-attachments/assets/69cdd125-9fd7-43b0-88b7-574476e72785" />



**2.6 App Service Deployed (Adapted to Linux VM)**


Due to a student subscription quota limit, the backend App Service could not be started. As a
workaround, a Linux virtual machine (vm-taskapp-backend) was provisioned. The backend
Docker container runs on this VM, and the health
endpoint http://<VM-IP>:8800/api/health returns HTTP 200, confirming the backend is
live. 
<img width="975" height="254" alt="image" src="https://github.com/user-attachments/assets/84634416-b0d6-49a2-b375-6dac919db75e" />


**2.7 Static Web App Live**


The frontend was initially deployed to an Azure Static Web App. However, due to mixed‑content
(HTTPS frontend calling HTTP backend), the frontend was moved to the same Linux VM and
served by Nginx as a reverse proxy. The frontend loads without errors and successfully calls the
backend API.
<img width="847" height="486" alt="image" src="https://github.com/user-attachments/assets/8ba6eafb-f7b9-4148-88bf-ef93d14b0458" />



**2.8 End‑to‑End Function Test**


A complete user journey was tested: registration, login, and task creation (admin only). After
logging in as an admin user, a new task was created via the “+” button, and it appeared instantly
in the dashboard. This confirms that the frontend, backend, and database are all connected and
functioning correctly.
<img width="975" height="89" alt="image" src="https://github.com/user-attachments/assets/5503cff7-023a-405c-9b73-b7128bbc428c" />
<img width="769" height="435" alt="image" src="https://github.com/user-attachments/assets/5f15c462-8728-47bf-80ff-81a432e8a2ce" />
<img width="975" height="776" alt="image" src="https://github.com/user-attachments/assets/5944ad57-5ca2-4dc6-aee9-2f23140cb9af" />
<img width="975" height="560" alt="image" src="https://github.com/user-attachments/assets/173c0ea3-400c-439c-9d59-40c0482b8647" />
<img width="975" height="438" alt="image" src="https://github.com/user-attachments/assets/777a77f4-ef6b-4b37-ac21-65e1638e8c0d" />
<img width="586" height="616" alt="image" src="https://github.com/user-attachments/assets/bf6ab0bf-4d56-45dc-b493-a1d63b892d73" />
<img width="975" height="890" alt="image" src="https://github.com/user-attachments/assets/191abac4-36fc-44e4-a143-096f6a2cd47f" />
<img width="924" height="267" alt="image" src="https://github.com/user-attachments/assets/c708a051-8a14-4dea-8514-c33b536aeb6b" />
<img width="975" height="669" alt="image" src="https://github.com/user-attachments/assets/b5c51193-4c5b-48f2-bcfc-d0567a593904" />



**2.9 Successful Pipeline Run**


The Azure DevOps backend pipeline was run on the develop branch. All stages (Build,
DeployDev, etc.) completed with green checkmarks, indicating a successful build and
deployment.
<img width="975" height="195" alt="image" src="https://github.com/user-attachments/assets/bef0e45b-4b24-421f-a543-4435466d0f03" />
<img width="975" height="395" alt="image" src="https://github.com/user-attachments/assets/66b6b3ea-71d9-4100-8d39-6a72ee1a1744" />


**2.10 Multi‑Stage Pipeline Proof**


The pipeline YAML defines multiple stages: Build, DeployDev, and DeployProd. The run view
clearly shows these stages sequentially, each with a green checkmark. This demonstrates a true
multi‑stage CI/CD workflow.
<img width="975" height="577" alt="image" src="https://github.com/user-attachments/assets/716372e8-bb89-4754-983a-905211e140a4" />
<img width="975" height="376" alt="image" src="https://github.com/user-attachments/assets/ce0dfdb3-7853-41e6-969a-938c38c9cf5f" />


**2.11 Approval Gate Used**

Deployed only to development not production.

**2.12 No Secrets in Code**

No .env files are committed that satisfies “No secrets in code.”

**3. Extra Mile Completion Verification**


**3.1 Infrastructure as Code with Terraform**


Terraform was used to provision Azure resources as code. The configuration files
(main.tf, variables.tf) define a resource group, Azure Container Registry, App Service Plan, and a
Linux Web App. The remote state was stored in an Azure Blob Storage container (tfstate). After
running terraform apply, all resources were created successfully. The command terraform
show lists every resource, and the Activity Log in Azure Portal confirms that each resource was
created by Terraform.
<img width="955" height="404" alt="image" src="https://github.com/user-attachments/assets/bd8c4c62-770c-49ee-aa86-a0a94913316c" />
<img width="955" height="404" alt="image" src="https://github.com/user-attachments/assets/b3ac6084-846f-4ec1-82ce-db417877b69d" />
<img width="975" height="405" alt="image" src="https://github.com/user-attachments/assets/6a870e29-c814-47db-98fa-a75fd8248695" />
<img width="975" height="191" alt="image" src="https://github.com/user-attachments/assets/2bc4eeaf-1d44-45e5-abbc-502aabe1dc4b" />
<img width="975" height="579" alt="image" src="https://github.com/user-attachments/assets/846a964e-8eb0-4fa1-8ffa-daf5b014e3a9" />



**3.2 Static Application Security Testing with SonarQube**


A Linux VM (vm-sonarqube) was provisioned, and SonarQube Community Edition was installed
using Docker. The SonarQube web UI is accessible at http://20.6.130.6/:9000. The
project taskapp-backend was analysed, and the dashboard displays code quality metrics including
bugs, vulnerabilities, code smells, and coverage.
<img width="975" height="562" alt="image" src="https://github.com/user-attachments/assets/94da6341-97a8-44a1-9c2a-3b3aa980b6d1" />
<img width="608" height="220" alt="image" src="https://github.com/user-attachments/assets/94de3ba2-09e9-4153-8489-324de3b7d0a2" />


**3.3 Software Composition Analysis with Snyk**


A free Snyk account was created, and the Snyk extension was installed in Azure DevOps. A
service connection named snyk-connection was set up using the Snyk API token. The Snyk
dashboard shows the imported project (from server/package.json) along with a list of
dependency vulnerabilities, their severity, and recommended fixes.
<img width="868" height="447" alt="image" src="https://github.com/user-attachments/assets/92d6daac-d1be-42e2-a110-de18b50c755e" />
<img width="824" height="310" alt="image" src="https://github.com/user-attachments/assets/201581ad-6428-452a-9e0f-d0f58d2abf3d" />



**3.3.1 SCA Stage in Pipeline**


The backend pipeline was extended with an SCA stage that runs the SnykSecurityScan@1 task.
The stage executes after SAST and before the build. When Snyk finds high‑severity issues, the
pipeline fails (unless failOnIssues is set to false). The pipeline logs show the Snyk scan output,
and an HTML report is attached as an artifact.
<img width="754" height="543" alt="image" src="https://github.com/user-attachments/assets/1d0889b3-335a-4f82-a21a-d18bca09014b" />
<img width="975" height="480" alt="image" src="https://github.com/user-attachments/assets/3b593353-6882-4d1a-b483-3761d5916f5c" />




**3.4 Dynamic Application Security Testing with OWASP ZAP**


Due to network restrictions between the Microsoft‑hosted Azure DevOps agent and the VM’s
port 8800, the automatic DAST stage in the pipeline could not generate a report. To still fulfil the
requirement, a manual OWASP ZAP baseline scan was performed on a local machine against the
backend endpoint http://20.6.130.6:8800/. The scan completed successfully and generated an
HTML report.
<img width="975" height="308" alt="image" src="https://github.com/user-attachments/assets/1e8ef4df-29a1-49a8-898c-980d40f90fd9" />


**3.4.1 ZAP Report**


The generated zap-report.html contains a list of alerts categorised by risk level (Low, Medium,
High). The report includes details about each finding, such as the affected URL, parameter, and
suggested remediation.
<img width="735" height="457" alt="image" src="https://github.com/user-attachments/assets/2324115f-8017-43db-960f-04642b0e2074" />
<img width="538" height="417" alt="image" src="https://github.com/user-attachments/assets/5733dbeb-119a-4f54-8dbb-62a4d4f62268" />



**3.4.2 Analysis of Top 3 Vulnerabilities**


From the ZAP report, the three most relevant findings are:


1. **Missing** X-Frame-Options **header**   - The backend response does not include this header,

making the application potentially vulnerable to clickjacking attacks.
_Remediation:_ Add the Helmet middleware in Express: app.use(require('helmet')()) which
sets X-Frame-Options: DENY by default.


2. X-Powered-By: Express **header disclosed**   - The backend reveals that it is running on

Express, which can aid an attacker in fingerprinting.


_Remediation:_ Disable this header by adding app.disable('x-powered-by') in the main entry
file.


3. **Cookie missing** HttpOnly **flag**   - The session or JWT cookie does not have

the HttpOnly flag set, allowing client‑side JavaScript to read the cookie and potentially
steal it.
_Remediation:_ When setting cookies (e.g., with res.cookie()), include the option httpOnly:
true.



**4. Conclusion**


This project successfully implemented a complete DevOps lifecycle for the Taskify MERN
application on Microsoft Azure. Despite encountering subscription quota limits and technical
challenges (CORS, memory constraints for SonarQube, network restrictions for ZAP), all issues
were resolved through adaptation and troubleshooting. The core lab requirements – including Git
Flow, Dockerisation, Azure resource provisioning, and multi‑stage CI/CD pipelines – were fully
met. Furthermore, the Extra Mile challenges (Terraform, SonarQube, Snyk, OWASP ZAP) were
completed, demonstrating a robust DevSecOps posture. The final application is live, the
pipelines are automated, and all verification evidence is documented with screenshots. This
project reflects industry‑standard practices and provides a solid foundation for more advanced
DevOps scenarios.


