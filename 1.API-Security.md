# A. Basic Security
- HTTPS everywhere (TLS 1.2+)  **-(Transport & Communication)**
- Authentication API Keys in Header HTTP Header: Authorization: ApiKey <key> **(Authentication)**
- Enforce role-based access (RBAC) **(Authorization)**
- Input Validation Headers,Query params, Body payloads

--- 
# B. Intermediate Security (Production-Ready APIs)
### **1. OAuth 2.0**
  OAuth 2.0 is an open standard for authorization that allows applications to access resources on behalf of a user without exposing the user's credentials
**Common OAuth 2.0 Providers:**
- Google – Sign in with Google, access Gmail, Google Drive, Calendar.
- Microsoft / Azure AD – Access Microsoft 365 resources, Outlook, OneDrive.
- Facebook – Login via Facebook, access basic profile and friends list.
- GitHub – Login via GitHub, access repos, organizations.
- LinkedIn – Login via LinkedIn, access profile, connections, posts.
- Twitter / X – Login via Twitter, access tweets, followers.

### 2.** Rate Limiting & Throttling**
- Rate Limiting: Controls the number of requests a client can make in a given time window. Example: max 100 requests per minute per IP.
- Throttling: Controls how requests are processed once limits are reached. Requests can be delayed, queued, or rejected.

###  **3. CORS Protection** 
 Allow only trusted origins - Avoid: Access-Control-Allow-Origin: *

### **4. Idempotency Keys**
Idempotency is a concept in APIs (and computing in general) that ensures performing the same operation multiple times has the same effect as doing it once. It prevents unintended consequences when a client retries a request due to network issues, timeouts, or server errors.

**Key Idea**
- For idempotency keys, the client is usually responsible for generating them, and the server enforces uniqueness and stores the result. **string idempotencyKey = Guid.NewGuid().ToString()**
- A request is idempotent if repeating it doesn’t change the result.
- Important for critical operations like payments, order creation, or inventory updates.
- Mostly used in post request as calling same order may create duplicate order
```
  POST /orders
Idempotency-Key: 12345
{
  "userId": 1,
  "itemId": 100,
  "quantity": 1
}
```
- Does this key exist in its database?- Yes: Return the previous response. -No: Process the request, store the key + response.

### 5. Logging & Monitoring
Log:
- Authentication failures
- Authorization violations
- Rate-limit breaches
- Mask sensitive data (PII, tokens)

# Advanced Security (Enterprise / Financial-Grade)

### 🔐 Token Security

* Use **short-lived JWTs**
* Store secrets in:
  * Azure Key Vault
  * AWS Secrets Manager
* Rotate secrets automatically
* 
### 🔏 Mutual TLS (mTLS)

* Both client and server authenticate each other
* Ideal for:
  * B2B integrations
  * Internal microservices

### 🧠 Behavioral Protection

* Detect:
  * Bot attacks
  * Scraping
  * Credential stuffing
* Use:
  * API Gateways
  * WAF (Web Application Firewall)

### 🛡 OWASP API Top 10 Protection

Protect against:
* Broken Object Level Authorization (BOLA)
* Mass Assignment
* Excessive Data Exposure
* Injection attacks
* SSRF
### 📜 Schema Enforcement

* Enforce OpenAPI schema validation
* Reject non-conforming requests automatically

---

## 4️⃣ API Gateway & Infrastructure Security

### 🚪 API Gateway (Highly Recommended)

Examples:

* Azure API Management
* AWS API Gateway
* Kong / Apigee

Capabilities:

* Auth enforcement
* Rate limiting
* IP filtering
* Request validation
* API versioning

### 🧱 Network Security

* IP allow-listing (where applicable)
* Private endpoints for internal APIs
* Zero Trust networking


