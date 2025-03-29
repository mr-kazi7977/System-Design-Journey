# 📌 Distributed Systems & Event-Driven Design  

## 🚀 Introduction  

A **Distributed System** is a system where multiple computers (nodes) work together to appear as a **single system** to users.  

📌 **Why Distributed Systems?**  
✅ Scalability – Handle large traffic loads efficiently.  
✅ Fault Tolerance – No single point of failure.  
✅ High Availability – Ensure minimal downtime.  
✅ Performance – Faster processing by distributing tasks.  

📌 **Examples of Distributed Systems:**  
- Google Search Engine  
- Amazon Web Services (AWS)  
- Netflix Streaming Service  
- Banking & Payment Systems  

---

## 🔥 **Key Concepts in Distributed Systems**  

### 1️⃣ **CAP Theorem**  
The CAP theorem states that a distributed system **can only guarantee two out of three** properties:  

| Property | Explanation |
|----------|------------|
| **C - Consistency** | All nodes see the same data at the same time. |
| **A - Availability** | Every request gets a response (even if stale). |
| **P - Partition Tolerance** | The system works despite network failures. |

📌 **Examples:**  
✅ **CP (Consistency + Partition Tolerance):** MongoDB (strong consistency, may sacrifice availability).  
✅ **AP (Availability + Partition Tolerance):** Cassandra, DynamoDB (eventual consistency).  
✅ **CA (Consistency + Availability):** Not possible in distributed systems!  

---

### 2️⃣ **Horizontal vs Vertical Scaling**  

| Scaling Type | Description | Example |
|-------------|-------------|---------|
| **Vertical Scaling (Scale-Up)** | Adding more power (CPU, RAM) to a single machine. | Upgrading a server from 16GB RAM to 64GB. |
| **Horizontal Scaling (Scale-Out)** | Adding more machines (servers) to distribute the load. | Adding 10 servers to handle more requests. |

✅ **Which is better?**  
- **Vertical Scaling** is easier but has hardware limits.  
- **Horizontal Scaling** is more flexible and preferred for distributed systems.  

---

### 3️⃣ **Consistency Models**  

| Consistency Model | Description | Example |
|------------------|-------------|---------|
| **Strong Consistency** | Every read gets the latest write. | Google Spanner |
| **Eventual Consistency** | Data is eventually consistent after a delay. | DynamoDB, Cassandra |
| **Read-Your-Own-Writes** | Users see their own latest updates. | Social Media Feeds |
| **Causal Consistency** | Operations follow a cause-effect order. | Version Control Systems (Git) |

📌 **Tradeoff:** Strong consistency is slow but reliable. Eventual consistency is fast but may return stale data.  

---

## 🔥 **Event-Driven Architecture (EDA)**  

### **What is Event-Driven Architecture?**  
An **Event-Driven Architecture (EDA)** is a design where microservices communicate using **events** instead of direct API calls.  

📌 **Example:**  
✅ A user places an order → **OrderCreated Event** is published → **Payment Service** listens and processes payment.  

---

### **Components of Event-Driven Architecture**  

1️⃣ **Event Producer** – The service that generates an event. (e.g., Order Service)  
2️⃣ **Event Broker** – Middleware that handles event routing. (e.g., Kafka, RabbitMQ)  
3️⃣ **Event Consumer** – Services that react to the event. (e.g., Payment Service)  

📌 **Example Flow in an E-Commerce App:**  
User clicks 'Buy Now' → Order Service publishes an event →
Payment Service processes payment → Inventory Service updates stock →
Notification Service sends an email to the user.

markdown
Copy
Edit

✅ **Advantages of Event-Driven Architecture:**  
- Decouples services → Microservices don’t depend on each other.  
- Improves **scalability and flexibility**.  
- Supports **real-time data processing**.  

---

## 🔥 **Message Queues & Event Brokers**  

Distributed systems often use **message queues** or **event brokers** for communication.  

| Tool | Type | Use Case |
|------|------|----------|
| **Kafka** | Event Streaming | High-throughput event processing |
| **RabbitMQ** | Message Queue | Decoupling microservices |
| **SQS (AWS)** | Managed Queue | Serverless applications |
| **Pulsar** | Event Streaming | Cloud-native event handling |

📌 **Example: Kafka-Based Event Processing**  

```java
// Producer (Order Service)
@Autowired
private KafkaTemplate<String, String> kafkaTemplate;

public void createOrder(Order order) {
    kafkaTemplate.send("order-events", order.toString());
}
java
Copy
Edit
// Consumer (Payment Service)
@KafkaListener(topics = "order-events", groupId = "payment-service")
public void processPayment(String orderEvent) {
    System.out.println("Processing payment for: " + orderEvent);
}
✅ Kafka ensures message durability and fault tolerance.

🔥 Challenges in Distributed Systems
🚧 1. Network Failures
✅ Solution: Implement Retry Mechanisms, Circuit Breakers (Resilience4j).

🚧 2. Data Consistency
✅ Solution: Use Event Sourcing, CQRS, Two-Phase Commit (2PC).

🚧 3. Monitoring & Debugging
✅ Solution: Use Distributed Logging (ELK), Tracing (Jaeger, OpenTelemetry).

🚧 4. Latency Issues
✅ Solution: Use CDNs, Caching (Redis), Load Balancing.

🛠️ Hands-On Practice
1️⃣ Build an Event-Driven System Using Kafka
📌 Step 1: Start Kafka using Docker

sh
Copy
Edit
docker-compose up -d
📌 Step 2: Publish an Event from Order Service

java
Copy
Edit
kafkaTemplate.send("orders", "New Order Placed: ID 12345");
📌 Step 3: Consume the Event in Payment Service

java
Copy
Edit
@KafkaListener(topics = "orders", groupId = "payments")
public void processOrder(String order) {
    System.out.println("Processing payment for order: " + order);
}
```
✅ Your microservices are now decoupled and event-driven! 🚀

🔥 Distributed Systems Interview Questions
📌 Basic Questions
1️⃣ What is a distributed system, and why do we use it?
2️⃣ Explain CAP theorem with examples.
3️⃣ What is the difference between vertical and horizontal scaling?
4️⃣ What are strong vs eventual consistency?

📌 Advanced Questions
1️⃣ How does Kafka ensure fault tolerance in event-driven systems?
2️⃣ What is eventual consistency, and how can you implement it?
3️⃣ How do you handle network failures in distributed systems?
4️⃣ Explain the difference between message queues and event streaming.

📌 Hands-on Interview Challenge:
👉 "Design an event-driven architecture for a ride-hailing app like Uber where events include RideRequested, DriverAssigned, and RideCompleted."

🎯 Conclusion
✅ Distributed systems enable scalability, fault tolerance, and availability.
✅ Event-driven design decouples services and makes systems more flexible.
✅ Tools like Kafka, RabbitMQ, and Redis are essential for distributed applications.

🔥 Next Topic: Netflix Case Study 🚀
