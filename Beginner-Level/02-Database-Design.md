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
