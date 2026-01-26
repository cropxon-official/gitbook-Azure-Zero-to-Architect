---
icon: grid-4
---

# Azure Architecture with .NET

### Azure Architecture with .NET

{% stepper %}
{% step %}
#### PART I — Azure & Cloud First Principles

{% hint style="info" %}
Architect Mindset Foundation
{% endhint %}

<details>

<summary>Chapter 1 – Thinking Like an Azure Architect</summary>

* [ ] 1.1 Role of an Azure Architect
* [ ] 1.2 Azure Global Infrastructure (Regions, AZs, Pairs)
* [ ] 1.3 Control Plane vs Data Plane
* [ ] 1.4 Shared Responsibility Model
* [ ] 1.5 Azure Resource Hierarchy & Governance
* [ ] 1.6 Azure Well-Architected Framework
* [ ] 1.7 Architect Failure Case Studies
* [ ] 1.8 Architecture Decision Checklists
* [ ] 1.9 Interview Questions & Traps

</details>
{% endstep %}

{% step %}
#### **PART II — Compute Architecture**

{% hint style="info" %}
#### **Where .NET Runs**
{% endhint %}

<details>

<summary>Chapter 2 – Azure Compute Landscape for .NET</summary>

* [ ] 2.1 Compute Decision Framework (VM vs PaaS vs Containers)
* [ ] 2.2 Azure Virtual Machines (IaaS)
* [ ] 2.3 Azure App Service (Web Apps & APIs)
* [ ] 2.4 Azure Functions (Serverless .NET)
* [ ] 2.5 Azure Container Instances (ACI)
* [ ] 2.6 Azure Kubernetes Service (AKS)
* [ ] 2.7 Scaling Models & SLAs
* [ ] 2.8 Cold Starts, Warm Paths & Latency
* [ ] 2.9 Compute Anti-Patterns

📌 **Per-Service Coverage**

* 3–4 case studies
* 5–8 diagrams
* Cost & scale analysis
* When NOT to use

</details>
{% endstep %}

{% step %}
#### PART III — Data & Storage Architecture

{% hint style="info" %}
State, Consistency, Scale
{% endhint %}

<details>

<summary>Chapter 3 – Data Architecture on Azure</summary>

* [ ] 3.1 Data Architecture Principles
* [ ] 3.2 Azure SQL Database
* [ ] 3.3 Azure SQL Managed Instance
* [ ] 3.4 Cosmos DB (SQL API Focus)
* [ ] 3.5 Blob Storage (Hot, Cool, Archive)
* [ ] 3.6 Azure Table Storage
* [ ] 3.7 Azure Redis Cache
* [ ] 3.8 Backup, DR & Geo-Replication
* [ ] 3.9 Polyglot Persistence Patterns

📌 **Mandatory Case Studies**

* Multi-tenant SaaS data model
* High-write ingestion systems
* Cosmos DB RU/s cost failure
* Backup & restore disaster

</details>

<details>

<summary>Chapter 3A – Data Movement &#x26; Analytics (Architect View)</summary>

{% hint style="info" %}
High-level, non-ML specialisation
{% endhint %}

* [ ] 3A.1 Azure Data Factory (ETL / ELT fundamentals)
* [ ] 3A.2 Azure Synapse Analytics (when & why, not deep SQL)
* [ ] 3A.3 Operational Data vs Analytical Data
* [ ] 3A.4 Cost & complexity trade-offs

📌 **Case Studies**

* OLTP vs OLAP separation in SaaS
* Data Factory overuse anti-pattern
* Synapse introduced too early
* Analytics isolation for cost control

> **Depth rule:** Architect decisions only.\
> No pipeline authoring, no Spark internals.

</details>
{% endstep %}

{% step %}
#### PART IV — Identity, Security & Governance

{% hint style="info" %}
Zero Trust by Design
{% endhint %}

<details>

<summary><strong>Chapter 4 – Secure-by-Design Azure Systems</strong></summary>

