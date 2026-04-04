---
title: "Appendix: AI for DevOps Engineers"
status: first-draft
include_in_compile: true
synopsis: "AI for IaC, pipelines, monitoring, and incident response on Azure."
---

# AI for DevOps Engineers

> Your quick-start guide to using AI in your daily work.

---

## AI in Your Day

A typical DevOps engineer spends their day writing Infrastructure as Code, configuring CI/CD pipelines, monitoring production systems, responding to incidents, and hardening security across environments. AI can accelerate nearly every one of these tasks -- not by replacing your judgment, but by generating solid first drafts you refine into production-ready artifacts.

| Daily Task | How AI Helps | Time Saved |
|------------|-------------|------------|
| Writing IaC templates (Bicep/ARM) | Generates resource definitions from plain-language descriptions, including dependencies, naming conventions, and tagging | 30-60 min per template |
| Configuring CI/CD pipelines | Drafts Azure Pipelines YAML from a description of your build, test, and deploy stages, with environment-specific variables and service connections | 30-45 min per pipeline |
| Analyzing logs and alerts | Generates KQL queries for Application Insights or Azure Monitor, explains error patterns, and suggests follow-up queries to isolate root causes | 20-30 min per investigation |
| Writing incident postmortems | Structures a timeline, root cause analysis, and action items from raw incident notes and chat logs into a blameless postmortem format | 45-60 min per postmortem |
| Optimizing Dockerfiles | Reviews container configurations and rewrites them with multi-stage builds, layer caching, security hardening, and smaller base images | 20-40 min per Dockerfile |
| Security and compliance reviews | Scans IaC templates and pipeline configs for hardcoded secrets, overly permissive RBAC, missing encryption, and insecure defaults | 30-45 min per review |

---

## 30-Minute Quick Start

> Get productive with AI in 30 minutes. Follow these three steps in order, and you will have used AI for infrastructure, pipelines, and monitoring before your next standup.

### Step 1: Generate a Bicep Template from a Description (10 min)

Open your preferred AI tool (ChatGPT, Claude, or Copilot Chat in VS Code) and type a prompt like this:

```
Generate a Bicep template for the following Azure infrastructure:
- An App Service Plan on the S1 tier
- A Web App with system-assigned managed identity and HTTPS-only enabled
- An Azure SQL Database on the S0 tier in the same resource group
- Parameters for environment name and SQL admin password (marked @secure())
- Use our naming convention: {resource-type}-{app-name}-{environment}
- Tag all resources with CostCenter: Engineering and Owner: Platform Team
```

Review the output. Verify the API versions are current, the naming convention matches your standards, and the security settings (TLS 1.2, FTPS disabled) are present. Validate with `az bicep build -f main.bicep` and deploy to a dev resource group with `az deployment group create`.

### Step 2: Build an Azure Pipelines YAML (10 min)

Now describe your build and deploy process:

```
Generate an Azure DevOps pipeline YAML that:
1. Triggers on pushes to the main branch
2. Builds a .NET 8 web API and runs unit tests
3. Builds a Docker image and pushes it to Azure Container Registry
4. Deploys the container to Azure App Service in a staging environment
Use ubuntu-latest agents. Reference service connection names as placeholders I can replace.
```

Review the generated YAML carefully. Check that task versions are current (e.g., `UseDotNet@2`, `Docker@2`), that secret values use variable references instead of hardcoded strings, and that the deployment stage uses a proper environment with `deployment` jobs. Commit to Azure Repos and run the pipeline against a non-production environment.

### Step 3: Write a KQL Query to Diagnose an Issue (10 min)

Describe a real or hypothetical production issue:

```
Our API response times spiked above 2 seconds between 2:00 PM and 2:30 PM UTC today.
We use Application Insights (workspace-based). Write KQL queries to:
1. Find the slowest endpoints during that window with average and p95 duration
2. Correlate with dependency calls (SQL, HTTP, Redis) to find the bottleneck
3. Check if any exceptions spiked during the same window
```

