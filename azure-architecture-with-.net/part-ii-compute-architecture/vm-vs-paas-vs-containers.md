---
description: Compute Decision Framework
icon: square-dashed-circle-plus
---

# VM vs PaaS vs Containers

> This section answers one of the **most important architect questions**:\
> &#xNAN;**“Which compute should I choose for this workload, and why?”**

Most production failures happen because **the wrong compute model was chosen**, not because Azure failed.

***

### The Core Problem (Why This Matters)

Azure gives you **many ways to run .NET**, but:

* More choice ≠ better architecture
* Wrong choice = cost issues, scaling issues, ops pain

An Azure Architect must **decide**, not guess.

***

### The Three Compute Models (Big Picture)

<table><thead><tr><th width="198.54296875">Compute Model</th><th width="298.17578125">Azure Examples</th></tr></thead><tbody><tr><td><strong>IaaS (VMs)</strong></td><td>Azure Virtual Machines</td></tr><tr><td><strong>PaaS</strong></td><td>App Service, Azure Functions</td></tr><tr><td><strong>Containers</strong></td><td>ACI, AKS</td></tr></tbody></table>

Think of this as a **control vs responsibility spectrum**.

<div align="left" data-with-frame="true"><figure><img src="../../.gitbook/assets/image (10).png" alt="" width="563"><figcaption></figcaption></figure></div>

### Mental Model (Very Important)

> **More control = more responsibility**\
> **Less control = more platform help**

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

***

### <mark style="color:$tint;">**Option 1: Virtual Machines (IaaS)**</mark>

#### What It Really Means

* You manage:
  * OS
  * Patching
  * Scaling
  * Security hardening
* Azure manages:
  * Hardware
  * Datacenter

#### When Architects Choose VMs

* Legacy .NET Framework apps
* Custom OS-level dependencies
* Lift-and-shift migrations
* Vendor software that requires VM

#### Real-Life Example

> A 10-year-old .NET Framework app using Windows services and local file system\
> → **VM is the correct first step**

#### Hidden Cost (Interview Insight)

* Slow scaling
* High ops effort
* Patch management burden

***

### <mark style="color:$tint;">**Option 2: PaaS (App Service / Functions)**</mark>

#### What It Really Means

* Azure manages:
  * OS
  * Patching
  * Scaling (mostly)
  * Availability
* You focus on:
  * Code
  * Config
  * Business logic

#### Why Architects Prefer PaaS

<div align="left" data-with-frame="true"><figure><img src="../../.gitbook/assets/image (13).png" alt="" width="375"><figcaption></figcaption></figure></div>

#### Real-Life Example

> REST API written in ASP.NET Core\
> Needs auto-scale, SSL, monitoring\
> → **Azure App Service is ideal**

#### Architect Insight

> If you are starting fresh with .NET → **default to PaaS**

***

### Option 3: Containers (ACI / AKS)

#### What Containers Actually Solve

* Consistent runtime
* Environment parity
* Microservice packaging

Containers **do not automatically mean AKS**.

<div align="left" data-with-frame="true"><figure><img src="../../.gitbook/assets/image (14).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left" data-with-frame="true"><figure><img src="../../.gitbook/assets/image (15).png" alt="" width="563"><figcaption></figcaption></figure></div>

#### Two Container Paths

| Service | Use Case                       |
| ------- | ------------------------------ |
| **ACI** | Simple, short-lived containers |
| **AKS** | Complex, large-scale systems   |

#### Real-Life Example

> Multiple services, independent scaling, DevOps maturity\
> → **AKS (only if team is ready)**

#### Architect Warning

> AKS is not a compute choice — it is an **operating model**

***

### The Compute Decision Table (Interview-Ready)

| Requirement         | Best Choice     |
| ------------------- | --------------- |
| Legacy app          | VM              |
| New .NET API        | App Service     |
| Event-driven jobs   | Azure Functions |
| Simple container    | ACI             |
| Large microservices | AKS             |
| Small team          | PaaS            |
| Strong DevOps team  | Containers      |

***

### Common Beginner Mistakes (Very Important)

#### ❌ “AKS is modern, so we should use AKS”

Reality:

* High ops overhead
* Needs Kubernetes expertise
* Overkill for many apps

#### ❌ “VM gives full control, so it’s better”

Reality:

* Control comes with operational cost
* Slower innovation

***

### Architect’s Golden Rule (Memorize This)

> **Choose the simplest compute that meets today’s requirements and can evolve tomorrow.**

***

{% hint style="info" %}
“I choose compute based on r**esponsibility trade-offs**—**VMs** for **legacy control**, **PaaS** for **productivity**, and **containers** only when _**scale and complexity justify them**_.”
{% endhint %}

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>