* [ ] 4.1 Security as an Architectural Concern
* [ ] 4.2 Microsoft Entra ID (Azure AD)
* [ ] 4.3 Managed Identity Patterns
* [ ] 4.4 Azure Key Vault
* [ ] 4.5 Role-Based Access Control (RBAC)
* [ ] 4.6 Azure Policy & Blueprints
* [ ] 4.7 Defender for Cloud
* [ ] 4.8 Compliance-Driven Architectures

📌 **Case Studies**

* Secret-less application design
* Enterprise RBAC model
* Security breach root cause
* SOC2 / ISO alignment

</details>
{% endstep %}

{% step %}
#### PART V — Networking Architecture

{% hint style="info" %}
The Architect Filter Chapter
{% endhint %}

<details>

<summary><strong>Chapter 5 – Azure Networking That Works in Production</strong></summary>

* [ ] 5.1 Networking Fundamentals for Architects
* [ ] 5.2 Virtual Networks & Subnets
* [ ] 5.3 Network Security Groups (NSGs)
* [ ] 5.4 Azure Load Balancer
* [ ] 5.5 Application Gateway (L7)
* [ ] 5.6 Azure Front Door (Global)
* [ ] 5.7 Private Endpoints & Private DNS
* [ ] 5.8 Azure Firewall & Egress Control
* [ ] 5.9 Hub-Spoke Architecture
* [ ] 5.10 DNS, Traffic & Global Routing
  * [ ] Azure DNS (custom domains, private DNS zones)
  * [ ] Traffic Manager (DNS-based routing)
  * [ ] Front Door vs Traffic Manager
  * [ ] Hybrid & multi-cloud routing

📌 **Case Studies**

* Public API + private DB
* DDoS mitigation strategy
* Multi-region traffic routing
* Hybrid on-prem connectivity
* Legacy DNS migration
* Multi-cloud routing decision
* DNS outage root cause

</details>
{% endstep %}

{% step %}
#### PART VI — API, Messaging & Event-Driven Architecture

{% hint style="info" %}
Async, Decoupled Systems | The Enterprise Integration Layer
{% endhint %}

<details>

<summary><strong>Chapter 6 – Designing Asynchronous Azure Systems</strong></summary>

* [ ] 6.1 Why APIs & Async Matter
* [ ] 6.2 **Azure API Management (APIM)** ⭐
* [ ] 6.3 API Gateway vs Reverse Proxy
* [ ] 6.4 APIM Policy Engine
* [ ] 6.5 Internal vs External APIs
* [ ] 6.6 Azure Service Bus
* [ ] 6.7 Event Grid
* [ ] 6.8 Event Hubs
* [ ] 6.9 Storage Queues
* [ ] 6.10 Logic Apps
* [ ] 6.11 Saga & Eventual Consistency
* [ ] 6.12 Message Poisoning & Retries

📌 **Case Studies**

* Public API monetization using APIM
* Internal microservice APIs
* Rate-limit failure incident
* APIM cost vs scale trade-off
* Order processing workflow
* Event-driven microservices
* Poison message failure
* Consumer scaling strategy

</details>
{% endstep %}

{% step %}
#### PART VII — Observability, Reliability & Failure

{% hint style="info" %}
Production Survival Skills
{% endhint %}

<details>

<summary><strong>Chapter 7 – Designing for Failure</strong></summary>

* [ ] 7.1 Reliability Engineering Basics
* [ ] 7.2 Azure Monitor
* [ ] 7.3 Application Insights
* [ ] 7.4 Log Analytics & KQL
* [ ] 7.5 Alerts & Action Groups
* [ ] 7.6 SLIs, SLOs & SLAs
* [ ] 7.7 Incident Response Design

📌 **Case Studies**

* Production outage analysis
* Missing telemetry failure
* Alert fatigue incident
* Chaos testing mindset

</details>
{% endstep %}

