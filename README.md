# **Azure Pipelines Basics** 🚀

---

## 📑 Table of Contents

1. [What is Azure Pipelines?](#-what-is-azure-pipelines)
2. [Key Concepts](#-key-concepts)
3. [Basic YAML Snippets](#-basic-yaml-snippets)
4. [Sample Pipelines](#-sample-pipelines)

   * [Hello World](#1-hello-world-pipeline-ubuntu-agent)
   * [Environment Variables](#2-echo-environment-variables)
   * [Python Script](#3-run-python-script)
   * [Node.js App](#4-run-nodejs-app)
   * [.NET Build](#5-simple-net-build)
   * [Bash Commands](#6-run-bash-commands)
   * [PowerShell Commands](#7-run-powershell-commands)
   * [Docker Build & Push](#8-docker-build--push-to-acr)
   * [Deploy Node.js App](#9-nodejs-app-deploy-to-azure-web-app)
   * [Artifacts](#10-copy-files-to-artifact)
   * [Deploy Static Website](#11-deploy-static-website-to-azure-app-service)
5. [Workflow](#-workflow)
6. [Benefits](#-benefits)

---

## 🚀 What is Azure Pipelines?

Azure Pipelines (part of **Azure DevOps**) is a **CI/CD service** that helps you:

✅ Automate builds, tests, and deployments.
✅ Deliver to **Azure, AWS, GCP, Kubernetes, Docker, or on-prem servers**.
✅ Work with **any language, any platform, any cloud**.

* **Languages**: Java, Python, .NET, Node.js, Go, PHP, etc.
* **Targets**: VMs, Kubernetes clusters, Containers, App Services, or Hybrid environments.

---

## ⚙️ Key Concepts

* **Pipeline** → Defines the CI/CD process. Written in `azure-pipelines.yml`.
* **Stages** → Logical divisions (Build, Test, Deploy).
* **Jobs** → A group of steps running on an agent.
* **Steps** → Individual tasks (install, test, deploy).
* **Agents** → Machines that run jobs. (Microsoft-hosted or Self-hosted).

---

## 📝 Basic YAML Snippets

### 1. Hello World (print only)

```yaml
steps:
- script: echo Hello, Azure Pipelines!
```

### 2. Use Ubuntu Agent

```yaml
pool: { vmImage: 'ubuntu-latest' }
steps:
- script: echo Running on Ubuntu
```

### 3. Multi-step

```yaml
steps:
- script: echo Step 1
- script: echo Step 2
- script: echo Step 3
```

### 4. Job Example

```yaml
jobs:
- job: Build
  steps:
  - script: echo Building...
```

### 5. Stage + Job + Step

```yaml
stages:
- stage: Demo
  jobs:
  - job: Run
    steps:
    - script: echo Inside Stage → Job → Step
```

---

## 🛠 Sample Pipelines

### 1. Hello World Pipeline (Ubuntu Agent)

```yaml
trigger:
- main
pool:
  vmImage: ubuntu-latest
steps:
- script: echo "Hello, Azure Pipelines 👋"
  displayName: "Print Hello World"
```

---

### 2. Echo Environment Variables

```yaml
steps:
- script: |
    echo "Build ID: $(Build.BuildId)"
    echo "Branch: $(Build.SourceBranchName)"
  displayName: "Print Pipeline Variables"
```

---

### 3. Run Python Script

```yaml
steps:
- task: UsePythonVersion@0
  inputs:
    versionSpec: '3.x'
- script: python hello.py
  displayName: "Run Python Script"
```

---

### 4. Run Node.js App

```yaml
steps:
- task: NodeTool@0
  inputs:
    versionSpec: '18.x'
- script: |
    npm install
    npm run build
  displayName: "Install & Build Node.js App"
```

---

### 5. Simple .NET Build

```yaml
steps:
- task: UseDotNet@2
  inputs:
    packageType: 'sdk'
    version: '8.x'
- script: dotnet build --configuration Release
  displayName: "Build .NET Project"
```

---

### 6. Run Bash Commands

```yaml
steps:
- script: |
    pwd
    ls -l
    echo "Pipeline running on Ubuntu"
  displayName: "Run Basic Bash Commands"
```

---

### 7. Run PowerShell Commands

```yaml
steps:
- powershell: |
    Get-Date
    Write-Output "Hello from PowerShell"
  displayName: "Run PowerShell Commands"
```

---

### 8. Docker Build & Push to ACR

```yaml
variables:
  imageName: 'myapp'
steps:
- task: Docker@2
  inputs:
    containerRegistry: 'myACRServiceConnection'
    repository: '$(imageName)'
    command: 'buildAndPush'
    Dockerfile: '**/Dockerfile'
    tags: |
      $(Build.BuildId)
```

---

### 9. Node.js App Deploy to Azure Web App

```yaml
steps:
- task: NodeTool@0
  inputs:
    versionSpec: '18.x'
- script: npm install
- script: npm test
- task: AzureWebApp@1
  inputs:
    azureSubscription: 'my-azure-connection'
    appName: 'my-nodejs-app'
    package: '$(System.DefaultWorkingDirectory)/**/*.zip'
```

---

### 10. Copy Files to Artifact

```yaml
steps:
- script: echo "Build output" > output.txt
- task: PublishBuildArtifacts@1
  inputs:
    pathToPublish: 'output.txt'
    artifactName: 'drop'
```

---

### 11. Deploy Static Website to Azure App Service

```yaml
steps:
- task: AzureWebApp@1
  inputs:
    azureSubscription: 'my-azure-connection'
    appName: 'my-static-site'
    package: '$(System.DefaultWorkingDirectory)/**/*.zip'
```

---

## 🔄 Workflow

1. Developer pushes code → GitHub / Azure Repos.
2. Pipeline triggers automatically.
3. Builds the code, runs tests.
4. Deploys to chosen environment (VM, App Service, AKS, etc.).

---

## ✅ Benefits

* Automates repetitive tasks.
* Improves release speed & quality.
* Integrates with **GitHub, Bitbucket, Azure Repos**.
* Works across **multi-cloud & hybrid environments**.

---

