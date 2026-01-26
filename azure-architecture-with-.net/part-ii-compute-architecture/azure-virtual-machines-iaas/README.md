---
icon: desktop-arrow-down
---

# Azure Virtual Machines (IaaS)

> Azure VMs give you **maximum control** and **maximum responsibility**.\
> This section teaches **how to create VMs correctly**, not just how to click buttons.

### <mark style="color:$tint;">**When SHOULD You Use Azure VMs (Quick Context)**</mark>

Before creating anything, an architect asks **why VM**.

#### Valid VM Use Cases

* Legacy **.NET Framework** apps
* Apps needing **Windows Services**
* Vendor software requiring OS access
* Lift-and-shift from on-prem

#### When VM is a BAD choice

* New ASP.NET Core APIs
* Burst traffic workloads
* Small teams without ops maturity

> **Architect rule:** VM is never the default choice—only a justified one.

***

### <mark style="color:$tint;">**VM Architecture (What You Are Building)**</mark>

A **VM** consists of:

* _Compute_ (CPU/RAM)
* _OS_ (Windows/Linux)
* _Disk_ (OS + Data)
* _Network_ (VNet + NIC)
* _Security_ (NSG, identity)

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (17).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Step-by-Step: Creating an Azure VM (Portal Way)

> This is how **90% of real teams** start.

<div align="left"><figure><img src="../../../.gitbook/assets/image (18).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/image (20).png" alt="" width="375"><figcaption></figcaption></figure></div>

***

#### Step 1: Basics Tab (MOST IMPORTANT)

**Key decisions here:**

| Setting        | What to Choose          | Architect Reason   |
| -------------- | ----------------------- | ------------------ |
| Subscription   | Correct env (Dev/Prod)  | Cost & isolation   |
| Resource Group | One per app             | Lifecycle control  |
| VM Name        | Meaningful              | Ops clarity        |
| Region         | Closest to users        | Latency            |
| Availability   | Availability Set / Zone | HA                 |
| Image          | Windows Server 2022     | .NET compatibility |
| Size           | Start small             | Cost control       |
| Auth           | SSH / Password          | Security           |

> **Mistake to avoid:** Choosing a large VM “just in case”.

***

### <mark style="color:$tint;">**VM Size Selection (Critical Decision)**</mark>

#### Practical Guidance

* **Dev/Test** → B-series
* **General workloads** → D-series
* **Compute-heavy** → F-series

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (22).png" alt="" width="375"><figcaption></figcaption></figure></div>

> **Architect insight:** Scaling up is easy. Scaling down politically is hard.

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (21).png" alt="" width="563"><figcaption></figcaption></figure></div>

***

### <mark style="color:$tint;">**Disks & Storage (Often Ignored, Later Regretted)**</mark>

#### Best Practices

* OS Disk → Standard SSD
* App/Data Disk → Premium SSD (if needed)
* Separate OS and data disks

**Never store app data on OS disk.**

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (23).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Disk Types (What Azure Offers)

| Disk Type          | Typical Use Case                   | Max IOPS  | Max Throughput | Cost          | Notes                          |
| ------------------ | ---------------------------------- | --------- | -------------- | ------------- | ------------------------------ |
| **Ultra Disk**     | Mission-critical DBs (SAP, Oracle) | \~160,000 | \~2,000 MB/s   | 💰💰💰💰      | Lowest latency, very expensive |
| **Premium SSD v2** | High-performance production apps   | \~80,000  | \~1,200 MB/s   | 💰💰💰        | Best modern choice for prod    |
| **Premium SSD**    | Production workloads               | \~20,000  | \~900 MB/s     | 💰💰          | Common for prod VMs            |
| **Standard SSD**   | Dev/Test, light prod               | \~6,000   | \~750 MB/s     | 💰            | Balanced cost/perf             |
| **Standard HDD**   | Backup, archive                    | \~2,000   | \~500 MB/s     | 💰 (cheapest) | Slow, not for apps             |

> Numbers vary by disk size and region, but **relative comparison is what matters for architecture decisions**.

***

## How Architects Actually Choose Disks (Simple Logic)

### 1. OS Disk (Almost Always)

**Recommended**

* ✅ **Standard SSD**

**Why**

* OS does not need ultra performance
* Cheap + reliable
* Faster boot than HDD

❌ Avoid:

* Premium SSD for OS (waste of money)
* HDD for OS (slow boot, bad experience)

***

### 2. Application / Data Disk (Depends on Load)

#### Low traffic / Dev / QA

* ✅ **Standard SSD**

#### Production Web App / API

* ✅ **Premium SSD**
* ✅ **Premium SSD v2** (if available)

#### Heavy Database / IO-intensive Workload

* ✅ **Ultra Disk** (only if truly needed)

> **Architect rule:**\
> Disk performance should match _actual IO demand_, not fear.

***

## Real-Life Example (Very Relatable)

#### ❌ Bad Design

* App logs + database stored on OS disk
* OS disk fills up
* VM crashes

#### ✅ Correct Design

* OS disk → Standard SSD
* Data disk → Premium SSD
* Logs → Separate data disk or Azure Monitor

***

### <mark style="color:$tint;">**Networking & Security (Do NOT Skip This)**</mark>

#### Key Rules

* Place VM inside a **VNet**
* Use **NSG** to restrict ports
* Avoid public IP unless required
* Never open RDP (3389) to `0.0.0.0/0`

> **Security is configuration, not intention.**

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (24).png" alt="" width="563"><figcaption></figcaption></figure></div>

> “VM security in Azure is enforced through VNet isolation, NSG-based port restrictions, and private access patterns—security depends on configuration, not intention.”

***

### Post-Creation Checklist (Architect View)

After VM is created, **your work has just started**.

<table><thead><tr><th width="262.609375">Task</th><th>Why</th></tr></thead><tbody><tr><td>Enable backups</td><td>Accidental deletion</td></tr><tr><td>Enable monitoring</td><td>Visibility</td></tr><tr><td>Patch OS</td><td>Security</td></tr><tr><td>Restrict access</td><td>Least privilege</td></tr></tbody></table>

***

## <mark style="background-color:purple;">**Deploying a Simple .NET App to the VM**</mark>

Now let’s make this **real**.

#### Example: Simple ASP.NET Core App

```bash
dotnet new webapp -n VmDemoApp
cd VmDemoApp
dotnet publish -c Release
```

This creates a **`publish`** folder.

***

#### Copy App to VM

Options:

* SCP
* RDP copy
* Azure Bastion (preferred)

***

#### Install .NET Runtime on VM (Windows)

```powershell
winget install Microsoft.DotNet.SDK.8
```

***

#### Run the App on VM

```powershell
dotnet VmDemoApp.dll --urls "http://0.0.0.0:5000"
```

Open port **5000** in NSG.

***

### Production-Grade VM Hosting (Important)

Never run apps manually in production.

#### Correct Options

* IIS + ASP.NET Hosting Bundle
* Windows Service
* Reverse proxy (Nginx / IIS)

> **Interview insight:**\
> “Manually running apps on VM is acceptable for demos, not production.”

***

### VM Scaling Reality (Hard Truth)

VMs:

* Scale **slowly**
* Need **VM Scale Sets**
* Require **load balancer**
* Increase ops complexity

> This is why PaaS exists.

***

### VM Anti-Patterns (Interview Gold)

❌ Using VM for simple API\
❌ No backups\
❌ Public RDP access\
❌ No monitoring\
❌ Treating VM like on-prem

***

> “Azure VMs provide full OS control but come with operational overhead, making them suitable for legacy workloads rather than cloud-native .NET applications.”

***
