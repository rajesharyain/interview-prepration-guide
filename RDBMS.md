# 🚀 Technical Interview Guide with Sample Answers

---

## 📌 1. Software Requirements & Agile Process

### ✅ 1.1 What do you understand by *Functional* and *Non-Functional Requirements*?
**Answer:**  
- **Functional Requirements**: What the system should do (features, actions, tasks).  
  *Example*: "Allow users to reset their password."
- **Non-Functional Requirements**: How the system performs (performance, scalability, security).  
  *Example*: "Password reset should complete in under 2 seconds."

---

### ✅ 1.2 If your PM asks you to develop a new feature, what steps would you follow in Scrum?
**Answer:**  
- **Backlog Refinement**: Clarify requirements and acceptance criteria.
- **Sprint Planning**: Estimate effort, prioritize.
- **Development**: Implement, write tests.
- **Daily Standup**: Report progress, discuss blockers.
- **Code Review & Merge**: Pull requests, pair programming.
- **Testing**: Unit, integration, regression.
- **Demo & Retrospective**: Show work done, gather feedback.

---

## 📌 2. DevOps, CI/CD & Security

### ✅ 2.1 What potential drawbacks do you see in CI/CD?
**Answer:**  
- Overhead in maintaining pipelines.
- Security risks if not well secured.
- Cost for infrastructure.
- Risk of unstable code if tests are weak.

---

### ✅ 2.2 Which tools have you used to scan containers for vulnerabilities?
**Answer:**  
- Examples: **Twistlock (Prisma Cloud)**, **Snyk Container**, **Anchore**, **Trivy**.
- Integrated into CI/CD pipelines to block builds with high-severity vulnerabilities.

---

### ✅ 2.3 How do you secure communication between services (REST, gRPC, HTTP)?
**Answer:**  
- Use **HTTPS/TLS** for encryption.
- Implement **mutual TLS (mTLS)** for service-to-service auth.
- Use **OAuth 2.0/JWT** for token-based authentication.
- Secure gRPC with mTLS (built on HTTP/2).
- Add **API Gateways** for rate limiting and logging.

---

### ✅ 2.4 How do you encrypt/decrypt PII data in POST requests?
**Answer:**  
- Use TLS for transport encryption.
- Encrypt sensitive payload with **symmetric encryption** (AES).
- Use **asymmetric keys** (RSA) to securely exchange symmetric keys.

---

## 📌 3. Cloud & Databases

### ✅ 3.1 What is RDS and DynamoDB?
**Answer:**  
- **RDS**: Managed relational DB (e.g., MySQL, Postgres).
- **DynamoDB**: NoSQL, serverless, highly scalable.
- Use RDS for complex joins/transactions, DynamoDB for fast key-value lookups.

---

### ✅ 3.2 Explain database indexes and their drawbacks.
**Answer:**  
- Indexes speed up read queries.
- **Drawbacks**: More storage, slower writes due to index maintenance, risk of stale indexes.

---

### ✅ 3.3 What are transactions? What is an autonomous transaction?
**Answer:**  
- **Transaction**: ACID unit of work.
- **Autonomous Transaction**: Independent transaction inside another, commits/rolls back independently. Useful for logging/audit.

---

### ✅ 3.4 Explain isolation levels in databases.
**Answer:**  
- Controls visibility of uncommitted changes.
- **Levels**: Read Uncommitted, Read Committed, Repeatable Read, Serializable.
- Higher isolation → fewer anomalies → more locking → less concurrency.

---

### ✅ 3.5 Optimistic vs. Pessimistic Locking.
**Answer:**  
- **Optimistic**: No lock; conflicts checked at commit.
- **Pessimistic**: Locks rows to prevent others from writing.

---

### ✅ 3.6 Difference between `UNION` vs. `UNION ALL` + Example
**Answer:**  
- `UNION`: Removes duplicates.
- `UNION ALL`: Keeps duplicates.

Example:  
