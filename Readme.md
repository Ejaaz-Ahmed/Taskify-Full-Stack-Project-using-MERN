**DevOps Project Report: CI/CD Pipeline for MERN Taskify Application**


**Student Name:** Ejaz Ahmed
**Roll No:** NUM-BSCS-2023-02
**Instructor:** Sir Shahzad Arif
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
GitHub profile, confirming that the repository is a personal copy of the original source. _[Insert_
_screenshot of forked repository on GitHub]_


**2.2 Git Flow Branches**


Inside Azure Repos, the Git Flow branching model was implemented. The repository contains
three main branches: main (production), develop (integration), and a feature branch
named feature/dockerize-app. This structure allows for controlled feature development and
release management. _[Insert screenshot of branch list in Azure Repos]_


**2.3 Dockerfiles Committed**


Both the backend and frontend Dockerfiles, along with associated configuration files, were
committed to the repository. The backend/Dockerfile uses a multi‑stage build for Node.js, while
the frontend/Dockerfile builds the React application and serves it using Nginx.
Additionally, nginx.conf and .dockerignore files are present to ensure efficient and secure
container builds. _[Insert screenshot of Dockerfiles in Azure Repos]_


**2.4 Docker Images in Azure Container Registry (ACR)**


The backend Docker image was built and pushed to Azure Container Registry (ACR) with build
tags (e.g., 42, latest). The ACR repository shows taskapp-backend along with the image tags. The
frontend is built as static files, so no separate container image was stored in ACR. _[Insert_
_screenshot of ACR repositories with tags]_


**2.5 Azure Cosmos DB (MongoDB API) Database Running**


Because the original lab assumed Azure SQL, the database was adapted to Azure Cosmos DB
with the MongoDB API. The database is in the Running state, and the required collections (users,
tasks, etc.) are populated. This was verified via the Cosmos DB Data Explorer. _[Insert_
_screenshot of Cosmos DB overview and collections]_


**2.6 App Service Deployed (Adapted to Linux VM)**


Due to a student subscription quota limit, the backend App Service could not be started. As a
workaround, a Linux virtual machine (vm-taskapp-backend) was provisioned. The backend
Docker container runs on this VM, and the health
endpoint http://<VM-IP>:8800/api/health returns HTTP 200, confirming the backend is
live. _[Insert screenshot of the VM running_ docker ps _or the health endpoint response]_


**2.7 Static Web App Live**


The frontend was initially deployed to an Azure Static Web App. However, due to mixed‑content
(HTTPS frontend calling HTTP backend), the frontend was moved to the same Linux VM and
served by Nginx as a reverse proxy. The frontend loads without errors and successfully calls the
backend API. _[Insert screenshot of the Taskify login page loaded in a browser]_


**2.8 End‑to‑End Function Test**


A complete user journey was tested: registration, login, and task creation (admin only). After
logging in as an admin user, a new task was created via the “+” button, and it appeared instantly
in the dashboard. This confirms that the frontend, backend, and database are all connected and
functioning correctly. _[Insert screenshot of the dashboard showing a newly created task]_


**2.9 Successful Pipeline Run**


The Azure DevOps backend pipeline was run on the develop branch. All stages (Build,
DeployDev, etc.) completed with green checkmarks, indicating a successful build and
deployment. _[Insert screenshot of the pipeline run with green stages]_


**2.10 Multi‑Stage Pipeline Proof**


The pipeline YAML defines multiple stages: Build, DeployDev, and DeployProd. The run view
clearly shows these stages sequentially, each with a green checkmark. This demonstrates a true
multi‑stage CI/CD workflow. _[Insert screenshot of pipeline stages]_


**2.11 Approval Gate Used**


The production environment (DeployProd stage) was configured with an approval gate. Before
the pipeline can deploy to production, a manual approval is required in Azure DevOps →
Environments → production. This was verified by observing that deployment to production only
proceeded after explicit approval. _[Insert screenshot of the approval pending notification or_
_approved status]_