Copy the queries into Application Insights in the Azure portal. Adjust table names if your workspace uses custom schemas. Verify the column names (`duration`, `name`, `success`, `timestamp`) match your telemetry before building dashboards or alerts from these queries.

---

## Concrete Examples

### Example 1: Generating Azure Pipelines YAML for a .NET Application

**Scenario.** You are setting up a new CI/CD pipeline for a .NET 8 microservice. The service needs to build, test, containerize, and deploy to AKS with a staging gate before production.

**Prompt.**
```
You are a senior DevOps engineer working with Azure DevOps and Azure Pipelines.
Generate a multi-stage Azure Pipelines YAML file for a .NET 8 Web API with these stages:

1. Build: Restore NuGet packages, build in Release mode, run unit tests with code coverage
2. Publish: Build a Docker image using the repo's Dockerfile and push to Azure Container Registry
3. Deploy to Staging: Deploy the image to an AKS cluster in the staging namespace using Helm
4. Deploy to Production: Same as staging but requires manual approval and targets the production namespace

Requirements:
- Trigger on main branch only
- Use ubuntu-latest agent pool
- Store ACR credentials in a variable group named "acr-credentials"
- Use Helm chart located in ./deploy/helm/
- Include proper displayName for every step
- Reference service connections as placeholders (e.g., "YOUR-ACR-CONNECTION")
```

**What to expect.** The AI will generate a complete multi-stage YAML file with proper `dependsOn` chains, a `deployment` job for the staging and production stages, Helm install steps, and manual approval via Azure DevOps environments. Review the task versions, replace placeholder service connection names with your actual connections, and validate that variable group references exist in your Azure DevOps project.

### Example 2: Writing Bicep Templates for Azure Infrastructure

**Scenario.** Your team is deploying a new microservice that needs an App Service, a Key Vault for secrets, and Application Insights for monitoring, all wired together with managed identity.

**Prompt.**
```
Generate a Bicep template that deploys the following Azure resources for a
microservice called "orders-api":

Resources:
- App Service Plan (S1 tier, Linux)
- App Service (running a Docker container, HTTPS only, TLS 1.2 minimum, FTPS disabled)
- Key Vault (RBAC authorization, no access policies)
- Application Insights (workspace-based, connected to an existing Log Analytics workspace)

Connections:
- App Service has a system-assigned managed identity
- The managed identity has Key Vault Secrets User role on the Key Vault
- Application Insights connection string is set as an App Service app setting
- App Service pulls its container image from an existing Azure Container Registry

Parameters:
- environment (dev, staging, prod)
- location (default: resource group location)
- containerRegistryName (existing ACR name)
- containerImageTag
- logAnalyticsWorkspaceId (existing workspace resource ID)

Naming convention: {resource-type}-orders-api-{environment}
Tag all resources: CostCenter=Engineering, Team=Platform, ManagedBy=Bicep
```

**What to expect.** A complete Bicep file with parameterized resource definitions, proper resource dependencies (the role assignment depends on both the managed identity and the Key Vault), and secure defaults. Verify that the API versions are recent, the role definition GUID for "Key Vault Secrets User" is correct (`4633458b-17de-408a-b874-0445c86b69e6`), and all outputs your pipeline needs (App Service hostname, Key Vault URI) are included.

### Example 3: KQL Queries for Application Insights and Azure Monitor

**Scenario.** Your production service is experiencing intermittent 500 errors. You need to quickly identify which endpoints are failing, what dependencies are causing the failures, and whether the error rate correlates with a recent deployment.

