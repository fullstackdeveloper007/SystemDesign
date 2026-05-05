# How do Microservices differ from Monolithic Architecture ?
- Architecture
- Scalability
- Technology Stack
- Deployment
- Use Case


# Key Feature of Microservices
- Independednt Deployment
- Fault Isolation
- Scalability

# Main component of Microsrvices

- Service Independence
- Domain Driven Design
- API Commpunication
- Technology Diversity

# Service Registery and Discovery: Cenralized system to discover each other 
- API Gateway
- Load Balancer
- Data management
- Monitoring and logging
- CI/CD pipeline

Here’s a clean, interview-ready flow with brief bullets:

---

# 🔹 End-to-End Request Flow

### 1. Client Request
* Request comes from **web / mobile app**
* Hits a single entry point → **API Gateway**

## 🔹 2. API Gateway Responsibilities

The API Gateway (e.g., NGINX, Azure API Management) handles:

### a) IP Filtering
* Allow list / Block list of IPs
* Prevent unauthorized network access
* 
### b) Authentication & Authorization
* Validates identity (JWT, OAuth, API keys)
* Checks permissions (who can access what)
### c) Rate Limiting / Throttling
* Limits number of requests per user/IP
* Protects backend from overload / abuse
### d) Request Routing
* Maps request path → target microservice
  ```
  /api/users → user-service
  ```
## 🔹 3. Service Discovery Interaction (Important Point)
* ❗ Service discovery is **NOT part of API Gateway**
* ✔ It is a **separate component**
### Flow:
* API Gateway asks discovery system (e.g., Consul, Apache ZooKeeper, Kubernetes):
  * “Where is `user-service` running?”
* Discovery returns:
  * Available instances (IP + port)
## 🔹 4. Internal Routing
* API Gateway:
  * Selects one instance (load balancing)
  * Forwards request internally
```
Client → API Gateway → user-service (via discovered IP)
```
## 🔹 5. Response Flow
* Microservice processes request
* Sends response → API Gateway
* Gateway returns response → Client---

## 🔚 Final Summary (One-liner)

* API Gateway = **Security + Routing + Control layer**
* Service Discovery = **Dynamic service location provider**
* Gateway **uses** discovery, but does not own it

- <img width="761" height="575" alt="image" src="https://github.com/user-attachments/assets/fa3a0437-912d-4a2f-92dc-655af9f92c49" />

# Api GateWay

<img width="581" height="675" alt="image" src="https://github.com/user-attachments/assets/01e7007a-a903-43e4-81ff-561d59aeafb4" />