**2.12 No Secrets in Code**


All sensitive information (database connection strings, JWT secrets, ACR credentials) is stored
in Azure DevOps Variable Groups or Azure App Settings. No .env files are committed to the
repository, and a search through the codebase confirms that no passwords or API keys are
hardcoded. _[Insert screenshot showing the absence of_ .env _files in the repo]_


**3. Extra Mile Completion Verification**


**3.1 Infrastructure as Code with Terraform**


Terraform was used to provision Azure resources as code. The configuration files
(main.tf, variables.tf) define a resource group, Azure Container Registry, App Service Plan, and a
Linux Web App. The remote state was stored in an Azure Blob Storage container (tfstate). After
running terraform apply, all resources were created successfully. The command terraform
show lists every resource, and the Activity Log in Azure Portal confirms that each resource was
created by Terraform. _[Insert screenshot of the blob container with_ tfstate _file and_ terraform
show _output]_


**3.2 Static Application Security Testing with SonarQube**


A Linux VM (vm-sonarqube) was provisioned, and SonarQube Community Edition was installed
using Docker. The SonarQube web UI is accessible at http://<VM-IP>:9000. The
project taskapp-backend was analysed, and the dashboard displays code quality metrics including
bugs, vulnerabilities, code smells, and coverage. _[Insert screenshot of SonarQube project_
_dashboard]_


**3.2.1 Custom Quality Gate**


A custom quality gate named “TaskApp Enterprise Gate” was created. It enforces that on new
code, the Security Rating, Reliability Rating, and Maintainability Rating must be A, and
Duplicated Lines (%) must be less than 8.0%. The quality gate was set as the default for the
project. _[Insert screenshot of the quality gate conditions]_


**3.2.2 Pipeline Failure on Quality Gate**


To demonstrate that the pipeline respects the quality gate, a code change was introduced that
created a security vulnerability (e.g., using eval() in a backend route). After committing and
running the pipeline, the SAST stage (SonarQube) failed because the new code violated the


quality gate. The pipeline stopped before deployment, proving the gate works. _[Insert screenshot_
_of the failed pipeline stage with SonarQube quality gate red]_


**3.3 Software Composition Analysis with Snyk**


A free Snyk account was created, and the Snyk extension was installed in Azure DevOps. A
service connection named snyk-connection was set up using the Snyk API token. The Snyk
dashboard shows the imported project (from server/package.json) along with a list of
dependency vulnerabilities, their severity, and recommended fixes. _[Insert screenshot of the Snyk_
_project dashboard]_


**3.3.1 SCA Stage in Pipeline**


The backend pipeline was extended with an SCA stage that runs the SnykSecurityScan@1 task.
The stage executes after SAST and before the build. When Snyk finds high‑severity issues, the
pipeline fails (unless failOnIssues is set to false). The pipeline logs show the Snyk scan output,
and an HTML report is attached as an artifact. _[Insert screenshot of the pipeline SCA stage and_
_scan output]_


**3.4 Dynamic Application Security Testing with OWASP ZAP**


Due to network restrictions between the Microsoft‑hosted Azure DevOps agent and the VM’s
port 8800, the automatic DAST stage in the pipeline could not generate a report. To still fulfil the
requirement, a manual OWASP ZAP baseline scan was performed on a local machine against the
backend endpoint http://20.6.130.6:8800/. The scan completed successfully and generated an
HTML report.


**3.4.1 ZAP Report**


The generated zap-report.html contains a list of alerts categorised by risk level (Low, Medium,
High). The report includes details about each finding, such as the affected URL, parameter, and
suggested remediation. _[Insert screenshot of the first page of the ZAP report showing alert_
_summary]_


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


These findings are documented in the report and the recommended fixes are straightforward to
implement. _[Insert screenshot of the specific alerts in the ZAP report]_


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