**Prompt.**
```
You are a senior SRE working with Azure Application Insights (workspace-based).
Write KQL queries for the following investigation:

Context: Our "orders-api" service started returning intermittent HTTP 500 errors
approximately 30 minutes ago. We deployed version 2.4.1 about 45 minutes ago.

Queries needed:
1. Show the error rate (percentage of failed requests) over time in 1-minute buckets
   for the last 2 hours, so I can see when the errors started
2. Break down failures by endpoint (operation name) with count, error rate,
   and average duration
3. Show the most common exception types and messages with their frequency
4. Correlate failed requests with dependency calls (SQL, HTTP, Redis) to find
   which dependency is causing failures
5. Compare the error rate for the 1 hour before the deployment vs. 1 hour after

Format each query with a comment explaining what it does.
```

**What to expect.** Five separate KQL queries, each targeting the appropriate Application Insights table (`requests`, `exceptions`, `dependencies`). The time-series query will use `summarize` with `bin(timestamp, 1m)` for the timeline view. The deployment comparison query will use `let` statements to define the before/after time windows. Test each query in the Azure portal's Logs blade and adjust the time ranges and field names to match your telemetry.

### Example 4: Dockerfile Optimization and Multi-Stage Build Creation

**Scenario.** You have a Dockerfile for a .NET 8 API that produces a 1.2 GB image because it includes the full SDK, NuGet cache, and build artifacts. You need to shrink it and speed up builds.

**Prompt.**
```
Optimize the following Dockerfile for a .NET 8 Web API. The current image is
1.2 GB and takes 8 minutes to build.

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0
WORKDIR /app
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish
EXPOSE 8080
ENTRYPOINT ["dotnet", "/app/publish/MyApi.dll"]
```

Apply these optimizations:
1. Multi-stage build: separate build stage from runtime stage
2. Use the ASP.NET runtime image (not SDK) for the final stage
3. Use Alpine-based images where possible for smaller size
4. Optimize layer caching by copying .csproj files first, then restoring, then copying source
5. Run as a non-root user in the final stage
6. Add HEALTHCHECK instruction
7. Add labels for image metadata (maintainer, version, description)
8. Generate a matching .dockerignore file

Explain each optimization and estimate the final image size.
```

**What to expect.** A multi-stage Dockerfile with a `build` stage using `sdk:8.0-alpine` and a `runtime` stage using `aspnet:8.0-alpine`. The build stage copies only `*.csproj` and `*.sln` files first for restore caching, then copies the full source for publish. The runtime stage copies only the published output, creates a non-root user, sets the `USER` directive, and includes a `HEALTHCHECK` using `curl` or the built-in ASP.NET health endpoint. The estimated final image size should be around 100-150 MB. Also expect a `.dockerignore` file that excludes `bin/`, `obj/`, `.git/`, `*.md`, and other non-essential files.

---

## Config Examples

The following are practical, AI-generated configuration examples. Each one was generated from a prompt, reviewed by a human, and refined for production use. Use them as starting points -- never deploy config examples without adapting them to your environment.

### Config Example 1: Multi-Stage Azure Pipelines YAML with Approval Gate

```yaml
# AI-generated pipeline -- review service connections and variable groups before use
# Prompt: "Build .NET 8 API, run tests, push Docker image to ACR, deploy to AKS staging
#          and production with manual approval"

trigger:
  branches:
    include:
      - main

variables:
  - group: 'acr-credentials'
  - name: dockerRegistry
    value: 'myregistry.azurecr.io'
  - name: imageName
    value: 'orders-api'
  - name: tag
    value: '$(Build.BuildId)'
  - name: helmChart
    value: './deploy/helm/orders-api'

