### 📦 3-Tier Architecture

1. **Frontend (Tier 1)** – Static website hosted in S3  
2. **Backend Logic (Tier 2)** – Lambda functions connected through API Gateway  
3. **Database (Tier 3)** – DynamoDB Global Tables replicated across two regions

---

### 🗺️ High-Level Flow

User → S3-hosted frontend → API Gateway → Lambda → DynamoDB (replicated)  
↳ If Region A fails, Route 53 sends traffic to Region B automatically.



![Architecture Diagram](https://github.com/user-attachments/assets/d76940fb-bbac-4d6b-8a9f-2a343959a482)
