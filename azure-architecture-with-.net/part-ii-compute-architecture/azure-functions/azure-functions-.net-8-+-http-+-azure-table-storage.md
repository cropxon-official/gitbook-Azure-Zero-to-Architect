---
icon: function
---

# Azure Functions (.NET 8) + HTTP + Azure Table Storage

### **Architecture Overview**

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure></div>

***

```javascript
Client (Postman / Swagger)
        ↓
HTTP Trigger (Azure Functions)
        ↓
Business Logic (.NET 8 Isolated)
        ↓
Azure Table Storage
        ↓
Application Insights (Logs & Metrics)

```

### Why Azure Functions for This CRUD?

#### We apply what we learned:

* **Low / burst traffic**
* **Pay per execution**
* **Auto scale**
* **Stateless design**

This CRUD API is:

* Admin-driven
* Not constant heavy traffic
* Cost-sensitive

➡ **Consumption plan is ideal**

***

<details>

<summary></summary>

#### Required Tools

Install once:

* **VS Code**
* **.NET SDK 8**
* **Azure Functions Core Tools**
* **Azure CLI**

#### VS Code Extensions (IMPORTANT)

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (69).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (70).png" alt="" width="461"><figcaption></figcaption></figure></div>

<div align="left" data-with-frame="true"><figure><img src="../../../.gitbook/assets/image (71).png" alt="" width="512"><figcaption></figcaption></figure></div>

***

**Install**:

* Azure Functions
* Azure Storage
* Azure CLI Tools
* REST Client (optional)

</details>

<details>

<summary><mark style="color:$tint;"><strong>STEP 2 — Create Azure Functions Project (.NET 8)</strong></mark></summary>

```bash
func init ProductApi --worker-runtime dotnetIsolated --target-framework net8.0
cd ProductApi

```

**Create HTTP function:**

```bash
func new --name ProductsFunction --template "HTTP trigger"
```

</details>

<details>

<summary><mark style="color:$tint;"><strong>STEP 3 — Define Domain Model (Product)</strong></mark></summary>

```csharp
public class ProductEntity : ITableEntity
{
    public string PartitionKey { get; set; } = "PRODUCT";
    public string RowKey { get; set; } = Guid.NewGuid().ToString();
    public string Name { get; set; }
    public decimal Price { get; set; }
    public DateTimeOffset? Timestamp { get; set; }
    public ETag ETag { get; set; }
}

```

#### Why Table Storage?

* Cheap
* Fast
* Serverless-friendly
* No schema friction

</details>

<details>

<summary><mark style="color:$tint;"><strong>STEP 4 — Azure Table Storage Setup (Local)</strong></mark></summary>

Use **Azurite** (local emulator):

```bash
azurite
```

Add connection string in <mark style="color:$tint;">`local.settings.json`</mark><mark style="color:$tint;">:</mark>

```json
{
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated"
  }
```

</details>

<details>

<summary><mark style="color:$tint;"><strong>STEP 5 — Implement CRUD Functions</strong></mark></summary>

### GET /products

```csharp
[Function("GetProducts")]
public async Task<HttpResponseData> GetProducts(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = "products")] HttpRequestData req,
    [TableInput("Products", Connection = "AzureWebJobsStorage")] TableClient tableClient)
{
    var products = tableClient.Query<ProductEntity>().ToList();
    var response = req.CreateResponse(HttpStatusCode.OK);
    await response.WriteAsJsonAsync(products);
    return response;
}
```

***

### GET /products/{id}

```csharp
[Function("GetProductById")]
public async Task<HttpResponseData> GetProductById(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = "products/{id}")] HttpRequestData req,
    string id,
    [TableInput("Products", Connection = "AzureWebJobsStorage")] TableClient tableClient)
{
    var entity = await tableClient.GetEntityAsync<ProductEntity>("PRODUCT", id);
    var response = req.CreateResponse(HttpStatusCode.OK);
    await response.WriteAsJsonAsync(entity.Value);
    return response;
}
```

***

### POST /products

```csharp
[Function("CreateProduct")]
public async Task<HttpResponseData> CreateProduct(
    [HttpTrigger(AuthorizationLevel.Function, "post", Route = "products")] HttpRequestData req,
    [TableInput("Products", Connection = "AzureWebJobsStorage")] TableClient tableClient)
{
    var product = await req.ReadFromJsonAsync<ProductEntity>();
    await tableClient.AddEntityAsync(product);
    var response = req.CreateResponse(HttpStatusCode.Created);
    await response.WriteAsJsonAsync(product);
    return response;
```

</details>

<details>

<summary><mark style="color:$tint;"><strong>STEP 6 — Run Locally &#x26; Test</strong></mark></summary>

```bash
func start
```

_**Endpoints**_:

```
http://localhost:7071/api/products
```

</details>
