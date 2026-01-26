---
description: Secure VM Access with Azure Bastion
icon: file-shield
---

# Azure Bastion

> **Azure Bastion solves one very specific but very dangerous problem:**\
> **How do admins securely access VMs without exposing them to the internet?**

### What Is Azure Bastion (Plain English)

**Azure Bastion is a managed service that lets you RDP/SSH into Azure VMs securely over HTTPS, directly from the Azure Portal, without giving the VM a public IP.**

#### In one sentence:

> Azure Bastion removes the need to open RDP (3389) or SSH (22) to the internet.

***

### The Problem Azure Bastion Solves (Real Life)

#### ❌ Traditional VM Access (Dangerous)

* VM has a **public IP**
* RDP (3389) / SSH (22) open to internet
* Bots continuously scan and attack

**This is one of the most common cloud security mistakes.**

#### ✅ With Azure Bastion

* VM has **NO public IP**
* Admin connects via Azure Portal
* Traffic flows over **HTTPS (443)**
* VM stays private inside VNet

> **No open ports. No exposed IP. No brute-force attacks.**

***

### <mark style="color:$tint;">Azure Bastion Architecture</mark>

<figure><img src="../../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

```javascript
Admin Browser
   ↓ (HTTPS 443)
Azure Portal
   ↓
Azure Bastion (inside VNet)
   ↓
Private IP of VM

👉 The VM is never exposed to the internet.

```

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

***

### Where Azure Bastion Lives (Important Concept)

Azure Bastion is:

* Deployed **inside your Virtual Network**
*   Placed in a **special subnet** called:

    ```
    AzureBastionSubnet
    ```

It acts like a **secure jump host**, but **fully managed by Azure**.

***

### Why Azure Bastion Is Better Than Old Approaches

<table><thead><tr><th width="234.40625">Approach</th><th width="330.62109375">Problem</th></tr></thead><tbody><tr><td>Public IP + RDP</td><td>Extremely unsafe</td></tr><tr><td>Jump Box VM</td><td>Still needs patching</td></tr><tr><td>VPN</td><td>Heavy setup for small teams</td></tr><tr><td><strong>Azure Bastion</strong></td><td>Secure, managed, simple</td></tr></tbody></table>

> **Architect rule:**\
> If your team needs occasional admin access → Bastion is ideal.

***

### When Should You Use Azure Bastion

#### ✅ Use Bastion When

* You have **Azure VMs**
* You want **zero public exposure**
* You want **simple admin access**
* You don’t want to manage jump boxes

#### ❌ Do NOT Use Bastion When

* No VMs (PaaS services)
* High-frequency admin access (VPN may be better)
* Strict cost constraints (Bastion is not free)

***

### Real-Life Example (Very Relatable)

#### Scenario

You have:

* A Windows VM
* Hosting a **legacy .NET Framework app**
* Used only by internal users

#### Wrong Design

* Public IP on VM
* RDP open to world

#### Correct Design

* VM has **private IP only**
* Azure Bastion enabled
* Admin connects via Portal → Bastion → VM

**Result:**\
Secure access, zero internet exposure.

***

### **Step-by-Step: Enabling Azure Bastion (Conceptual)**

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

#### Step 1: Ensure VNet Exists

* VM must be inside a VNet

#### Step 2: Create Subnet

* Name: `AzureBastionSubnet`
* Minimum size: `/26`

#### Step 3: Create Bastion

* Select VNet
* Choose SKU
* Azure handles the rest

#### Step 4: Connect to VM

* Go to VM → **Connect** → **Bastion**
* Login via browser

***

### How Bastion Works with .NET Applications (Important)

#### Key Point (Many Miss This)

> **Azure Bastion does NOT host or run your .NET app.**\
> It only provides **secure admin access** to the VM.

#### Typical .NET Use Case

* VM hosts:
  * IIS
  * ASP.NET Framework / Core app
* Admin uses Bastion to:
  * RDP into VM
  * Deploy app
  * Check logs
  * Restart services

***

### Example: .NET Deployment via Bastion (Concept Flow)

1. Admin connects using **Azure Bastion**
2. RDP session opens in browser
3. Admin:
   * Opens IIS
   * Deploys ASP.NET app
   * Verifies service

**No public IP. No open ports.**

***

### Bastion + NSG Rules (Very Important)

With Bastion:

* ❌ Do NOT open 3389 / 22 to internet
* ✅ Allow Bastion subnet to access VM
* ❌ No inbound rules from `0.0.0.0/0`

> Bastion works **with** NSG, not against it.

***

### Cost Awareness (Architect Insight)

Azure Bastion:

* Is billed hourly
* Has fixed cost
* Worth it for **security + simplicity**

> Cheaper than:
>
> * Security incidents
> * Jump box maintenance
> * Compliance violations

***

### Interview-Ready Q\&A (Must Memorize)

**Q: What is Azure Bastion?**

> A managed service that provides secure RDP/SSH access to Azure VMs over HTTPS without exposing them to the internet.

**Q: Why use Bastion instead of public IP?**

> To eliminate internet exposure and prevent brute-force attacks.

**Q: Does Bastion replace VPN?**

> No. Bastion is for admin access; VPN is for full network connectivity.

***

### One-Line Architect Rule (Gold)

> **If a VM has a public IP and open RDP/SSH, it is already insecure.**

***

### Side-by-Side Comparison (Cheat Table)

| Feature              | Azure Bastion | VPN           | Jump Box         |
| -------------------- | ------------- | ------------- | ---------------- |
| Public IP on VM      | ❌ No          | ❌ No          | ⚠️ Yes (usually) |
| RDP/SSH exposed      | ❌ No          | ❌ No          | ⚠️ Often         |
| Managed by Azure     | ✅ Yes         | ⚠️ Partial    | ❌ No             |
| Setup complexity     | ⭐ Easy        | ⭐⭐⭐ Medium    | ⭐⭐ Medium        |
| Ongoing ops          | ⭐ Low         | ⭐⭐ Medium     | ⭐⭐⭐ High         |
| Network-wide access  | ❌ No          | ✅ Yes         | ❌ No             |
| Security posture     | ✅ Very strong | ✅ Very strong | ❌ Weak           |
| Modern best practice | ✅ Yes         | ✅ Yes         | ❌ No             |
