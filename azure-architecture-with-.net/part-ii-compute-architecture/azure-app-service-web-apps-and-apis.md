---
icon: command
---

# Azure App Service (Web Apps & APIs)

> Azure App Service is **PaaS for web workloads** — meaning Azure manages the OS, patching, scaling, and availability, so you focus on **.NET code and business logic**.

***

<div align="left" data-with-frame="true"><figure><img src="../../.gitbook/assets/image (32).png" alt="" width="563"><figcaption></figcaption></figure></div>

### 2.3.1 Why Azure App Service Exists

#### The Problem with VMs

* OS patching
* RDP access
* Manual scaling
* Security hardening
* Ops-heavy

#### What App Service Solves

* No VM management
* Built-in HTTPS
* Auto-scale
* High availability
* Deep .NET integration

> **Architect rule:**\
> If your workload is HTTP-based → **App Service should be your default**.

***

### 2.3.2 App Service Architecture (Mental Model)

<div align="left" data-with-frame="true"><figure><img src="../../.gitbook/assets/image (29).png" alt="" width="563"><figcaption></figcaption></figure></div>

#### How It Works

```
User
 ↓
Azure Front-End (Managed)
 ↓
App Service Plan (Compute)
 ↓
Your .NET App
```

You **never see the VM**, but it exists under the hood.

### 2.3.3 Core Components (Must Understand)

<table><thead><tr><th width="208.61328125" valign="top">Component</th><th width="285.58984375">What It Is</th></tr></thead><tbody><tr><td valign="top">App Service Plan</td><td>Compute (CPU/RAM/Scale)</td></tr><tr><td valign="top">Web App</td><td>Your application</td></tr><tr><td valign="top">Runtime</td><td>.NET version</td></tr><tr><td valign="top">Configuration</td><td>App settings</td></tr><tr><td valign="top">Slots</td><td>Deployment safety</td></tr></tbody></table>

> **Interview trap:**\
> **App Service ≠ App Service Plan**\
> (Plan = compute, App = code)

***

### 2.3.4 Step 1 – Create a .NET App Locally

#### Create ASP.NET Core Web API

```bash
dotnet new webapi -n AppServiceDemo
cd AppServiceDemo
dotnet run
```

Open:

```
https://localhost:5001/weatherforecast
```

✔ Local development done\
✔ No Azure yet

***

### 2.3.5 Step 2 – Prepare App for Cloud (Important)

#### Key Cloud Principles

* No hardcoded config
* No local file dependencies
* Use environment variables

#### Example: Configuration

```csharp
var builder = WebApplication.CreateBuilder(args);

var connectionString =
    builder.Configuration.GetConnectionString("DefaultConnection");
```

Azure will inject this later.

***

### 2.3.6 Step 3 – Create Azure App Service (Portal)

<div align="left" data-with-frame="true"><figure><img src="../../.gitbook/assets/image (30).png" alt="" width="563"><figcaption></figcaption></figure></div>

#### Key Choices (Architect Focus)

<table><thead><tr><th width="226.94140625">Setting</th><th width="233.12890625">What to Choose</th><th>Why</th></tr></thead><tbody><tr><td>Publish</td><td>Code</td><td>.NET app</td></tr><tr><td>Runtime</td><td>.NET 8</td><td>LTS</td></tr><tr><td>OS</td><td>Linux (preferred)</td><td>Cheaper, faster</td></tr><tr><td>App Service Plan</td><td>Basic / Standard</td><td>Start small</td></tr><tr><td>Region</td><td>Near users</td><td>Latency</td></tr></tbody></table>

> **Architect tip:**\
> Linux App Service + .NET is the modern default.

### 2.3.7 Step 4 – Deploy from Local to Azure

#### Option 1: Azure CLI (Most Realistic)

```bash
az login
az webapp up \
  --name appservice-demo-123 \
  --resource-group rg-appservice-demo \
  --runtime "DOTNET:8"
```

✔ Azure creates resources\
✔ App deployed\
✔ URL available instantly

***

#### Option 2: Visual Studio Publish

* Right click project
* Publish → Azure App Service
* Guided, beginner-friendly

***

### 2.3.8 Step 5 – App Settings & Configuration

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

#### Why This Matters

* No secrets in code
* Environment-specific config

#### Example

| Key                                        | Value            |
| ------------------------------------------ | ---------------- |
| **ASPNETCORE\_ENVIRONMENT**                | Production       |
| **ConnectionStrings\_\_DefaultConnection** | Secure DB string |

> **App Service injects these as environment variables**

***

### 2.3.9 Scaling in App Service (Huge Advantage)

#### Two Types of Scaling

<table><thead><tr><th width="182.578125">Type</th><th width="200.1328125">Meaning</th></tr></thead><tbody><tr><td>Scale Up</td><td>Bigger machine</td></tr><tr><td>Scale Out</td><td>More instances</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

#### Architect Rule

> Prefer **scale out** over scale up.

***

### 2.3.10 Deployment Slots (Production Safety)

#### Why Slots Matter

* Zero downtime deploys
* Test in production safely

#### Flow

<div align="left" data-with-frame="true"><figure><img src="../../.gitbook/assets/image (34).png" alt="" width="373"><figcaption></figcaption></figure></div>

```
Deploy → Staging Slot
Test
Swap with Production
```

> This is **blue-green deployment**, built-in.

***

### 2.3.11 Real Production Issues (And Mitigation)

#### Issue 1: App Restart During Scale

**Mitigation**

* Stateless design
* Use Redis / DB for state

***

#### Issue 2: Cold Start

**Mitigation**

* Always On enabled
* Warm-up endpoint

***

#### Issue 3: Secrets Exposure

**Mitigation**

