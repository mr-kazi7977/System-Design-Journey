# 📌 Microservices Architecture  

## 🚀 Introduction  

### **What are Microservices?**  
Microservices is an **architectural style** where an application is structured as a collection of **small, independent services** that communicate with each other via APIs.  

Each microservice is:  
✅ **Independent** – Can be developed, deployed, and scaled separately.  
✅ **Single Responsibility** – Focuses on a specific business function.  
✅ **Loosely Coupled** – Communicates with other services through APIs (usually REST or gRPC).  

📌 **Real-World Examples**:  
- **Netflix**: Uses microservices to handle user authentication, video streaming, recommendations, etc.  
- **Amazon**: Has independent services for orders, payments, inventory, and shipping.  
- **Uber**: Manages ride booking, driver allocation, pricing, and notifications as separate microservices.  

---

## 🔥 **Monolithic vs Microservices**  

| Feature | Monolithic Architecture | Microservices Architecture |
|---------|----------------------|----------------------|
| **Scalability** | Difficult to scale specific parts | Easily scalable |
| **Deployment** | Entire application redeployed | Deploy services independently |
| **Technology** | Single tech stack | Can use multiple technologies |
| **Fault Isolation** | A bug can crash the entire app | Failure in one service doesn’t affect others |
| **Speed** | Faster for small applications | Better for large-scale applications |

✅ **When to use Monolithic?** Small apps, startups, and fast MVPs.  
✅ **When to use Microservices?** Large, complex, and scalable applications.  

---

## 🔥 **Microservices Architecture Components**  

### 1️⃣ **API Gateway**  
- Acts as a **single entry point** for clients.  
- Handles **authentication, load balancing, caching, and request routing**.  
- Examples: **Kong, AWS API Gateway, Nginx, Spring Cloud Gateway**.  

### 2️⃣ **Service Discovery**  
- Maintains a **dynamic list of available services**.  
- Ensures microservices can find each other **without hardcoding URLs**.  
- Examples: **Eureka, Consul, Zookeeper**.  

### 3️⃣ **Inter-Service Communication**  
Microservices talk to each other using:  
✅ **REST APIs** – Simple, stateless HTTP-based communication.  
✅ **gRPC** – High-performance, protocol buffer-based RPC.  
✅ **Message Queues (Kafka, RabbitMQ, SQS)** – Used for async processing.  

### 4️⃣ **Database per Microservice**  
Each microservice **owns its own database** to maintain **data consistency and independence**.  
✅ Example: **User service → MySQL, Order service → PostgreSQL, Payment service → MongoDB**.  

### 5️⃣ **Event-Driven Architecture**  
Microservices can communicate asynchronously using **event-driven messaging**.  
✅ Examples: **Kafka, RabbitMQ, Apache Pulsar**.  

---

## 🔥 **Microservices Deployment Strategies**  

### 1️⃣ **Containerization (Docker)**  
- Packages microservices into **lightweight containers**.  
- Ensures **consistent deployment across environments**.  

📌 **Example: Running a Microservice in Docker**  
```sh
docker build -t my-microservice .
docker run -p 8080:8080 my-microservice
2️⃣ Serverless Microservices
Microservices run without managing servers.

Examples: AWS Lambda, Google Cloud Functions, Azure Functions.

📌 Example: AWS Lambda (Node.js Microservice)

js
Copy
Edit
exports.handler = async (event) => {
    return { statusCode: 200, body: "Hello from Microservice!" };
};
🔥 Challenges in Microservices
🚧 1. Service Communication Complexity
✅ Solution: Use API Gateway, Service Discovery (Eureka), and Message Queues.

🚧 2. Data Consistency
✅ Solution: Implement Event Sourcing and CQRS.

🚧 3. Distributed Logging & Monitoring
✅ Solution: Use ELK Stack, Prometheus, and Grafana.

🚧 4. Security
✅ Solution: Use OAuth2, JWT, API Gateway Security.

🛠️ Hands-On Practice
1️⃣ Build a Simple Microservice with Spring Boot
📌 Step 1: Create a Spring Boot Microservice

sh
Copy
Edit
spring init --name=user-service --dependencies=web,data-jpa,h2,lombok user-service
📌 Step 2: Implement REST API (UserController.java)

java
Copy
Edit
@RestController
@RequestMapping("/users")
public class UserController {
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = new User(id, "John Doe", "john@example.com");
        return ResponseEntity.ok(user);
    }
}
📌 Step 3: Run the Service

sh
Copy
Edit
mvn spring-boot:run
✅ Your first microservice is running at http://localhost:8080/users/1 🚀

🔥 Microservices Interview Questions
📌 Basic Questions
1️⃣ What are microservices, and how do they differ from monolithic architecture?
2️⃣ What are the advantages and disadvantages of microservices?
3️⃣ How do microservices communicate with each other?
4️⃣ What is an API Gateway? Why is it important?

📌 Advanced Questions
1️⃣ How would you handle data consistency across microservices?
2️⃣ How do you secure microservices in production?
3️⃣ What are event-driven microservices, and why are they useful?
4️⃣ What is circuit breaking in microservices?

📌 Hands-on Interview Challenge:
👉 "Design a microservices architecture for an e-commerce platform where users can browse products, add them to a cart, and make payments."

🎯 Conclusion
✅ Microservices enable scalability, flexibility, and independent deployments.
✅ Requires API Gateways, Service Discovery, and Message Queues for smooth communication.
✅ Tools like Docker, Kafka, RabbitMQ, and Spring Boot make microservices development easier.

🔥 Next Topic: Distributed Systems 🚀