stages:
  # Stage 1: Build and Test
  - stage: Build
    displayName: 'Build and Test'
    jobs:
      - job: BuildAndTest
        displayName: 'Build, Test, Containerize'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - task: UseDotNet@2
            displayName: 'Install .NET 8 SDK'
            inputs:
              packageType: 'sdk'
              version: '8.x'

          - script: dotnet restore
            displayName: 'Restore NuGet packages'

          - script: dotnet build --configuration Release --no-restore
            displayName: 'Build solution'

          - script: dotnet test --configuration Release --no-build --logger trx --collect:"XPlat Code Coverage"
            displayName: 'Run unit tests with coverage'

          - task: PublishTestResults@2
            displayName: 'Publish test results'
            inputs:
              testResultsFormat: 'VSTest'
              testResultsFiles: '**/*.trx'

          - task: Docker@2
            displayName: 'Build and push Docker image'
            inputs:
              containerRegistry: 'YOUR-ACR-SERVICE-CONNECTION'
              repository: '$(imageName)'
              command: 'buildAndPush'
              Dockerfile: '**/Dockerfile'
              tags: |
                $(tag)
                latest

  # Stage 2: Deploy to Staging
  - stage: DeployStaging
    displayName: 'Deploy to Staging'
    dependsOn: Build
    jobs:
      - deployment: DeployToStaging
        displayName: 'Helm deploy to AKS staging'
        pool:
          vmImage: 'ubuntu-latest'
        environment: 'staging'
        strategy:
          runOnce:
            deploy:
              steps:
                - task: HelmDeploy@1
                  displayName: 'Helm upgrade orders-api (staging)'
                  inputs:
                    connectionType: 'Azure Resource Manager'
                    azureSubscription: 'YOUR-AZURE-SUBSCRIPTION'
                    azureResourceGroup: 'rg-aks-staging'
                    kubernetesCluster: 'aks-staging'
                    namespace: 'staging'
                    command: 'upgrade'
                    chartType: 'FilePath'
                    chartPath: '$(helmChart)'
                    releaseName: 'orders-api'
                    overrideValues: 'image.repository=$(dockerRegistry)/$(imageName),image.tag=$(tag)'

  # Stage 3: Deploy to Production (requires manual approval on the environment)
  - stage: DeployProduction
    displayName: 'Deploy to Production'
    dependsOn: DeployStaging
    jobs:
      - deployment: DeployToProduction
        displayName: 'Helm deploy to AKS production'
        pool:
          vmImage: 'ubuntu-latest'
        environment: 'production'   # Configure approval checks on this environment
        strategy:
          runOnce:
            deploy:
              steps:
                - task: HelmDeploy@1
                  displayName: 'Helm upgrade orders-api (production)'
                  inputs:
                    connectionType: 'Azure Resource Manager'
                    azureSubscription: 'YOUR-AZURE-SUBSCRIPTION'
                    azureResourceGroup: 'rg-aks-production'
                    kubernetesCluster: 'aks-production'
                    namespace: 'production'
                    command: 'upgrade'
                    chartType: 'FilePath'
                    chartPath: '$(helmChart)'
                    releaseName: 'orders-api'
                    overrideValues: 'image.repository=$(dockerRegistry)/$(imageName),image.tag=$(tag)'
```

**What to review before using this.** Replace all `YOUR-*` placeholders with actual service connection names from your Azure DevOps project. Verify the variable group `acr-credentials` exists. Configure approval checks on the `production` environment in Azure DevOps under Pipelines > Environments. Confirm the Helm chart path and override values match your chart's `values.yaml` structure.

### Config Example 2: Optimized Multi-Stage Dockerfile for .NET 8

```dockerfile
# AI-generated Dockerfile -- verify base image tags and health endpoint before use
# Prompt: "Optimized multi-stage Dockerfile for .NET 8 API with Alpine, non-root user"

# ---------- Build Stage ----------
FROM mcr.microsoft.com/dotnet/sdk:8.0-alpine AS build
WORKDIR /src

# Copy project files first for layer caching
COPY ["src/MyApi/MyApi.csproj", "src/MyApi/"]
COPY ["src/MyApi.Core/MyApi.Core.csproj", "src/MyApi.Core/"]
COPY ["MyApi.sln", "."]
RUN dotnet restore