* Managed Identity
* Azure Key Vault

***

#### Issue 4: Logging Blindness

**Mitigation**

* Application Insights
* Structured logs

***

### 2.3.12 App Service + Azure Services (Real Stack)

Typical Production Setup:

* App Service (API)
* Azure SQL / Cosmos DB
* Key Vault
* Application Insights
* Private Endpoints (optional)

***

### App Service Anti-Patterns (Interview Gold)

❌ Treating App Service like a VM\
❌ Writing to local disk\
❌ Hardcoding secrets\
❌ No deployment slots\
❌ No autoscale rules

***

> “Azure App Service is a PaaS offering that removes OS and infrastructure management, making it ideal for scalable, secure, and production-ready .NET web applications.”

***

### Azure App Service — Real Outage Case Studies

<details>

<summary>🔴 Case 1: App Goes Down During Traffic Spike</summary>

#### What Happened

* ASP.NET Core API hosted on App Service
* Marketing campaign launched
* Traffic increased **5× in 10 minutes**
* App started returning **503 errors**

#### Root Cause

* App Service Plan had:
  * Only **1 instance**
  * No autoscale rules
* CPU hit 100%

#### How the Team Fixed It

* Enabled **autoscale**
* Added rule:
  * Scale out when CPU > 70%
* Increased max instances

#### Architect Lesson

> **App Service does not autoscale unless you configure it**

</details>

<details>

<summary>🔴 Case 2: App Restarts Randomly (Users Lose Sessions)</summary>

#### What Happened

* Users logged out unexpectedly
* In-progress operations failed
* No clear error logs

#### Root Cause

* App stored session data **in memory**
* App Service recycled instance during:
  * Scale
  * Platform patching

#### How the Team Fixed It

* Made app **stateless**
* Moved session state to:
  * Redis Cache
  * Database

#### Architect Lesson

> **Assume App Service instances are disposable**

</details>

<details>

<summary>🔴 Case 3: Cold Start Causing Slow First Request</summary>

#### What Happened

* First user request took **20–30 seconds**
* Users thought app was broken
* Happened after inactivity

#### Root Cause

* App Service went idle
* Cold start on first request

#### How the Team Fixed It

* Enabled **Always On**
* Added warm-up endpoint
* Reduced startup logic

#### Architect Lesson

> **Cold start is an architectural concern, not a bug**

</details>

<details>

<summary>🔴 Case 4: Deployment Broke Production Instantly</summary>

#### What Happened

* New version deployed
* Production app crashed immediately
* Rollback took time

#### Root Cause

* Deployment done directly to production
* No staging or validation

#### How the Team Fixed It

* Introduced **Deployment Slots**
* Deployed to **Staging**
* Tested → Swapped to Production

#### Architect Lesson

> **Never deploy directly to production**

</details>

<details>

<summary>🔴 Case 5: App Works Locally but Fails in Azure</summary>

#### What Happened

* App ran fine locally
* Failed in App Service
* File not found errors

#### Root Cause

* App wrote files to local disk
* App Service filesystem is not persistent

#### How the Team Fixed It

* Moved files to:
  * Azure Blob Storage
* Treated App Service as **stateless**

#### Architect Lesson

> **App Service is not a VM**

</details>

<details>

<summary>🔴 Case 6: Secrets Leaked in Git Repository</summary>

#### What Happened

* DB password committed to Git
* Repo accidentally exposed
* Security incident raised

#### Root Cause

* Secrets hardcoded in `appsettings.json`

#### How the Team Fixed It

* Removed secrets from code
* Used:
  * App Service Configuration
  * Azure Key Vault
  * Managed Identity

#### Architect Lesson

> **Secrets must never live in source code**

</details>

<details>

<summary>🔴 Case 7: Sudden App Crash After Scale-Out</summary>

#### What Happened

* App scaled out correctly
* New instances crashed immediately

#### Root Cause

* Startup code depended on:
  * Local cache
  * Singleton state

#### How the Team Fixed It

* Made startup idempotent
* Removed static initialization assumptions

#### Architect Lesson

> **Every instance must start cleanly and independently**

</details>

<details>

<summary>🔴 Case 8: App Is Slow but No Errors Visible</summary>

#### What Happened

* Users complained about slowness
* No exceptions in logs
* Difficult troubleshooting

#### Root Cause

* No Application Insights
* No dependency tracking

#### How the Team Fixed It

* Enabled **Application Insights**
* Found slow SQL queries
* Added caching & async calls

#### Architect Lesson

> **If you can’t observe it, you can’t fix it**

</details>

<details>

<summary>🔴 Case 9: Unexpected Downtime During Platform Update</summary>

#### What Happened

* App restarted during Azure maintenance
* Short downtime observed

#### Root Cause

* Single instance App Service
* No redundancy

#### How the Team Fixed It

* Increased instance count to 2+
* Enabled zone redundancy (where available)

#### Architect Lesson

> **High availability requires multiple instances**

</details>

<details>

<summary>🔴 Case 10: Cost Spikes Without Traffic Growth</summary>

#### What Happened

* Cloud bill increased suddenly
* Traffic unchanged

#### Root Cause

* Autoscale rules too aggressive
* Logs retained indefinitely

#### How the Team Fixed It

* Limited max scale-out
* Reduced log retention
* Added Azure Budget alerts

#### Architect Lesson

> **Cost must be designed, not monitored later**

</details>

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

> <kbd>“Most App Service outages happen due to missing autoscale, stateful design, lack of deployment slots, or poor observability. These are architectural issues, not Azure failures.”</kbd>

***

### Architect’s Golden Insight

> **Azure App Service is extremely reliable —**\
> **most outages are caused by treating it like a VM.**

***