{% step %}
#### PART VIII — DevOps, IaC & Platform Automation

{% hint style="info" %}
Build Once, Deploy Forever
{% endhint %}

<details>

<summary><strong>Chapter 8 – CI/CD &#x26; Infrastructure as Code</strong></summary>

* [ ] 8.1 DevOps Architecture Principles
* [ ] 8.2 Azure DevOps vs GitHub Actions
* [ ] 8.3 ARM Templates
* [ ] 8.4 Bicep
* [ ] 8.5 Terraform on Azure
* [ ] 8.6 Blue-Green Deployments
* [ ] 8.7 Canary Releases
* [ ] 8.8 Rollback Without Data Loss
* [ ] 8.9 Azure Automation & Runbooks (Optional)
  * [ ] When automation is justified
  * [ ] Ops vs Dev responsibility
  * [ ] Legacy VM-heavy environments

📌 **Case Studies**

* Broken pipeline recovery
* Environment drift failure
* IaC rollback disaster
* Secure secret injection
* Nightly patch automation gone wrong

</details>
{% endstep %}

{% step %}
#### PART IX — Cost, Scale & FinOps

{% hint style="info" %}
Architects Own the Bill
{% endhint %}

<details>

<summary><strong>Chapter 9 – Cost-Aware Azure Architecture</strong></summary>

* [ ] 9.1 Azure Cost Management
* [ ] 9.2 Scaling vs Over-Provisioning
* [ ] 9.3 Reserved Instances
* [ ] 9.4 Spot VMs
* [ ] 9.5 Cosmos DB Cost Control
* [ ] 9.6 FinOps Operating Model

📌 **Case Studies**

* Startup cost explosion
* AKS over-engineering
* RU/s tuning success
* Cost accountability mod

</details>
{% endstep %}

{% step %}
#### PART X — Enterprise, SaaS & Reference Architectures

{% hint style="info" %}
Capstones
{% endhint %}

<details>

<summary><strong>Chapter 10 – Real-World Azure Reference Architectures</strong></summary>

* [ ] 10.1 Multi-Tenant SaaS Architecture
* [ ] 10.2 B2B Platform Architecture
* [ ] 10.3 Government-Grade (B2G) Systems
* [ ] 10.4 Legacy Monolith → Azure
* [ ] 10.5 High-Scale Event Platforms
* [ ] 10.6 Platform Evolution Roadmaps

📌 **Deliverables**

* End-to-end diagrams
* Trade-off analysis
* Interview walkthroughs

</details>

<details>

<summary><strong>Chapter 10A – AI &#x26; Intelligent Workloads (Architect View)</strong></summary>

* [ ] 10A.1 Azure ML – when architects care
* [ ] 10A.2 Azure Cognitive Services (Vision, Speech, NLP)
* [ ] 10A.3 AI workloads vs transactional systems
* [ ] 10A.4 Cost, security & data risks

📌 **Case Studies**

* AI feature added to SaaS safely
* Cognitive Services cost spike
* ML platform over-engineering

> _**Important:**_\
> This is **NOT an ML book**.\
> This is _“how architects safely enable AI without breaking systems.”_

</details>
{% endstep %}

{% step %}
### Appendices

* [ ] **Appendix A:** Azure Service Decision Matrix
* [ ] **Appendix B:** Interview Question Bank
* [ ] **Appendix C:** Architecture Review Checklist
* [ ] **Appendix D:** Common Azure Anti-Patterns
* [ ] **Appendix E:** Certification Mapping (AZ-104, AZ-204, AZ-305)
* [ ] **Appendix F:** Azure Service Scope Map (Core vs Specialist)
{% endstep %}
{% endstepper %}

***

<figure><img src=".gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

***

<figure><img src=".gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

[https://whimsical.com/azure-services-MAqY2TzMoU92HQQRwu9SXp](https://whimsical.com/azure-services-MAqY2TzMoU92HQQRwu9SXp)
