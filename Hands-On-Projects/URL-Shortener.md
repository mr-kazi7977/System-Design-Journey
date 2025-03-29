# 🔗 **URL Shortener - System Design & Implementation**  

## 🚀 **Introduction**  

A **URL Shortener** is a system that converts a **long URL** into a **short URL** (e.g., `bit.ly/xyz123`). Users can access the original URL by visiting the short URL.  

📌 **Use Cases:**  
✅ Shorten long URLs for easy sharing.  
✅ Track analytics (clicks, geolocation, etc.).  
✅ Expire or delete URLs after a certain period.  

---

## 🔥 **System Requirements**  

### **Functional Requirements**  
✅ Users can **shorten** long URLs.  
✅ Users can **access** the original URL using the short link.  
✅ Short links should be **unique** and **persistent**.  
✅ (Optional) Track click analytics.  

### **Non-Functional Requirements**  
✅ High **availability** – The system should handle millions of requests.  
✅ Low **latency** – Redirects should be fast.  
✅ Scalability – Support growing traffic.  

---

## 🔥 **High-Level System Design**  

📌 **Architecture Overview:**  
1️⃣ A user submits a long URL.  
2️⃣ The system generates a **unique short URL**.  
3️⃣ When a user visits the short URL, they are **redirected** to the original URL.  

✅ **Components Involved:**  
- **API Gateway** – Handles incoming requests.  
- **Shortening Service** – Converts long URLs to short ones.  
- **Database** – Stores mappings (`shortURL ↔ longURL`).  
- **Cache (Redis)** – Stores frequently accessed URLs.  
- **Load Balancer** – Distributes traffic among servers.  

📌 **System Flow Diagram:**  
User → Load Balancer → URL Shortener Service → Database
↳ (Optional) Cache Lookup → Redirect

pgsql
Copy
Edit

---

## 🔥 **Database Design**  

### **Schema (Relational DB - MySQL/PostgreSQL)**  

| Column       | Type         | Description                     |
|-------------|-------------|---------------------------------|
| id          | BIGINT (PK)  | Unique ID                      |
| short_url   | VARCHAR(10)  | Shortened URL key              |
| long_url    | TEXT         | Original long URL              |
| created_at  | TIMESTAMP    | Timestamp of URL creation      |
| expires_at  | TIMESTAMP    | (Optional) Expiry timestamp    |

📌 **Indexes:**  
✅ Index `short_url` for fast lookups.  

📌 **Alternative: NoSQL (MongoDB)**  
```json
{
  "short_url": "abc123",
  "long_url": "https://www.example.com/very-long-url",
  "created_at": "2025-03-29T12:00:00Z",
  "expires_at": "2025-04-29T12:00:00Z"
}
```
🔥 URL Shortening Algorithm
✅ How do we generate a short URL?

1️⃣ Hashing Approach (MD5/SHA)
Hash the URL and take first 6-8 characters.

❌ Problem: Hash collisions may occur.

2️⃣ Base62 Encoding (Recommended ✅)
Convert a unique ID (auto-increment or UUID) into a Base62 string (A-Z, a-z, 0-9).

📌 Java Implementation:

```java
public class Base62Encoder {
    private static final String CHARSET = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    public static String encode(long num) {
        StringBuilder sb = new StringBuilder();
        while (num > 0) {
            sb.append(CHARSET.charAt((int) (num % 62)));
            num /= 62;
        }
        return sb.reverse().toString();
    }
}
```
🔥 Caching with Redis
✅ Why use caching?

Speeds up lookups (avoids DB hits).

Reduces database load.

📌 Redis as a Cache Layer:

Store {shortURL → longURL} mappings.

Set TTL (time-to-live) to expire old URLs.

📌 Example: Caching with Redis (Spring Boot)

```java
@Cacheable(value = "urls", key = "#shortUrl")
public String getLongUrl(String shortUrl) {
    return urlRepository.findByShortUrl(shortUrl);
}
```
🔥 API Endpoints
📌 1️⃣ Shorten URL API

```http
POST /shorten
{
  "long_url": "https://www.example.com/some-long-url"
}
Response:
{
  "short_url": "https://short.ly/xyz123"
}
```
📌 2️⃣ Redirect API

```http
GET /xyz123
Response: 302 Redirect → "https://www.example.com/some-long-url"
```
📌 3️⃣ Analytics API (Optional)

```http
GET /analytics/xyz123
Response:
{
  "clicks": 1052,
  "last_accessed": "2025-03-28T10:00:00Z"
}
```
🔥 Scaling the System
✅ Horizontal Scaling

Deploy multiple URL Shortener Service instances.

Use Load Balancer (Nginx/AWS ALB).

✅ Database Sharding

Partition URLs by hash to multiple DB instances.

✅ Event-Driven Processing

Use Kafka/RabbitMQ for tracking analytics asynchronously.

🔥 Security Considerations
✅ Prevent Short URL Enumeration

Avoid sequential short URLs (1, 2, 3…).

Use randomized Base62 keys instead.

✅ Blacklist Malicious URLs

Use URL validation APIs to prevent phishing links.

✅ Rate Limiting

Prevent abuse by limiting API requests per user.

Example: Allow 100 requests per minute per IP.

📌 Rate Limiting with Spring Boot & Redis

```java
@RateLimiter(name = "shortenRateLimit", fallbackMethod = "rateLimitExceeded")
public ResponseEntity<String> shortenUrl(@RequestBody UrlRequest request) {
    return ResponseEntity.ok(urlService.shorten(request.getLongUrl()));
}
```
🔥 Deployment & Monitoring
✅ Deploy on AWS/GCP

Use EC2 instances for hosting the service.

Use RDS (PostgreSQL) as the database.

Use Redis Elasticache for caching.

✅ Monitoring

Prometheus & Grafana for tracking API response times.

CloudWatch/Azure Monitor for logs & alerts.

🎯 Final Thoughts
✅ Designed a scalable URL Shortener with:
✔ Base62 encoding for short URLs.
✔ Redis caching for fast lookups.
✔ API Gateway & Load Balancer for scalability.
✔ Security best practices (rate limiting, blacklisting).

📌 Next Steps:
🔥 Implement a working version using Spring Boot & Redis 🚀

🔥 Interview Questions
1️⃣ How would you handle billions of URLs in the database?
2️⃣ What are the trade-offs of using hashing vs Base62 for short URLs?
3️⃣ How do you prevent abuse of a URL shortener system?
4️⃣ How do you design a highly available URL shortener?

📌 Next Project: Rate Limiter System 🚀
