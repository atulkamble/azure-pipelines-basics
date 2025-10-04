##  **Azure Pipelines basics** 👇

---

## 🚀 What is Azure Pipelines?

Azure Pipelines (part of **Azure DevOps**) is a **CI/CD (Continuous Integration & Continuous Delivery) service**. It lets you **automate builds, tests, and deployments** across platforms like **Azure, AWS, GCP, on-prem servers, or containers**.

It works with **any language, any platform, any cloud**.

* Languages: Java, Python, .NET, Node.js, Go, PHP, etc.
* Targets: Azure, AWS, GCP, VMs, Kubernetes, Docker, or even on-prem servers.

---

## ⚙️ Key Concepts

1. **Pipeline**

   * A process that defines how code moves from commit → build → test → deploy.
   * Defined in a **YAML file** (`azure-pipelines.yml`) stored in your repo.

2. **Stages**

   * Logical sections in the pipeline (e.g., *Build*, *Test*, *Deploy*).

3. **Jobs**

   * A collection of steps that run on an **agent** (machine).

4. **Steps**

   * Individual tasks (like installing dependencies, running tests, deploying).

5. **Agents**

   * The machine that executes the jobs.
   * Microsoft-hosted agents (ready to use) or self-hosted agents (your own servers).

---

## 📝 Sample Pipeline (azure-pipelines.yml)

Here’s a set of **very basic Azure Pipelines YAML codes** you can use as starting points. These are minimal examples for common scenarios:

---

## 1. Hello World Pipeline (Ubuntu Agent)

```yaml
# azure-pipelines.yml
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: echo "Hello, Azure Pipelines 👋"
  displayName: "Print Hello World"
```

---

## 2. Run Python Script

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- task: UsePythonVersion@0
  inputs:
    versionSpec: '3.x'

- script: python hello.py
  displayName: "Run Python Script"
```

---

## 3. Run Node.js App

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

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

## 4. Simple .NET Build

```yaml
trigger:
- main

pool:
  vmImage: windows-latest

steps:
- task: UseDotNet@2
  inputs:
    packageType: 'sdk'
    version: '8.x'

- script: dotnet build --configuration Release
  displayName: "Build .NET Project"
```

---

## 5. Docker Build & Push to ACR

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

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

```yaml
trigger:
- main   # Run pipeline when code is pushed to main branch

pool:
  vmImage: ubuntu-latest   # Agent image

steps:
- task: UseNode@2
  inputs:
    version: '18.x'

- script: npm install
  displayName: 'Install dependencies'

- script: npm test
  displayName: 'Run tests'

- task: AzureWebApp@1
  inputs:
    azureSubscription: 'my-azure-connection'
    appName: 'my-nodejs-app'
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
* Integrates with GitHub, Bitbucket, or Azure Repos.
* Works across **multi-cloud + hybrid environments**.

---
