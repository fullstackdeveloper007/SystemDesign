https://www.youtube.com/watch?v=wmawYODmQU0

### How do Microservices differ from Monolithic Architecture ?
- Architecture
- Scalability
- Technology Stack
- Deployment
- Use Case
### Key Feature of Microservices
- Independednt Deployment
- Fault Isolation
- Scalability

### Core Characteristics
- Service Independence
- Domain Driven Design
- API Commpunication
- Technology Diversity
### Main component of Microsrvices 
- Service Registery and Discovery: Cenralized system to discover each other. AKS(Azure Kubernates Service),Eureka,Consul (for VM-based deployments)
- API Gateway : It acts as a single entry point for client,Routing request to appropriate services and handle Authentication, Rate limiting,Logging
- Load Balancer : Distribute incoming traffic across the instances of a service to ensure high availability
- Data management : EAch service manage its own database
- Monitoring and logging : Grafana and RealkStack,Azure Monitor,
- CI/CD pipeline

## 🔹 End-to-End Request Flow

**1. Client Request**
* Request comes from **web / mobile app**
* Hits a single entry point → **API Gateway**

**2. API Gateway Responsibilities**

The API Gateway (e.g., NGINX, Azure API Management) serves as centrlized entry point for managing client intaeraytions with microservices. It simplifies communication by routing incoming request to appropriate microservices and handle various operational responsibility. 
<img width="761" height="412" alt="image" src="https://github.com/user-attachments/assets/a86dd398-4787-4bcd-b7e2-86650a206120" />
- **a) IP Filtering**
* Allow list / Block list of IPs
* Prevent unauthorized network access
  
- **b) Authentication & Authorization**
* Validates identity (JWT, OAuth, API keys)
* Checks permissions (who can access what)

- **c) Rate Limiting / Throttling**
* Limits number of requests per user/IP
* Protects backend from overload / abuse

- **d) Request Routing**
* Maps request path → target microservice
  ```
  /api/users → user-service
  ```
### 3. Service Discovery Interaction (Important Point)
* ❗ Service discovery is **NOT part of API Gateway**
* ✔ It is a **separate component**
**Flow:**
* API Gateway asks discovery system (e.g., Consul, Apache ZooKeeper, Kubernetes):
  * “Where is `user-service` running?”
* Discovery returns:
  * Available instances (IP + port)
### 4. Internal Routing
* API Gateway:
  * Selects one instance (load balancing)
  * Forwards request internally
```
Client → API Gateway → user-service (via discovered IP)
```
### 5. Response Flow
* Microservice processes request
* Sends response → API Gateway
* Gateway returns response → Client---

### 🔚 Final Summary (One-liner)

* API Gateway = **Security + Routing + Control layer**
* Service Discovery = **Dynamic service location provider**
* Gateway **uses** discovery, but does not own it

 ### How do Microservice Communicate with each other ?
 - Rest (Synhchronus Communiation )
 - Message Queue (Async)
\

### Q. How to handle Microservice Failure ?
**- Circuit Breaker** : It is basically a design pattern that helps prevent cascading failures by stopping the flow of requests to a service that is likely to fail so if a microservice detects that aparticulatr servoce is not responsding so it will not call the service 
**- Retry and Call back :** Useful for scenario when failure is due to Temporary issues such as Network congestion or brief outages .
**- Centralized Logging:**

### Q. Compare Load Balancer and API Gateway

### Q. What are common Microservices design principles ?
- **High Cohesion** : High cohesion basically refers to the design of each microservices to be focused around a single well-defined responsibility or a business capability a cohesive service should be limited for a specific task or a set of related tasks with minimal dependencies on other services
- **Autonomous :**  it means they should operate independently without depending on one another so each microservice should have its own Data Business logic and the interface so this principle emphasizes loose coupling between services so that one service failure does not affect the other services
**- Business Domain Centric:** microservices should be aligned with business domains or capabilities rather than technical layers or function so each service should correspond to a specific business function or a capability making the architecture more intuitive and closely
**- Resilience :**  basically microservices should be resilience to failure and designed to gracefully handle disruptions or faults so this includes strategies like redundancy circuit breakers retries and fallbacks to ensure that a failure in one service does not lead to a systemwide downtime. 
**- Observable :** Ability to Monitor and track the health and performance of microservices in real time so this includes logging metrics and tracing to capture the insights into how services are performing and interacting with one another
**- Automation :** In microservices design and this refers to the use of automated processes for deployment testing scaling and management of microservices so automated pipelines can
ensure that microservices can be deployed quickly and reliably.

### Q. how to ensure Security in Microservices:
- Secure Communication : securing communication refers to encrypting the data exchange between microservices to prevent eavesdropping manin the-middle attacks and unauthorized data access. so this is done by using the TLs or the transort layer security or the SSL which is the secure socket layer protocols.
- Authentication and Authorization
- Service Level isolation 
  


  
 

<img width="581" height="675" alt="image" src="https://github.com/user-attachments/assets/01e7007a-a903-43e4-81ff-561d59aeafb4" />
