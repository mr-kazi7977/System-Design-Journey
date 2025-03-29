📌 System Design - YouTube
🔹 Scenario
Interviewer: Design a scalable video streaming platform like YouTube, focusing on video uploads, streaming, recommendations, and handling millions of concurrent users.

🔹 Step 1: Clarifying Questions
(These help define the problem scope.)

✅ What are the core features?

Video uploads.

Video playback (adaptive streaming).

Comments, likes, and subscriptions.

Search & recommendations.

Live streaming.

✅ How many users and videos do we expect?

1B+ users, 500 hours of video uploaded per minute.

Millions of concurrent video streams.

✅ Should videos be stored permanently?

Yes, but older/low-view videos can be stored in cold storage.

✅ Read vs. Write ratio?

95% reads (video views) vs. 5% writes (uploads, comments).

🔹 Step 2: High-Level Design
🔥 Architecture Overview
1️⃣ Clients (Mobile, Web, Smart TVs) → API Gateway
2️⃣ Load Balancer → Microservices (Video Service, User Service, Comment Service)
3️⃣ Databases (SQL for metadata, NoSQL for comments, Blob storage for video files)
4️⃣ Caching (Redis, CDN) for fast video streaming
5️⃣ Message Queue (Kafka) for async processing
6️⃣ Recommendation Engine (Machine Learning-based)

🔹 Step 3: Database Schema Design
📌 Users Table (SQL - MySQL, PostgreSQL)
```sql
CREATE TABLE users (
    user_id BIGINT PRIMARY KEY,
    username VARCHAR(50) UNIQUE,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP
);
```
📌 Videos Table (SQL - MySQL, PostgreSQL)
```sql
CREATE TABLE videos (
    video_id BIGINT PRIMARY KEY,
    user_id BIGINT,
    title VARCHAR(255),
    description TEXT,
    upload_time TIMESTAMP,
    views BIGINT DEFAULT 0,
    PRIMARY KEY (video_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```
📌 Comments Table (NoSQL - MongoDB, Cassandra)
```json
{
    "comment_id": "123",
    "video_id": "456",
    "user_id": "789",
    "text": "Great video!",
    "timestamp": "2025-03-29T12:00:00Z"
}
```
🔹 Step 4: Video Upload & Storage
📌 How do we handle video uploads?
1️⃣ User uploads a raw video → Stored in S3 (or Google Cloud Storage).
2️⃣ Encoding Service converts it into multiple resolutions (240p, 480p, 720p, 1080p, 4K).
3️⃣ Thumbnails are generated for previews.
4️⃣ Metadata (title, tags, category) is stored in SQL DB.

✅ Storage Optimization:

Cold storage (Glacier, Deep Archive) for old videos.

CDN (Cloudflare, Akamai) to cache & distribute popular videos.

🔹 Step 5: Video Streaming & Delivery
📌 How do we stream videos efficiently?
1️⃣ Use HLS (HTTP Live Streaming) or DASH for adaptive bitrate streaming.
2️⃣ Store video chunks (e.g., 10-second segments) instead of full files.
3️⃣ CDNs cache popular videos close to users to reduce latency.
4️⃣ Load balance traffic between multiple data centers worldwide.

✅ Adaptive Streaming:

Low bandwidth → 240p or 480p.

High bandwidth → 1080p or 4K.

✅ Edge Caching:

Popular videos cached on CDN (Cloudflare, Akamai).

Least-viewed videos stored in cold storage.

🔹 Step 6: Recommendation Engine
📌 How does YouTube recommend videos?
1️⃣ Collaborative Filtering (Similar users watch similar videos).
2️⃣ Content-Based Filtering (Videos with similar tags, categories).
3️⃣ Deep Learning (AI-based) (User behavior, watch history, likes).
4️⃣ Trending Videos (Videos with high engagement in a short time).

✅ Data Processing with Batch & Real-Time Pipelines:

Batch Processing (Hadoop, Spark) for historical data.

Real-time Processing (Kafka, Flink) for trending videos.

🔹 Step 7: API Design
🚀 Upload Video API
```http
POST /upload_video
{
    "user_id": "123",
    "title": "My Travel Vlog",
    "description": "Exploring the Himalayas!",
    "file": "video.mp4"
}
```
🚀 Get Video API
```http
GET /get_video?video_id=456
```
🚀 Comment on Video API
```http
POST /comment
{
    "user_id": "789",
    "video_id": "456",
    "text": "Awesome video!"
}
```
🔹 Step 8: Security & Scalability
✅ Rate Limiting (Prevent DDoS attacks on API requests).
✅ Token-Based Authentication (OAuth 2.0, JWT for secure access).
✅ Encryption (AES-256 for video storage, HTTPS for data transfer).

✅ Horizontal Scaling (Multiple instances of encoding & streaming services).
✅ Sharding (Partition metadata based on video_id).

🔹 Step 9: Interview Questions & Challenges
✅ How do you handle 1 million concurrent video streams?
✅ How do you optimize storage costs for old videos?
✅ How do you prevent piracy & unauthorized downloads?
✅ How to design YouTube Shorts (TikTok-style short videos)?

🎯 Final Thoughts
✅ Explain trade-offs (SQL vs. NoSQL, HLS vs. DASH).
✅ Use diagrams to showcase architecture.
✅ Think about edge cases (copyright protection, buffering issues).
