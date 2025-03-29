# 🔐 **Security & Best Practices in System Design**  

## 🚀 **Introduction**  

Security is **critical** in system design to protect **data, users, and infrastructure** from attacks. A well-designed system follows **security best practices** to ensure **confidentiality, integrity, and availability (CIA Triad).**  

📌 **Why is Security Important?**  
✅ Prevents unauthorized access to sensitive data.  
✅ Protects against cyberattacks (DDoS, SQL Injection, XSS).  
✅ Ensures compliance with security standards (ISO, GDPR, PCI-DSS).  

---

## 🔥 **1. Authentication & Authorization**  

### 📌 **Authentication (Who are you?)**  
Authentication verifies the user's identity. Common methods:  
✅ **Username & Password (Basic Auth)** – Traditional but weak.  
✅ **OAuth 2.0 & OpenID Connect** – Secure token-based authentication.  
✅ **JWT (JSON Web Tokens)** – Stateless authentication.  
✅ **Multi-Factor Authentication (MFA)** – Adds extra security layers.  

📌 **Example: JWT Authentication**  
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
{
  "sub": "user123",
  "exp": 1712345678,
  "role": "admin"
}
```
✅ JWTs are signed and stored client-side, reducing database lookups.

📌 Authorization (What can you access?)
Authorization ensures users only access what they’re allowed.

✅ Role-Based Access Control (RBAC)

Users have specific roles (Admin, User, Guest).

Each role has defined permissions.

✅ Attribute-Based Access Control (ABAC)

Access is granted based on user attributes (location, device, etc.).

📌 Example: RBAC Policy in Spring Security

```java
@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests(auth -> auth
            .requestMatchers("/admin/**").hasRole("ADMIN")
            .requestMatchers("/user/**").hasRole("USER")
            .anyRequest().authenticated()
        );
        return http.build();
    }
}
```
✅ Only Admins can access /admin/** routes.

🔥 2. API Security Best Practices
📌 Common API Security Threats:
❌ SQL Injection – Injecting malicious SQL queries.
❌ Cross-Site Scripting (XSS) – Running scripts in a user's browser.
❌ Cross-Site Request Forgery (CSRF) – Exploiting user sessions.

✅ How to Secure APIs?
1️⃣ Use HTTPS (TLS/SSL) – Encrypts data in transit.
2️⃣ Validate & Sanitize Input – Prevents SQL Injection & XSS attacks.
3️⃣ Use API Rate Limiting – Prevents DDoS and brute-force attacks.
4️⃣ Implement OAuth 2.0 – Secure authentication.
5️⃣ Encrypt Sensitive Data – Use AES-256 for strong encryption.

📌 Example: Rate Limiting with Spring Boot & Redis

```java@RateLimiter(name = "default", fallbackMethod = "rateLimitFallback")
public ResponseEntity<String> apiRequest() {
    return ResponseEntity.ok("Request Processed");
}
public ResponseEntity<String> rateLimitFallback() {
    return ResponseEntity.status(429).body("Too many requests. Try later.");
}
```
✅ Limits API requests to prevent abuse.

🔥 3. Secure Data Storage
📌 Best Practices for Secure Storage:
✅ Use Database Encryption – Protects sensitive data.
✅ Hash Passwords (bcrypt, Argon2) – Prevents password leaks.
✅ Tokenize Payment Data – Replaces sensitive data with a token.

📌 Example: Hashing Passwords with BCrypt in Java

```java
BCryptPasswordEncoder encoder = new BCryptPasswordEncoder();
String hashedPassword = encoder.encode("mySecurePassword");
```
✅ Even if stolen, hashed passwords cannot be reversed easily.

🔥 4. Protecting Against DDoS Attacks
📌 What is a DDoS Attack?
A Distributed Denial of Service (DDoS) attack floods a system with traffic, causing downtime.

✅ How to Prevent DDoS?

Use Rate Limiting – Restrict API requests per IP.

Enable Web Application Firewall (WAF) – Filters malicious traffic.

Use CDN (Cloudflare, AWS Shield) – Absorbs attack traffic.

Monitor Logs & Traffic – Detects abnormal spikes.

📌 Example: Cloudflare Rate Limiting Configuration
```json{
  "rule": "block",
  "requests_per_minute": 100,
  "action": "throttle"
}```

✅ Blocks IPs exceeding 100 requests per minute.

🔥 5. Secure Cloud Infrastructure
✅ Best Practices for Securing Cloud Services:

Use IAM Roles & Policies – Restrict cloud permissions.

Enable VPC & Private Subnets – Isolate sensitive services.

Encrypt S3 Buckets & Databases – Protect stored data.

Enable Logging & Monitoring – Detect unauthorized access.

📌 Example: AWS IAM Role Policy

```json{
  "Effect": "Deny",
  "Action": "s3:DeleteBucket",
  "Resource": "arn:aws:s3:::my-secure-bucket"
}```

✅ Prevents accidental bucket deletion.

🔥 6. Secure Logging & Monitoring
✅ Why Logging is Important?

Detects intrusions & suspicious activity.

Helps debug security incidents.

📌 Best Practices for Secure Logging:

Use Centralized Logging (ELK, Prometheus) – Aggregate logs.

Mask Sensitive Data – Never log passwords.

Use SIEM (Security Information & Event Management) – AI-powered monitoring.

📌 Example: Masking Sensitive Logs in Java

```java
log.info("User Login: [email protected], Password: ****");
```
✅ Never log raw passwords or sensitive data.

🔥 7. Security Best Practices Checklist
✅ Authentication & Authorization
☑ Use OAuth 2.0 / JWT for API authentication.
☑ Implement RBAC for access control.
☑ Enforce MFA for sensitive actions.

✅ API Security
☑ Enforce HTTPS and encrypt API responses.
☑ Implement rate limiting to prevent abuse.
☑ Sanitize input to prevent SQL Injection & XSS.

✅ Data Protection
☑ Hash passwords with bcrypt or Argon2.
☑ Encrypt sensitive data (AES-256).
☑ Store API keys securely (Vault, AWS Secrets Manager).

✅ Infrastructure Security
☑ Restrict cloud permissions with IAM policies.
☑ Use WAF & CDN to mitigate DDoS attacks.
☑ Enable logging & real-time monitoring.

✅ Incident Response
☑ Set up alerts for unauthorized access attempts.
☑ Have an incident response plan in case of a breach.
☑ Regularly test system security with penetration testing.

🔥 Security Interview Questions
📌 Basic Questions
1️⃣ What is OAuth 2.0, and how does it work?
2️⃣ What is the difference between RBAC and ABAC?
3️⃣ How do you prevent SQL Injection in APIs?
4️⃣ Why should we use HTTPS instead of HTTP?

📌 Advanced Questions
1️⃣ How do you secure microservices communication?
2️⃣ How do you prevent DDoS attacks on APIs?
3️⃣ Explain Zero Trust Security Model.
4️⃣ How does JWT prevent session hijacking?

📌 Hands-on Challenge:
👉 "Design a secure authentication system for an e-commerce website."

🎯 Conclusion
✅ Security is a continuous process, not a one-time fix.
✅ Use encryption, authentication, and monitoring to safeguard systems.
✅ Follow best practices to prevent cyber threats.

🔥 Next Topic: Mock Interviews 🚀
