# 📌 Load Balancing  

## 🚀 Introduction  
Load balancing is the process of **distributing incoming network traffic** across multiple servers to ensure high availability, reliability, and performance.  

📌 **Why Load Balancing?**  
✅ Prevents a single server from being overloaded.  
✅ Improves application availability and fault tolerance.  
✅ Reduces response times by routing requests efficiently.  

---

## 🔥 How Load Balancers Work  

A **Load Balancer (LB)** sits between users and servers. It receives requests from clients and distributes them among available backend servers based on a chosen algorithm.  

📌 **Basic Flow:**  
1️⃣ Client sends a request to the application.  
2️⃣ The load balancer decides which server should handle it.  
3️⃣ The selected server processes the request and responds.  

---

## 🔥 Types of Load Balancers  

### 1️⃣ **Layer 4 (Transport Layer) Load Balancer**  
- Operates at the **TCP/UDP** level.  
- Routes traffic based on **IP address & port**.  
- Faster but lacks deep request inspection.  

📌 **Example:**  
- AWS Elastic Load Balancer (ELB) at **Layer 4**.  
- HAProxy operating at TCP mode.  

---

### 2️⃣ **Layer 7 (Application Layer) Load Balancer**  
- Operates at the **HTTP/HTTPS** level.  
- Can route requests based on **URL path, cookies, headers**.  
- More intelligent but requires extra processing power.  

📌 **Example:**  
- AWS ALB (Application Load Balancer).  
- Nginx, HAProxy configured for Layer 7.  

✅ **When to use Layer 4 vs Layer 7?**  

| Feature | Layer 4 | Layer 7 |
|---------|--------|--------|
| Speed | ⚡ Faster | 🛑 Slightly slower |
| Traffic Type | TCP/UDP | HTTP/HTTPS |
| Routing Rules | Based on IP, Port | Based on Headers, URL, Cookies |
| Example | HAProxy, AWS ELB | Nginx, AWS ALB |

---

## 🔥 Load Balancing Algorithms  

A load balancer decides **how to distribute traffic** among servers using different algorithms.  

### 1️⃣ **Round Robin**  
- Requests are assigned to servers in a **circular order**.  
- Simple but does not account for server health or load.  

📌 **Example (Nginx Configuration):**  
```nginx
upstream backend {
    server server1.example.com;
    server server2.example.com;
}
