# 📌 Database Design  

## 🚀 Introduction  
Database design is crucial for **scalability, consistency, and performance** in system design. Choosing the right database model impacts how efficiently data is stored, retrieved, and maintained.  

---

## 🔥 Key Concepts  

### 1️⃣ **SQL vs NoSQL**  
Databases are broadly classified into:  

| Feature        | SQL (Relational) | NoSQL (Non-Relational) |
|---------------|-----------------|-----------------------|
| Schema        | Fixed (Tables)  | Flexible (JSON, Key-Value, Graph) |
| Scaling       | Vertical Scaling | Horizontal Scaling |
| Transactions  | ACID Compliance | BASE Model |
| Use Case      | Banking, E-commerce | Social Media, Real-time Apps |

📌 **Examples:**  
- **SQL Databases** → MySQL, PostgreSQL, SQL Server  
- **NoSQL Databases** → MongoDB, Cassandra, Redis  

---

### 2️⃣ **Normalization vs Denormalization**  
✅ **Normalization**:  
- Reduces data redundancy.  
- Data is stored in multiple related tables.  
- Follows **1NF → 2NF → 3NF** rules.  
- **Example:** Breaking a `Users` table into `Users` and `Addresses` tables.  

✅ **Denormalization**:  
- Increases read performance by reducing joins.  
- Data duplication is allowed for faster retrieval.  
- Common in NoSQL databases.  

📌 **When to Use?**  
- **Normalization** → When write-heavy and data integrity is critical.  
- **Denormalization** → When read-heavy and low latency is required.  

---

### 3️⃣ **Indexes**  
Indexes speed up queries by allowing faster lookups.  

✅ **Types of Indexes:**  
- **Primary Index** → Based on primary key (Unique & Auto-Indexed).  
- **Secondary Index** → Created on non-primary key columns to speed up search.  
- **Composite Index** → Uses multiple columns to improve query performance.  

📌 **Example:**  
```sql
CREATE INDEX idx_name ON Users(name);
```

ACID vs BASE in Simple Terms
Think of ACID like a bank and BASE like WhatsApp messages.

🔹 ACID (Used in SQL Databases – MySQL, PostgreSQL, Oracle)
✅ Like a Bank Transaction 🏦

Atomicity → "All or Nothing" → If you send ₹1000, it should fully go or not at all.
Consistency → "Always Correct" → Your bank balance should never be wrong.
Isolation → "No Mixing" → Your transfer should not get mixed up with someone else's.
Durability → "Never Lost" → Even if the system crashes, your transaction is saved.
💡 Used in: Banking, Insurance Policy Issuance, Payments

🔹 BASE (Used in NoSQL Databases – MongoDB, DynamoDB, Redis)
✅ Like WhatsApp Messages 📱

Basically Available → "Always Working" → Even if some servers are slow, you can still send messages.
Soft State → "Data Can Change" → Message status (Sent → Delivered → Read) updates over time.
Eventual Consistency → "Will Sync Later" → If the internet is slow, messages will deliver eventually.
💡 Used in: Social Media, POS Agent Tracking, Lead Management

🚀 When to Use What?
✔️ ACID → Use when data must always be correct & transactions must never fail (Bank, Insurance).
✔️ BASE → Use when speed & availability are more important than instant accuracy (WhatsApp, POS tracking).

# 📌 Caching Strategies  

## 🚀 Introduction  
Caching helps **reduce latency, improve response times, and handle high traffic loads** by storing frequently accessed data in memory instead of fetching it from the database every time.  

---

## 🔥 Why Use Caching?  
✅ Reduces **database load** by serving frequent queries from memory.  
✅ Improves **performance** (lower latency).  
✅ Enhances **scalability** to handle more users.  

📌 **Example:**  
Without caching → Every request hits the database → Slower response.  
With caching → Frequently requested data is stored in memory → Faster response.  

---

## 🔥 Types of Caching  

### 1️⃣ **Client-Side Caching**  
- Stored **locally** in the user's browser (cookies, localStorage, sessionStorage).  
- Used for static assets (CSS, JS, images).  

📌 **Example:**  
```js
localStorage.setItem("username", "JohnDoe");
console.log(localStorage.getItem("username")); // Output: JohnDoe
