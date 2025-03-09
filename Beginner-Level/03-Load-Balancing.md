# 📌 Load Balancing  

## 🚀 Introduction  

### **What is Load Balancing?**  
Load balancing is a technique used to **distribute incoming network traffic** across multiple backend servers to prevent overload, ensure high availability, and optimize response times.  

### 📌 **Why is Load Balancing Important?**  
✅ **High Availability** – Ensures application uptime even if some servers fail.  
✅ **Scalability** – Handles increasing user traffic by distributing requests efficiently.  
✅ **Performance Optimization** – Reduces latency and improves response times.  
✅ **Fault Tolerance** – If a server crashes, the load balancer redirects traffic to healthy servers.  

### 🔥 **Real-World Examples**  
- **Google Search**: Uses global load balancing to serve billions of queries daily.  
- **Netflix**: Routes video streaming requests dynamically to different data centers worldwide.  
- **E-commerce Sites (Amazon, Flipkart)**: Prevents downtime during sales events by balancing millions of requests across multiple servers.  

---

## 🔥 **How Load Balancers Work**  

A **Load Balancer (LB)** sits between the client and backend servers. It decides **which server should handle a request** based on various factors like **server health, response time, or request type**.  

### 📌 **Basic Flow**  
1️⃣ **Client Request** – A user sends a request to the application (e.g., visiting `amazon.com`).  
2️⃣ **Load Balancer Routing** – The LB picks a healthy backend server.  
3️⃣ **Server Response** – The selected server processes the request and returns the response to the client.  

### 📌 **Diagram Representation**  

```plaintext
  [Client]  --->  [Load Balancer]  --->  [Server 1]
                                    --->  [Server 2]
                                    --->  [Server 3]
```

🔥 Types of Load Balancers
1️⃣ Layer 4 (Transport Layer) Load Balancer
Operates at the TCP/UDP level.
Routes traffic based on IP address & port.
Faster but lacks deep request inspection.
📌 Example:

AWS Elastic Load Balancer (ELB) at Layer 4.
HAProxy operating in TCP mode.
2️⃣ Layer 7 (Application Layer) Load Balancer
Operates at the HTTP/HTTPS level.
Can route requests based on URL path, cookies, headers.
More intelligent but requires extra processing power.
📌 Example:

AWS ALB (Application Load Balancer).
Nginx, HAProxy configured for Layer 7.
✅ Comparison: Layer 4 vs. Layer 7

Feature	Layer 4 (Transport)	Layer 7 (Application)
Speed	⚡ Faster	🛑 Slightly slower
Traffic Type	TCP/UDP	HTTP/HTTPS
Routing Rules	Based on IP, Port	Based on Headers, URL, Cookies
Example	HAProxy, AWS ELB	Nginx, AWS ALB
🔥 Load Balancing Algorithms
A load balancer decides how to distribute traffic among servers using different algorithms.

1️⃣ Round Robin
Requests are assigned to servers in a circular order.
Simple but does not account for server health or load.
📌 Example (Nginx Configuration):

nginx
Copy
Edit
upstream backend {
    server server1.example.com;
    server server2.example.com;
}
server {
    listen 80;
    location / {
        proxy_pass http://backend;
    }
}
2️⃣ Least Connections
Routes traffic to the server with the fewest active connections.
Helps when some requests take longer to process.
📌 Example (HAProxy Configuration):

plaintext
Copy
Edit
backend servers
    balance leastconn
    server server1 192.168.1.10:80 check
    server server2 192.168.1.11:80 check
3️⃣ IP Hashing
Uses client’s IP address to determine which server handles the request.
Ensures session persistence (sticky sessions).
4️⃣ Weighted Round Robin
Assigns higher weight to more powerful servers.
Useful when servers have different capabilities.
📌 Example:
If Server A has 2x the CPU power of Server B, we can assign weights:

Server A: Weight = 2
Server B: Weight = 1
nginx
Copy
Edit
upstream backend {
    server server1.example.com weight=2;
    server server2.example.com weight=1;
}
🔥 Load Balancer Architectures
1️⃣ Global Load Balancer (Geo-Based Load Balancing)
Routes users to the nearest data center based on geography.
Used by Google, Netflix, AWS Route 53.
2️⃣ CDN (Content Delivery Network) Load Balancing
Offloads traffic to edge servers close to users.
Examples: Cloudflare, Akamai, AWS CloudFront.
3️⃣ DNS Load Balancing
Uses DNS resolution to distribute traffic.
Example: Google DNS Load Balancer.
🔥 Hands-On Practice
1️⃣ Setting Up a Load Balancer with Nginx
Step 1: Install Nginx
sh
Copy
Edit
sudo apt update
sudo apt install nginx -y
Step 2: Configure Nginx as a Load Balancer
Edit /etc/nginx/nginx.conf and add:

nginx
Copy
Edit
http {
    upstream backend_servers {
        server 192.168.1.10;
        server 192.168.1.11;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://backend_servers;
        }
    }
}
Step 3: Restart Nginx
sh
Copy
Edit
sudo systemctl restart nginx
✅ Done! Your load balancer is ready!

🔥 Interview Questions on Load Balancing
📌 Basic Questions
1️⃣ What is load balancing, and why is it important?
2️⃣ What is the difference between Layer 4 and Layer 7 load balancing?
3️⃣ What are the most common load balancing algorithms?

📌 Advanced Questions
1️⃣ How would you design a load balancing solution for a real-time chat application?
2️⃣ How do CDNs improve website performance with load balancing?
3️⃣ What are the trade-offs between DNS load balancing and an application-level load balancer?

🎯 Conclusion
✅ Load balancing is critical for high availability and scalability.
✅ There are different types of load balancers (Layer 4 vs. Layer 7).
✅ Several algorithms help distribute traffic efficiently.
✅ Practical hands-on experience with Nginx and HAProxy is essential.

🔥 Next Topic: [Microservices Architecture](./Intermediate-Level/04-Microservices.md) 🚀
