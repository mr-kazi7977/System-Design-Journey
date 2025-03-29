🔹 Scenario
Interviewer: Design a scalable system for WhatsApp, focusing on messaging, real-time delivery, and group chats while handling millions of active users.

🔹 Step 1: Clarifying Questions
(These help define the problem scope.)

✅ What are the core features?

One-on-one messaging.

Group chats.

Media (images, videos, voice notes) sharing.

End-to-end encryption.

Online status & read receipts.

✅ Real-time or eventual consistency?

Real-time messaging is required.

Eventual consistency for message sync across devices.

✅ Read vs. Write ratio?

Read and write are almost equal (since every sent message is received instantly).

✅ Should messages be stored permanently?

Yes, but can be deleted after delivery to optimize storage.

🔹 Step 2: High-Level Design
🔥 Architecture Overview
1️⃣ Clients (Mobile, Web) → API Gateway
2️⃣ Load Balancer → Microservices (Messaging Service, User Service, Group Service)
3️⃣ Databases (SQL for user data, NoSQL for messages, Blob storage for media)
4️⃣ Caching (Redis, Memcached) for message queues
5️⃣ Message Queue (Kafka, RabbitMQ) for asynchronous delivery
6️⃣ Push Notification Service (Firebase, APNs) for offline users

🔹 Step 3: Database Schema Design
📌 Users Table (SQL - MySQL, PostgreSQL)
```sql
CREATE TABLE users (
    user_id BIGINT PRIMARY KEY,
    phone_number VARCHAR(20) UNIQUE,
    created_at TIMESTAMP
);
```
📌 Messages Table (NoSQL - Cassandra, DynamoDB)
```json
{
    "message_id": "12345",
    "sender_id": "789",
    "receiver_id": "567",
    "content": "Hello!",
    "timestamp": "2025-03-29T12:00:00Z",
    "status": "delivered"
}
```
📌 Group Chats Table (SQL - Many-to-Many Relationship)
```sql
CREATE TABLE group_members (
    group_id BIGINT,
    user_id BIGINT,
    PRIMARY KEY (group_id, user_id)
);
```
🔹 Step 4: Handling High Traffic & Scalability
✅ Partition messages by user_id → Each user’s messages go to a different database shard.
✅ Use Redis as a message broker → Store undelivered messages until users come online.
✅ WebSockets for real-time updates → Persistent connection between client & server.
✅ Media storage on a CDN → Images, videos stored on AWS S3, Cloudflare.

🔹 Step 5: Message Delivery Mechanism
📌 When User A sends a message to User B:
1️⃣ API receives the message & stores it in the Message Service.
2️⃣ The Message Queue (Kafka) sends it to the recipient’s device.
3️⃣ If the recipient is online, they receive it immediately via WebSockets.
4️⃣ If offline, the message is stored in Redis until they come online.
5️⃣ A push notification (Firebase/APNs) is sent.

✅ Read Receipts:

Single tick (✓) → Message sent to the server.

Double tick (✓✓) → Message delivered to the recipient.

Blue tick (✓✓) → Message read.

🔹 Step 6: API Design
🚀 Send Message API
```http
POST /send_message
{
    "sender_id": "123",
    "receiver_id": "456",
    "content": "Hey there!"
}
```
🚀 Get Messages API
```http
GET /get_messages?user_id=123
```
🔹 Step 7: Security & Encryption
✅ End-to-End Encryption (E2EE) → Messages are encrypted using AES-256 before sending.
✅ Device Binding → Messages can only be read on the recipient’s registered device.
✅ Metadata Protection → No storage of message content on servers.

🔹 Step 8: Interview Questions & Challenges
✅ How do you handle 1 million messages per second?
✅ How do you sync messages across multiple devices?
✅ How do you ensure message delivery even when a user is offline?
✅ How to design WhatsApp Status (like Instagram Stories)?

🎯 Final Thoughts
✅ Explain trade-offs (SQL vs. NoSQL, Push vs. Pull).
✅ Use diagrams to showcase architecture.
✅ Think about edge cases (message deletion, privacy settings).
