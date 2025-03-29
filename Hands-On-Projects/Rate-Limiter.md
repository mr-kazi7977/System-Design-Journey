# 🚀 **Rate Limiter - System Design & Implementation**  

## 📌 **Introduction**  

A **Rate Limiter** is used to control the number of requests a client can make within a specific time window. This helps prevent:  
✅ **Abuse** – Stops excessive API requests.  
✅ **DDoS Attacks** – Prevents system overload.  
✅ **Fair Usage** – Ensures resources are fairly distributed.  

📌 **Real-World Examples:**  
- **API Rate Limiting** – Prevents excessive requests per second.  
- **Login Brute Force Protection** – Limits login attempts.  
- **Web Scraping Prevention** – Stops bots from overloading the system.  

---

## 🔥 **System Requirements**  

### **Functional Requirements**  
✅ Restrict users to a certain number of requests per second/minute/hour.  
✅ Block requests exceeding the limit.  
✅ Apply limits per **IP, user, or API key**.  
✅ Allow **custom rate limits** for different users (e.g., free vs. premium users).  

### **Non-Functional Requirements**  
✅ Low latency – Should not slow down requests.  
✅ Scalability – Should work for millions of users.  
✅ High availability – No single point of failure.  

---

## 🔥 **Rate Limiting Algorithms**  

### **1️⃣ Token Bucket (Most Common ✅)**  
- Each user gets a **bucket of tokens** (e.g., 100 tokens per minute).  
- Each request **removes a token**.  
- If **tokens run out**, further requests are blocked until tokens refill.  

📌 **Best For:** APIs, authentication systems.  

### **2️⃣ Leaky Bucket**  
- Requests enter a **queue** and are processed at a fixed rate.  
- If the queue is full, **excess requests are dropped**.  

📌 **Best For:** Traffic shaping (e.g., video streaming).  

### **3️⃣ Sliding Window (Time-Based Counters)**  
- Counts the number of requests in a **moving time window**.  
- More accurate than fixed window counters.  

📌 **Best For:** General API request limiting.  

---

## 🔥 **Database & Caching Design**  

### **1️⃣ Redis (Recommended ✅)**  
- Redis is **fast (in-memory)** and supports **atomic counters**.  
- Stores `{userID → request_count}` with **TTL (time-to-live)**.  

📌 **Example Redis Key:**  
```text
rate_limit:user123 → 5 (Expires in 1 minute)
2️⃣ Alternative: MySQL/PostgreSQL (Not Recommended ❌)
Requires locking and may cause performance bottlenecks.

🔥 Rate Limiting Implementation (Spring Boot + Redis)
1️⃣ Using Redis + Bucket4J (Token Bucket Approach)
📌 Add Dependencies (pom.xml)

```xml
<dependency>
    <groupId>com.github.vladimir-bukhtoyarov</groupId>
    <artifactId>bucket4j-core</artifactId>
    <version>8.0.0</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
📌 Rate Limiting Configuration (RateLimiterService.java)

```java
import io.github.bucket4j.*;
import org.springframework.stereotype.Service;
import java.time.Duration;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

@Service
public class RateLimiterService {
    private final Map<String, Bucket> buckets = new ConcurrentHashMap<>();

    public boolean isAllowed(String userId) {
        Bucket bucket = buckets.computeIfAbsent(userId, k -> createNewBucket());
        return bucket.tryConsume(1); // Consume 1 token per request
    }

    private Bucket createNewBucket() {
        return Bucket.builder()
                .addLimit(Bandwidth.classic(10, Refill.greedy(10, Duration.ofMinutes(1))))
                .build();
    }
}
```
📌 Controller to Apply Rate Limit (RateLimiterController.java)

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class RateLimiterController {
    private final RateLimiterService rateLimiterService;

    public RateLimiterController(RateLimiterService rateLimiterService) {
        this.rateLimiterService = rateLimiterService;
    }

    @GetMapping("/limited-endpoint")
    public String accessLimitedEndpoint(@RequestHeader("User-ID") String userId) {
        if (!rateLimiterService.isAllowed(userId)) {
            return "429 Too Many Requests – Try again later!";
        }
        return "Request successful!";
    }
}
```
📌 How It Works?

Each user has 10 requests per minute.

If they exceed the limit, they get 429 Too Many Requests.

🔥 Scaling Rate Limiting
✅ Redis Cluster – Scale Redis across multiple nodes.
✅ Distributed Rate Limiting – Use Redis in multiple regions for failover.
✅ Global Rate Limits – Enforce system-wide limits across all users.

🔥 Security Considerations
✅ Prevent Rate Limit Bypass

Block IP switching (use User ID-based limiting).

✅ Rate Limit Exemptions

Allow VIP users to have higher limits.

✅ Logging & Monitoring

Use Prometheus + Grafana to monitor blocked requests.

🔥 Testing the Rate Limiter
📌 Using Postman or cURL

```sh
curl -H "User-ID: user123" http://localhost:8080/api/limited-endpoint
```
📌 Expected Responses:
✅ Within Limit: Request successful!
❌ Exceeded Limit: 429 Too Many Requests

🎯 Final Thoughts
✅ Designed a scalable Rate Limiter with:
✔ Redis-based token bucket.
✔ Spring Boot & Bucket4J implementation.
✔ Protection against API abuse & attacks.

📌 Next Steps:
🔥 Implement dynamic rate limits (e.g., free vs. premium users).

🔥 Interview Questions
1️⃣ How would you scale a rate limiter for millions of users?
2️⃣ What’s the difference between Token Bucket & Leaky Bucket?
3️⃣ How do you handle burst traffic efficiently?
4️⃣ How do you prevent rate limit bypassing?

📌 Next Project: Video Streaming System 🚀
