https://www.youtube.com/watch?v=wmawYODmQU0

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

# Core Characteristics

- Service Independence
- Domain Driven Design
- API Commpunication
- Technology Diversity

# Main component of Microsrvices 
- Service Registery and Discovery: Cenralized system to discover each other. AKS(Azure Kubernates Service),Eureka,Consul (for VM-based deployments)
- API Gateway : It acts as a single entry point for client,Routing request to appropriate services and handle Authentication, Rate limiting,Logging
- Load Balancer : Distribute incoming traffic across the instances of a service to ensure high availability
- Data management : EAch service manage its own database
- Monitoring and logging : Grafana and RealkStack,Azure Monitor,
- CI/CD pipeline

# 🔹 End-to-End Request Flow

### 1. Client Request
* Request comes from **web / mobile app**
* Hits a single entry point → **API Gateway**

## 🔹 2. API Gateway Responsibilities

The API Gateway (e.g., NGINX, Azure API Management) serves as centrlized entry point for managing client intaeraytions with microservices. It simplifies communication by routing incoming request to appropriate microservices and handle various operational responsibility. 
<img width="761" height="412" alt="image" src="https://github.com/user-attachments/assets/a86dd398-4787-4bcd-b7e2-86650a206120" />


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
