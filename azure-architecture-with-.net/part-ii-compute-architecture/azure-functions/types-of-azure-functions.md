---
description: By Trigger
icon: fulcrum
---

# Types of Azure Functions

{% hint style="info" %}
**Azure** **Functions** are categorized by **Triggers** (what starts them), not by “function types”.
{% endhint %}

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (73).png" alt="" width="468"><figcaption></figcaption></figure></div>

```sql
Event Source → Trigger → Function (.NET) → Bindings → External Services
```

If you understand **the event**, you know **which Function type to use**.

***

### 1️⃣ <mark style="color:$tint;">HTTP Trigger</mark> \[API & Webhook Functions]

#### What It Is

Triggered by an **HTTP request**.

#### When to Use

* REST APIs
* Webhooks
* Lightweight backend endpoints

#### Real-Time Use Cases

* Product CRUD API
* Payment gateway webhook
* Mobile app backend
* Admin APIs

#### Example

```bash
POST /api/orders
GET  /api/products
```

#### When NOT to Use

* Long-running operations
* Heavy synchronous processing

> **Rule:**\
> If a human or frontend calls it → HTTP Trigger.

***

### 2️⃣ <mark style="color:$tint;">Timer Trigger</mark> \[ Scheduled / Cron Jobs ]

#### What It Is

Triggered on a **schedule** (cron expression).

#### When to Use

* Nightly jobs
* Cleanup tasks
* Scheduled reports
* Data sync jobs

#### Real-Time Use Cases

* Clear expired sessions at midnight
* Generate daily sales report
* Sync data every 15 minutes

#### Example

```sql
0 0 0 * * *   → every day at midnight
```

> **Rule:**\
> If it runs “at a time” → Timer Trigger.

***

### 3️⃣ <mark style="color:$tint;">Queue Trigger</mark> (Azure Storage Queue / Service Bus)

#### (Background Processing)

#### What It Is

Triggered when a **message arrives in a queue**.

#### When to Use

* Async processing
* Decoupling systems
* Retrying failed work

#### Real-Time Use Cases

* Order placed → process payment
* Image uploaded → resize image
* Email sending
* Notification processing

#### Why Architects Love This

* Handles spikes gracefully
* Built-in retries
* Loose coupling

> **Rule:**\
> If work can happen later → Queue Trigger.

***

### 4️⃣ Blob Trigger \[ File-Driven Processing ]

#### What It Is

Triggered when a **file is uploaded or changed** in Blob Storage.

#### When to Use

* File processing
* Media handling
* Document ingestion

#### Real-Time Use Cases

* Upload invoice → extract data
* Upload image → compress/resize
* Upload CSV → import to DB

#### Example Flow

```sql
User uploads file → Blob Trigger → Process file
```

> **Rule:**\
> If a file appears → Blob Trigger.

***

### 5️⃣ <mark style="color:$tint;">Event Grid Trigger</mark> \[ System & Cloud Events ]

#### What It Is

Triggered by **Azure events** (resource changes, SaaS events).

#### When to Use

* Reacting to Azure resource events
* Large event-driven architectures
* Cross-service integration

#### Real-Time Use Cases

* VM created → tag automatically
* Blob created → notify system
* Subscription events

#### Why It’s Powerful

* Near real-time
* Push-based
* Highly scalable

> **Rule:**\
> If Azure emits the event → Event Grid Trigger.

***

### 6️⃣ <mark style="color:$tint;">Service Bus Trigger</mark> \[Enterprise Messaging ]

#### What It Is

Triggered by **Service Bus messages** (queues or topics).

#### When to Use

* Enterprise systems
* Guaranteed delivery
* Ordered processing

#### Real-Time Use Cases

* Financial transactions
* Order lifecycle events
* Multi-subscriber workflows

#### Difference from Storage Queue

| Storage Queue | Service Bus         |
| ------------- | ------------------- |
| Simple        | Enterprise-grade    |
| Cheap         | Feature-rich        |
| Basic retry   | Advanced retry, DLQ |

> **Rule:**\
> If reliability matters more than cost → Service Bus Trigger.

***

### 7️⃣ <mark style="color:$tint;">Cosmos DB Trigger</mark> \[ Data-Change Driven ]

#### What It Is

Triggered when **data changes** in Cosmos DB.

#### When to Use

* Event sourcing
* Change tracking
* Real-time projections

#### Real-Time Use Cases

* Order status changed → notify user
* New document → update search index
* Audit logging

> **Rule:**\
> If data change is the event → Cosmos DB Trigger.

***

### 8️⃣ <mark style="color:$tint;">Event Hub Trigger</mark> \[ Streaming & Telemetry ]

#### What It Is

Triggered by **high-volume event streams**.

#### When to Use

* Telemetry
* IoT
* Log ingestion
* Streaming analytics

#### Real-Time Use Cases

* IoT sensor data
* Application logs
* Clickstream analytics

> **Rule:**\
> If events come in millions/sec → Event Hub Trigger.

***

### Master Comparison Table (Cheat Sheet)

| Trigger Type | Best For             | Real Example     |
| ------------ | -------------------- | ---------------- |
| HTTP         | APIs & Webhooks      | Product CRUD     |
| Timer        | Scheduled jobs       | Nightly reports  |
| Queue        | Async work           | Order processing |
| Blob         | File handling        | Image resize     |
| Event Grid   | Azure events         | Auto-tag VM      |
| Service Bus  | Enterprise messaging | Payment workflow |
| Cosmos DB    | Data changes         | Real-time sync   |
| Event Hub    | Streaming            | IoT telemetry    |

***

### When to Use WHAT (Decision Shortcut)

```matlab
User calls API?           → HTTP Trigger
Runs on schedule?         → Timer Trigger
Work can be async?        → Queue Trigger
File uploaded?            → Blob Trigger
Azure resource event?     → Event Grid Trigger
Enterprise messaging?     → Service Bus Trigger
DB change driven?         → Cosmos DB Trigger
High-volume streaming?    → Event Hub Trigger
```

***

### Common Architect Mistakes (Avoid These)

❌ Using HTTP trigger for background jobs\
❌ Doing heavy processing synchronously\
❌ Ignoring queues for spikes\
❌ Treating Functions like App Service

***

{% hint style="warning" %}
“**Azure Functions** are categorized by **triggers**—HTTP for APIs, Timer for schedules, Queue and Service Bus for async processing, Blob for file events, and Event Grid or Event Hub for event-driven systems.”
{% endhint %}

***

### Architect’s Golden Insight

> **Choose the trigger based on the event, not on familiarity.**
