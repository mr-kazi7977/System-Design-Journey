# 🎥 **Video Streaming System - System Design & Implementation**  

## 📌 **Introduction**  

A **Video Streaming System** allows users to stream video content seamlessly.  
Examples: **Netflix, YouTube, Amazon Prime Video**  

📌 **Key Challenges in Video Streaming:**  
✅ Efficiently store & serve large video files.  
✅ Adaptive streaming for different network conditions.  
✅ Handle high concurrent user requests.  
✅ Ensure smooth playback with minimal buffering.  

---

## 🔥 **System Requirements**  

### **1️⃣ Functional Requirements**  
✅ Users should be able to **upload, store, and stream** videos.  
✅ Support for **multiple resolutions** (240p, 480p, 720p, 1080p).  
✅ Adaptive Bitrate Streaming (ABS) for different network speeds.  
✅ Efficient content delivery via **CDN (Content Delivery Network)**.  
✅ Support for **video recommendations**.  

### **2️⃣ Non-Functional Requirements**  
✅ **High Availability** – No downtime, seamless video playback.  
✅ **Low Latency** – Ensure videos load quickly.  
✅ **Scalability** – Handle millions of concurrent users.  
✅ **Efficient Storage** – Reduce bandwidth and storage costs.  

---

## 🔥 **System Architecture**  

📌 **Components of a Video Streaming System:**  

1️⃣ **Video Ingestion Service** – Handles **video uploads & processing**.  
2️⃣ **Video Storage Service** – Stores videos in **multiple formats**.  
3️⃣ **Streaming Service** – Delivers video **on-demand** to users.  
4️⃣ **Content Delivery Network (CDN)** – **Caches & delivers** videos closer to users.  
5️⃣ **Database** – Stores **metadata** (titles, descriptions, thumbnails).  
6️⃣ **User Authentication & Access Control** – Ensures only authorized users can view content.  

---

## 🔥 **Video Upload & Processing Workflow**  

📌 **Step-by-step process:**  

1️⃣ **User uploads a video** to the platform.  
2️⃣ **Transcoding Service** converts the video into multiple resolutions (240p, 480p, 720p, 1080p).  
3️⃣ The processed video is **stored in cloud storage** (AWS S3, Google Cloud Storage).  
4️⃣ A **CDN caches** the video for fast delivery.  
5️⃣ The user selects a video, and the **Streaming Service** delivers the correct resolution based on network speed.  

---

## 🔥 **Database Design**  

📌 **Table: `videos` (Stores Video Metadata)**  

| Column         | Type          | Description                          |
|---------------|--------------|--------------------------------------|
| `video_id`    | VARCHAR(255)  | Unique ID for the video             |
| `title`       | TEXT          | Video title                         |
| `description` | TEXT          | Video description                   |
| `duration`    | INT           | Duration in seconds                 |
| `upload_date` | TIMESTAMP     | When the video was uploaded         |
| `storage_url` | VARCHAR(255)  | Link to stored video files          |
| `user_id`     | VARCHAR(255)  | ID of the uploader                  |

📌 **Table: `users` (Stores User Information)**  

| Column         | Type         | Description                         |
|---------------|-------------|-------------------------------------|
| `user_id`     | VARCHAR(255) | Unique User ID                     |
| `name`        | VARCHAR(255) | Full Name                          |
| `email`       | VARCHAR(255) | User Email                         |
| `subscription`| ENUM         | Free, Premium, VIP                 |

---

## 🔥 **Streaming Protocols & Formats**  

✅ **HLS (HTTP Live Streaming) – Recommended**  
- Used by YouTube & Netflix.  
- Splits videos into **small chunks (TS files)** and serves them over HTTP.  

✅ **DASH (Dynamic Adaptive Streaming over HTTP)**  
- Similar to HLS, used for adaptive streaming.  
- Supports **multiple resolutions** dynamically.  

✅ **RTMP (Real-Time Messaging Protocol)**  
- Used for **live streaming**.  

---

## 🔥 **Content Delivery Network (CDN) Optimization**  

📌 **Why use a CDN?**  
✅ Reduces **latency** by serving videos from servers **closest to users**.  
✅ Reduces **server load** by caching popular content.  
✅ Improves **scalability** to handle **millions of users**.  

📌 **CDN Providers:**  
- **Cloudflare CDN**  
- **AWS CloudFront**  
- **Google Cloud CDN**  

---

## 🔥 **Storage & Compression Techniques**  

✅ **Cloud Storage Solutions:**  
- AWS S3, Google Cloud Storage, Azure Blob Storage.  

✅ **Video Compression (Reduce Storage Cost)**  
- Use **H.264 or H.265 codec** for video encoding.  
- Convert video into **multiple bitrates** for adaptive streaming.  

📌 **Example: Transcoding in FFmpeg**  
```sh
ffmpeg -i input.mp4 -preset fast -crf 23 -c:v libx264 -c:a aac output.mp4
🔥 API Design (Spring Boot Example)
📌 Upload Video API (VideoUploadController.java)

```java
@RestController
@RequestMapping("/api/videos")
public class VideoUploadController {
    @PostMapping("/upload")
    public ResponseEntity<String> uploadVideo(@RequestParam("file") MultipartFile file) {
        String videoId = videoService.processUpload(file);
        return ResponseEntity.ok("Video uploaded successfully: " + videoId);
    }
}
```
📌 Fetch Video Metadata API (VideoController.java)

```java
@RestController
@RequestMapping("/api/videos")
public class VideoController {
    @GetMapping("/{videoId}")
    public ResponseEntity<VideoMetadata> getVideoMetadata(@PathVariable String videoId) {
        VideoMetadata metadata = videoService.getVideoMetadata(videoId);
        return ResponseEntity.ok(metadata);
    }
}
```
🔥 Scaling the System
✅ Horizontal Scaling:

Multiple video processing servers handle concurrent uploads.

Multiple streaming servers serve users across regions.

✅ Database Scaling:

Use sharding to distribute video metadata across databases.

Store popular videos in an in-memory cache (Redis).

✅ Storage Optimization:

Use cold storage (low-cost storage) for old videos.

Delete or compress unused videos to save space.

🔥 Security Best Practices
✅ Token-Based Authentication

Use JWT tokens to secure video access.

✅ Prevent Video Piracy

Signed URLs (Expire after some time).

Watermarking (Embed user ID into video frames).

✅ DDoS Protection

Use rate limiting to prevent bot abuse.

Protect APIs with WAF (Web Application Firewall).

🔥 Testing Video Streaming
📌 Test video playback with cURL

```sh
curl -I https://cdn.example.com/video123.m3u8
```
📌 Check CDN Response Time

```sh
ping cdn.example.com
```
🎯 Final Thoughts
✅ Designed a scalable Video Streaming System with:
✔ Efficient video storage & delivery.
✔ Adaptive Bitrate Streaming (HLS/DASH).
✔ CDN for optimized performance.

📌 Next Steps:
🔥 Implement video recommendations (AI-powered).

🔥 Interview Questions
1️⃣ How would you scale a video streaming service like Netflix?
2️⃣ How do CDNs improve video streaming performance?
3️⃣ What is the difference between HLS & DASH streaming protocols?
4️⃣ How do you prevent video piracy in a streaming system?

📌 Next Project: System Design Case Study - YouTube 🚀
