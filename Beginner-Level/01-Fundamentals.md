# 📌 Fundamentals of System Design  

## 🚀 Introduction  
System design is the process of **defining the architecture, components, modules, interfaces, and data flow** of a system. It helps in creating **scalable, high-performance, and reliable** systems.  

---

## 🔥 Key Concepts  

### 1️⃣ **Scalability**  
The ability of a system to handle increased load efficiently.  
✅ **Vertical Scaling (Scaling Up)** - Increasing system resources (CPU, RAM, Disk).  
✅ **Horizontal Scaling (Scaling Out)** - Adding more servers/machines.  

### 2️⃣ **Availability**  
The percentage of time a system is operational. Measured as:  
**Availability (%) = (Uptime / Total Time) × 100**  
📌 **Example:**  
- **99% uptime** → ~3.65 days of downtime per year.  
- **99.999% uptime (Five Nines)** → ~5.26 minutes of downtime per year.  

### 3️⃣ **Latency vs Throughput**  
- **Latency** ⏳ → Time taken for a request to get a response. (Lower is better)  
- **Throughput** 🚀 → Number of requests handled per second. (Higher is better)  

### 4️⃣ **CAP Theorem**  
A distributed system can guarantee **at most 2** of the following 3:  
1. **Consistency (C)** → Every read gets the latest write.  
2. **Availability (A)** → Every request gets a response, even if stale.  
3. **Partition Tolerance (P)** → System continues working despite network failures.  

📌 **Example:**  
- **CP System** → MongoDB (Consistency + Partition Tolerance).  
- **AP System** → Cassandra, DynamoDB (Availability + Partition Tolerance).  

### 5️⃣ **Load Balancing**  
Distributes incoming requests across multiple servers to improve performance.  
✅ Types:  
- **Round Robin** 🌀 → Simple rotation of requests.  
- **Least Connections** 🔗 → Sends request to the least busy server.  
- **Geo-based** 🌍 → Routes traffic based on user location.  

### 6️⃣ **Database Types**  
1. **SQL (Relational DB)** → Structured data with strong consistency (MySQL, PostgreSQL).  
2. **NoSQL (Non-Relational DB)** → Flexible schema, used for high scalability (MongoDB, DynamoDB, Cassandra).  

📌 **Comparison:**  
| Feature     | SQL (RDBMS) | NoSQL (MongoDB, Cassandra) |
|------------|------------|------------------|
| Schema     | Fixed      | Flexible |
| Scaling    | Vertical   | Horizontal |
| Transactions | ACID      | BASE |

### 7️⃣ **Caching**  
Stores frequently accessed data in **memory** (Redis, Memcached) to reduce latency.  
✅ **Cache Strategies:**  
- **Write-Through** → Data written to cache & DB simultaneously.  
- **Write-Back** → Data written to cache first, then DB later.  
- **Least Recently Used (LRU)** → Evicts least-used items first.  

---

## 🛠️ **Basic System Design Process**  

1️⃣ **Clarify Requirements** → Understand system constraints, scalability needs, and use cases.  
2️⃣ **Define High-Level Architecture** → Decide on monolithic or microservices architecture.  
3️⃣ **Choose the Right Database** → SQL vs NoSQL based on use case.  
4️⃣ **Implement Caching** → Reduce DB calls using Redis or Memcached.  
5️⃣ **Ensure Scalability** → Use **load balancers**, **database sharding**, and **horizontal scaling**.  
6️⃣ **Consider Security** → Use **OAuth, JWT, HTTPS, and data encryption**.  

---

## 📌 **Example: Designing a URL Shortener**  
**Requirements:**  
✅ Users enter long URLs → System generates short URLs.  
✅ Short URLs should be unique and redirect users to the original link.  
✅ Should handle millions of requests per second.  

📌 **System Components:**  
1. **Frontend:** Simple UI for input/output.  
2. **Backend:** REST API to generate and store short URLs.  
3. **Database:** NoSQL (e.g., Redis for fast lookups).  
4. **Load Balancer:** Distributes traffic among multiple servers.  
5. **Caching:** Store popular URLs in memory to reduce DB queries.  

---

## 📚 Recommended Resources  
📌 **Books:**  
- **Designing Data-Intensive Applications** – Martin Kleppmann  
- **System Design Interview** – Alex Xu  

📌 **Videos & Courses:**  
- [Grokking the System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview)  
- [System Design Primer GitHub](https://github.com/donnemartin/system-design-primer)  

---

## 🎯 **Next Steps**  
📌 **Move to the next topic: [Database Design](./02-Database-Design.md)**  