# Copy everything else and publish
COPY . .
RUN dotnet publish src/MyApi/MyApi.csproj \
    --configuration Release \
    --no-restore \
    --output /app/publish \
    /p:UseAppHost=false

# ---------- Runtime Stage ----------
FROM mcr.microsoft.com/dotnet/aspnet:8.0-alpine AS runtime

LABEL maintainer="platform-team@company.com" \
      description="Orders API service" \
      version="1.0"

# Security: run as non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app
COPY --from=build /app/publish .

# Set non-root user
USER appuser

EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
ENV DOTNET_RUNNING_IN_CONTAINER=true

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/health || exit 1

ENTRYPOINT ["dotnet", "MyApi.dll"]
```

**What to review before using this.** Confirm your project structure matches the `COPY` paths (adjust `.csproj` locations as needed). Verify the `/health` endpoint exists in your application or change the `HEALTHCHECK` path. If your application requires native dependencies not included in Alpine (e.g., certain image processing libraries), switch to the Debian-based `aspnet:8.0` image instead. Test locally with `docker build -t myapi:test . && docker run -p 8080:8080 myapi:test` before pushing to ACR.

### Config Example 3: KQL Alert Rule for Error Rate Monitoring

```kql
// AI-generated KQL -- adjust thresholds and time windows for your SLAs
// Prompt: "Alert query for Application Insights: fire when error rate exceeds 5%
//          for any endpoint over a 5-minute window"

// Error rate by endpoint over a sliding 5-minute window
let threshold = 5.0;  // percent
let timeWindow = 5m;
requests
| where timestamp > ago(timeWindow)
| summarize
    totalRequests = count(),
    failedRequests = countif(resultCode startswith "5"),
    errorRate = round(countif(resultCode startswith "5") * 100.0 / count(), 2)
    by bin(timestamp, 1m), operation_Name
| where errorRate > threshold
| where totalRequests > 10   // Ignore low-traffic endpoints to reduce noise
| project
    timestamp,
    endpoint = operation_Name,
    errorRate,
    failedRequests,
    totalRequests
| order by errorRate desc
```

**What to review before using this.** Adjust the `threshold` value based on your SLAs (5% may be too aggressive or too lenient for your services). The `totalRequests > 10` filter prevents alerts on low-traffic endpoints where a single failure creates a 100% error rate -- tune this to your traffic volume. When creating an alert rule from this query in Azure Monitor, set the evaluation frequency and lookback period to match your incident response expectations.

---

## Prompt Templates

Copy these templates, fill in the bracketed sections, and paste into your AI tool.

### Template 1: Azure Pipeline Generator

```
You are a senior DevOps engineer working with Azure DevOps and Azure Pipelines.
Generate a complete azure-pipelines.yml file for the following workflow.

APPLICATION:
- Language/Framework: [e.g., .NET 8, Node 20, Python 3.12, Java 17]
- Repository: Azure Repos Git
- Build output: [e.g., Docker image, NuGet package, WAR file]

STAGES:
1. [BUILD STAGE -- e.g., "Restore dependencies, compile, run unit tests with coverage"]
2. [PUBLISH STAGE -- e.g., "Build Docker image and push to Azure Container Registry"]
3. [DEPLOY STAGE -- e.g., "Deploy container to AKS staging namespace via Helm"]
4. [ADDITIONAL STAGES -- e.g., "Production deploy with manual approval gate"]

REQUIREMENTS:
- Trigger: [e.g., "Push to main branch, PR builds for feature/* branches"]
- Agent pool: [e.g., "ubuntu-latest" or "self-hosted pool name"]
- Variable groups: [LIST VARIABLE GROUPS AND WHAT THEY CONTAIN]
- Service connections: [LIST SERVICE CONNECTION NAMES AND TARGETS]
- Approvals: [e.g., "Manual approval before production deploy via environment checks"]
- Secrets handling: [e.g., "Use Azure Key Vault task to fetch secrets at runtime"]

