# Pre-Event Setup Guide

> **Before running commands:** this repository contains participant documentation only. Use the separate starter-kit URL shared in the event announcement. Its application projects, provisioning templates, smoke test, and development container must be available before you can run the commands below.

Finish everything here **before Day 1**. The **setup clinic is at T-3 days**, and anyone whose smoke test fails must attend. Plan for 45–60 minutes.

## 1. Prerequisites checklist

### Accounts & access
- [ ] An Azure subscription (one per participant) with **Owner** or **Contributor + User Access Administrator** rights. The provisioning step creates role assignments.
- [ ] Quota in the event region (recommended: **East US 2** or **Sweden Central**) of at least 50K TPM each for:
  - [ ] `gpt-4.1-mini` (Global Standard)
  - [ ] `text-embedding-3-small` (Standard)
- [ ] Resource providers registered: `Microsoft.CognitiveServices`, `Microsoft.Search`, `Microsoft.App`, `Microsoft.ServiceBus`, `Microsoft.Web`, `Microsoft.OperationalInsights`, `Microsoft.ContainerRegistry`
- [ ] A GitHub account with **GitHub Copilot** enabled
- [ ] Access to the starter repo `ai-hack-starter` (fork or template)

### Local tools
| Tool | Version | Check |
|---|---|---|
| .NET SDK | 9.0+ | `dotnet --version` |
| VS Code | latest | — |
| VS Code extensions | C# Dev Kit, GitHub Copilot + Copilot Chat, Bicep, Azure Resources, Azure Functions, Mermaid preview (optional) | Extensions view |
| Azure CLI | 2.60+ | `az version` |
| Azure Developer CLI | 1.10+ | `azd version` |
| Azure Functions Core Tools | v4 | `func --version` |
| Git | 2.40+ | `git --version` |
| Docker Desktop | **Optional.** `azd` can build remotely in ACR. | `docker version` |

> On a locked-down corporate laptop, use **GitHub Codespaces** instead. The repo includes `.devcontainer/` with every tool preinstalled.

### Knowledge (optional pre-reading, about 1 hour)
- C# basics: `async`/`await`, records, dependency injection in Minimal APIs
- Microsoft Learn: *What is Azure AI Foundry Agent Service?* and *Microsoft Agent Framework overview*
- Microsoft Learn: *RAG in Azure AI Search*

## 2. Setup steps

```powershell
# 1. Clone the repo
git clone https://github.com/<org>/ai-hack-starter.git
cd ai-hack-starter

# 2. Sign in (use the same tenant for both)
az login
azd auth login

# 3. Create an environment and choose region + subscription
azd env new hack-<yourname>
azd env set AZURE_LOCATION eastus2

# 4. Provision everything (about 10–15 min)
azd up
```

`azd up` provisions the following resources into a resource group named `rg-hack-<yourname>`.

| Resource | Used on | Notes |
|---|---|---|
| Azure AI Foundry resource + project | Day 1–4 | Agent Service endpoint |
| Model deployments: `gpt-4.1-mini`, `text-embedding-3-small` | Day 1–4 | |
| Azure AI Search (Basic) | Day 2–4 | Semantic ranker enabled |
| Storage account (blob container `docs`) | Day 2 | Source documents |
| Application Insights + Log Analytics | Day 1–4 | Tracing |
| Container Registry + Container Apps environment | Day 4 | |
| Service Bus namespace (Basic) + queue `inbound` | Day 4 | |
| Function App (Flex Consumption) | Day 4 | |
| Role assignments for **your user** and the **managed identity** | All | *Azure AI User*, *Search Index Data Contributor*, *Search Service Contributor*, *Storage Blob Data Contributor*, *Service Bus Data Owner* |

All code uses **`DefaultAzureCredential`**, so there are no keys in config. `azd` writes these values to `.azure/<env>/.env`, and the console app reads them:

```text
AZURE_AI_PROJECT_ENDPOINT=https://<resource>.services.ai.azure.com/api/projects/<project>
AZURE_AI_MODEL_DEPLOYMENT=gpt-4.1-mini
AZURE_AI_EMBEDDING_DEPLOYMENT=text-embedding-3-small
AZURE_SEARCH_ENDPOINT=https://<search>.search.windows.net
APPLICATIONINSIGHTS_CONNECTION_STRING=...
SERVICEBUS_FQDN=<ns>.servicebus.windows.net
```

## 3. Smoke test (must pass before Day 1)

```powershell
dotnet run --project src/HackAgent.Console -- --smoke-test
```

Expected output:

```text
[OK] Azure credential acquired (user: you@contoso.com)
[OK] Foundry project reachable
[OK] Model 'gpt-4.1-mini' responded: "pong"
[OK] Embedding model returned 1536 dims
[OK] AI Search reachable (0 indexes)
All checks passed.
```

Post a screenshot of the passing output in the event chat channel `#setup`. Redact account details and sensitive configuration.

## 4. Troubleshooting

| Symptom | Fix |
|---|---|
| `InsufficientQuota` during `azd up` | Change region (`azd env set AZURE_LOCATION swedencentral`) or lower the TPM capacity in `infra/main.parameters.json`. |
| `401/403 PermissionDenied` calling the agent or Search | The role assignment hasn't propagated yet. Wait 5 minutes, then run `az logout; az login`. Check that the `az` and `azd` tenants match. |
| `DefaultAzureCredential` picks the wrong account | Set `AZURE_TENANT_ID` in `.env`, or sign out of other accounts in VS Code. |
| Corporate proxy blocks `*.services.ai.azure.com` | Use Codespaces. |
| `azd up` fails on the Functions/ACA step | That's fine before Day 4. Run `azd provision` again on Day 4. |

## 5. Teardown (end of Day 4)

```powershell
azd down --purge --force
```

`--purge` requests permanent removal of supported soft-deleted resources and can be irreversible. Preview the target environment and confirm it contains only workshop resources before teardown. Verify in the portal that all workshop resource groups and billable resources are gone; do not assume quota has been released without checking.
