# Azure Application Insights – Practical Notes for C# Developers

## 1. What Application Insights Automatically Tracks (Out of the Box)

When you enable **Application Insights** for an **Azure Function** or **ASP.NET Core App**, it **automatically tracks**:

### 🔹 Request & Dependency Telemetry

* Incoming **HTTP requests**
* **Response time / latency**
* **Request success & failure rates**
* **HTTP status codes**
* **Dependencies**

  * SQL Database calls
  * HTTP calls
  * Azure Storage, Service Bus, etc.

### 🔹 Exception Telemetry

* **Unhandled exceptions**
* Stack traces
* Exception type & message
* Request context where the exception occurred

### 🔹 Performance Metrics

* Average / P95 / P99 response time
* Slowest requests
* Throughput (requests per second)
* CPU, memory (for App Service)

📌 **Use case:**
You can quickly identify **performance bottlenecks**, **slow APIs**, and **failing dependencies** without writing any code.

---
## 2. Custom Logging in Application Insights (C#)

In addition to auto-tracking, you can write **custom logs**.

### Using `ILogger` (Recommended)

```csharp
public class UsersController : ControllerBase
{
    private readonly ILogger<UsersController> _logger;

    public UsersController(ILogger<UsersController> logger)
    {
        _logger = logger;
    }

    [HttpGet("GetUsers")]
    public IActionResult GetUsers()
    {
        _logger.LogInformation("GetUsers API called");
        return Ok();
    }
}
```
These logs appear in **Application Insights → Logs / Transaction Search**.

---

## 3. Application Insights with Azure App Service (ASP.NET Core)

You can deploy an **ASP.NET Core Web App** to **Azure App Service** and enable Application Insights.

### Required Configuration Steps

### ✅ Step 1: Create ASP.NET Core App

Create a new ASP.NET Core Web API or MVC project in Visual Studio.
---

### ✅ Step 2: Install NuGet Package

```bash
Microsoft.ApplicationInsights.AspNetCore
```

---

### ✅ Step 3: Configure `appsettings.json`

```json
{
  "ApplicationInsights": {
    "ConnectionString": "InstrumentationKey=xxxx;IngestionEndpoint=xxxx"
  }
}
```

---

### ✅ Step 4: Register in `Program.cs`

```csharp
builder.Services.AddApplicationInsightsTelemetry();
```

---

### ✅ Step 5: Deploy to Azure App Service

Once deployed, telemetry automatically starts flowing into **Application Insights**.

---

## 4. Finding Requests & Logs (Transaction Search)

### 📍 Navigation

**Azure Portal → Application Insights → Transaction Search**

### Common Filters You’ll Use

#### 🔹 Search by Operation Id

* Used to trace **a single request across services**
* Helpful in **microservices** or **Function → API → DB** flows

#### 🔹 Filter by Event Type

Examples:

* `Request`
* `Dependency`
* `Exception`
* `Trace`

📌 Example:
To see only exceptions:

```
Event Type = Exception
```

---

#### 🔹 Filter by Operation Name

Used to track logs for a specific API or Function.

Example:

```
operation_Name = "GetUsers"
```

This shows:

* Request logs
* Exceptions
* Dependencies
* Custom traces

---

## 5. Performance Troubleshooting with Application Insights

### Key Views:

* **Performance → Operation Performance**
* **Failures → Exceptions**
* **Application Map**

You can answer:

* Which API is slow?
* Which dependency is failing?
* Which requests time out most?

---

## 6. Availability Tests (Uptime Monitoring)

Application Insights allows **synthetic monitoring**.

### Create Availability Test

1. **Application Insights → Availability**
2. Click **Add Standard Test**
3. Provide:

   * Test Name
   * URL to monitor
   * Frequency (5 or 10 minutes)
   * Test Locations

📌 This checks if your app is **up and responding**.

---

### Configure Alerts for Downtime

1. Go to **Monitor → Alerts → Alert Rules**
2. Select the **Availability Test**
3. Edit Rule
4. Add **Action Group**

   * Email
   * SMS
   * Webhook

📌 You’ll get notified when the app goes down.

---

## 7. Database Availability Monitoring (Custom Approach)

Application Insights **does not directly monitor DB availability**, but you can implement it using **Azure Functions**.

### Architecture

* Timer-triggered Azure Function (runs every 5 minutes)
* Tries DB connection
* Logs success/failure to App Insights
* Sends email alert on failure

---

### Steps to Implement

#### Step 1: Create Azure Function Project

* Select **Timer Trigger**
* Set schedule (CRON)

  ```
  */5 * * * *
  ```

---

#### Step 2: Database Connectivity Check

```csharp
public class DbHealthCheck
{
    private readonly ILogger _logger;

    public DbHealthCheck(ILoggerFactory loggerFactory)
    {
        _logger = loggerFactory.CreateLogger<DbHealthCheck>();
    }

    [FunctionName("DbHealthCheck")]
    public async Task Run([TimerTrigger("*/5 * * * *")] TimerInfo timer)
    {
        try
        {
            // Try DB connection
            _logger.LogInformation("Database connection successful");
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Database connection failed");
            throw;
        }
    }
}
```

---

### What You Gain

* DB failure visible in **Application Insights → Exceptions**
* Alerts can be triggered
* Root cause analysis using stack trace

---

## 8. Logging in Azure Functions (Important Update)

⚠️ **`TraceWriter` is deprecated**

### ✅ Use `ILogger` instead

```csharp
public static void Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation("Function executed");
}
```

---

## 9. Important Missing Concepts You Should Know (Very Important)

### 🔹 Application Map

* Visual representation of:

  * APIs
  * Azure Functions
  * Dependencies (DB, external APIs)

### 🔹 Dependency Tracking

* Automatically tracks:

  * SQL calls
  * HTTP calls
  * Azure services

### 🔹 Live Metrics

* Real-time request rate
* Failure count
* CPU / Memory

### 🔹 Sampling

* Reduces telemetry volume in high-traffic apps
* Prevents cost explosion

### 🔹 Kusto Query Language (KQL)

Used in **Logs** to analyze data:

```kql
requests
| where success == false
| order by timestamp desc


exceptions
| summarize count()


requests
| summarize avg(duration) by name

```

---

## 10. Real-World Usage Summary (For C# Developers)

✔ Monitor APIs & Azure Functions
✔ Trace slow requests
✔ Debug production issues
✔ Set alerts for downtime
✔ Track DB & dependency failures
✔ Centralized logging & diagnostics

---

If you want next, I can:

* Convert this into **interview-ready notes**
* Create a **production checklist**
* Add **real KQL queries**
* Show **App Insights vs Azure Monitor vs Log Analytics**

Just tell me 👍