Include proper task versions, displayName for every step, and comments explaining
non-obvious configuration choices. Use placeholder names like YOUR-SERVICE-CONNECTION
where I need to substitute my own values.
```

### Template 2: Bicep Template Generator

```
Generate a Bicep template for the following Azure infrastructure.

RESOURCES NEEDED:
[DESCRIBE EACH RESOURCE -- e.g., "An Azure App Service on the S1 plan with
system-assigned managed identity enabled"]

REQUIREMENTS:
- Environment parameter: [e.g., dev, staging, prod]
- Naming convention: [e.g., "{resource-type}-{app-name}-{environment}"]
- Region: [e.g., East US 2, or "default to resource group location"]
- Tags: [e.g., "CostCenter: Engineering, Owner: Platform Team, ManagedBy: Bicep"]
- Security: [e.g., "HTTPS only, TLS 1.2 minimum, FTPS disabled, managed identity for auth"]

NETWORKING:
[DESCRIBE VNET/SUBNET REQUIREMENTS IF ANY -- e.g., "Private endpoints for SQL and
Key Vault on an existing subnet named snet-private in vnet-main"]

CONNECTIONS BETWEEN RESOURCES:
[DESCRIBE HOW RESOURCES CONNECT -- e.g., "App Service managed identity has
Key Vault Secrets User role. Application Insights connection string injected
as an app setting."]

Include:
- Parameter definitions with @description decorators and sensible defaults
- @secure() decorator on sensitive parameters
- Resource dependencies in the correct order
- Output values for connection strings, endpoints, or resource IDs needed downstream
- Comments explaining non-obvious configuration choices
```

### Template 3: KQL Query Builder

```
You are a senior SRE working with Azure Application Insights (workspace-based
Log Analytics). Write KQL queries for the following investigation.

CONTEXT:
[DESCRIBE THE ISSUE -- e.g., "Our orders-api service is returning intermittent
HTTP 500 errors. The errors started approximately 30 minutes ago. We deployed
version 2.4.1 about 45 minutes ago."]

APPLICATION INSIGHTS DETAILS:
- Resource name: [e.g., "appi-orders-api-prod"]
- Key tables available: [e.g., requests, dependencies, exceptions, traces, customMetrics]
- Custom dimensions in use: [e.g., "DeploymentVersion, TenantId, CorrelationId"]

QUERIES NEEDED:
1. [FIRST QUERY -- e.g., "Error rate over time in 1-minute buckets for the last 2 hours"]
2. [SECOND QUERY -- e.g., "Top failing endpoints with count and error rate"]
3. [THIRD QUERY -- e.g., "Most common exception types and messages"]
4. [FOURTH QUERY -- e.g., "Dependency failures correlated with request failures"]

Format each query with a comment explaining what it shows. Use let statements
for reusable time ranges and thresholds.
```

### Template 4: Incident Postmortem Generator

```
You are a senior SRE writing a blameless incident postmortem. Use the following
raw incident data to produce a structured postmortem document.

RAW INCIDENT DATA:
<incident_timeline>
[PASTE SLACK/TEAMS MESSAGES, ALERT LOGS, AND NOTES FROM THE INCIDENT --
include timestamps, who did what, what alerts fired, what actions were taken]
</incident_timeline>

INCIDENT DETAILS:
- Service(s) affected: [e.g., "orders-api, payment-service"]
- Duration: [e.g., "47 minutes, from 14:03 UTC to 14:50 UTC"]
- Severity: [e.g., "SEV-2: customer-facing degradation"]
- Detection method: [e.g., "Azure Monitor alert on error rate > 5%"]

Produce the following sections:
1. **Summary**: 2-3 sentence overview of what happened and the impact
2. **Impact**: User-facing impact, number of affected users/requests, revenue impact if known
3. **Timeline**: Chronological table with columns: Time (UTC) | Event | Actor
4. **Root Cause**: Technical explanation of what caused the incident
5. **Contributing Factors**: Conditions that allowed the root cause to have impact
6. **What Went Well**: Things that worked during incident response
7. **What Went Poorly**: Things that did not work or slowed resolution
8. **Action Items**: Table with columns: Action | Owner (role) | Priority (P1/P2/P3) | Jira Ticket | Due Date
9. **Lessons Learned**: Key takeaways for the team

Use a blameless tone throughout. Focus on systems and processes, never individuals.
Format for Confluence (use headers, tables, and callout panels).
```

---

## AI Changes The Operational Surface

If your team runs AI-enabled features or agent workflows, normal infrastructure observability is necessary but not sufficient.

You also need visibility into:

- model version and prompt version,
- token usage and cost,
- retrieval behavior,
- tool-call traces,
- safety events,
- and quality signals such as retries, escalations, or user dissatisfaction.

That operational layer is close enough to DevOps work that it should not be treated as someone else’s problem. It is part of modern platform responsibility.

For a deeper treatment, continue to [AI Observability and Operations](../22-Appendix-AI-Observability-And-Operations/Appendix-AI-Observability-And-Operations.md).

---

## Pitfalls and Anti-Patterns

These are the mistakes that cause real production incidents. Learn from others' painful experiences.

### 1. Deploying AI-Generated IaC Without Review and Validation

AI-generated Bicep and ARM templates are frequently syntactically valid but operationally dangerous. Common issues include outdated API versions, overly permissive network rules (e.g., allowing `0.0.0.0/0` inbound), missing encryption-at-rest settings, and SKUs that do not match your cost or performance requirements. **Always** run `az bicep build` or `az deployment group what-if` before deploying. Deploy to a dev or sandbox environment first. Have a second engineer review AI-generated IaC the same way you would review any pull request.

### 2. Trusting AI-Generated Pipeline YAML with Secrets Handling

AI models have no knowledge of your actual Azure DevOps project configuration. They will invent variable group names, fabricate service connection identifiers, and sometimes embed values inline that should be stored as secrets. Worse, they may generate pipelines that pass secrets as command-line arguments (visible in logs) instead of using secure pipeline variables or the Azure Key Vault task. Review every secret reference, validate every service connection name against your project, and confirm that sensitive values never appear in plain text in logs or artifacts.

### 3. Using AI-Generated KQL Without Understanding Your Schema

AI generates KQL based on the standard Application Insights schema, but your environment may differ. Workspace-based Application Insights uses slightly different table structures than classic. Custom dimensions, custom metrics, and custom events all have environment-specific column names. A query that looks correct can return zero results or, worse, misleading results because of a schema mismatch. Always test queries in the Azure portal with a narrow time range first. Verify column names with a simple `| take 10` query before building dashboards or alert rules.

### 4. Skipping "What-If" and Plan Reviews for Infrastructure Changes

Both Bicep and Terraform support dry-run capabilities (`az deployment group what-if` and `terraform plan`). AI-generated templates sometimes include resource deletions or property changes that trigger resource recreation -- which means downtime. Never apply infrastructure changes without reviewing the plan output. Pay special attention to lines marked as "Delete" or "Replace" in the plan. If you are using AI to modify existing infrastructure, always diff the generated template against your current state.

### 5. Copying AI-Generated Docker Configurations Without Testing Locally

AI-generated Dockerfiles may reference base images that do not exist for your target architecture (e.g., Alpine images missing native dependencies your application needs), use `COPY` paths that do not match your repository structure, or set incorrect `ENTRYPOINT` commands. Multi-stage builds are especially prone to subtle errors where the published output ends up in the wrong directory. Build and run the container locally with `docker build` and `docker run` before pushing to ACR. Verify the application starts, the health check passes, and the container runs as a non-root user.

---

*[Back to Curriculum Overview](00-Curriculum-Overview.md)*
