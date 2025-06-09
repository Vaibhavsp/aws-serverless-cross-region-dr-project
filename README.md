# ☁️ AWS Serverless Cross-Region Disaster Recovery Project (with Frontend)

This project is a **real-world cloud architecture** that demonstrates how to build a **highly available, serverless, cross-region disaster recovery system** using AWS.

✅ Zero Downtime
✅ Fully Serverless (No EC2!)
✅ Frontend + API + Global DB + Failover

---

## 🧠 What You Will Learn

> Even if you're a beginner or non-tech person, this project will help you understand:

* What serverless really means
* How apps handle region failure (Disaster Recovery)
* How frontend connects to backend APIs
* How AWS services talk to each other
* How to use Route 53 for automatic failover

---

## 🏟️ Architecture Overview

### 📦 3-Tier Architecture

1. **Frontend (Tier 1)** – Static website hosted in S3
2. **Backend Logic (Tier 2)** – Lambda functions connected through API Gateway
3. **Database (Tier 3)** – DynamoDB Global Tables replicated across two regions

---

### 🗺️ High-Level Flow

User → S3-hosted frontend → API Gateway → Lambda → DynamoDB (replicated)
↳ If Region A fails, Route 53 sends traffic to Region B automatically.

![0  Architecture Diagram](https://github.com/user-attachments/assets/433155af-6d5c-41fb-aa2c-9d8b6f0004bb)

---

## ⚙️ Technologies Used

* 🟩 AWS Lambda (Python) – for backend logic
* 🟨 API Gateway – for REST APIs
* 🗭 DynamoDB Global Tables – for multi-region database
* 🔴 Route 53 – for health check and failover
* 🕪 S3 – for hosting frontend (HTML+JS)
* 🔐 IAM Roles – for secure Lambda access

---

## 🧱 Project Setup – Fully Beginner Friendly

This guide walks you through building the entire project from scratch using **AWS Console** (no code editors required at first). You can follow this even if you’re new to AWS!

---

## ✅ Step-by-Step Setup Instructions (with Screenshots)

> 📸 I’ve included 55+ screenshots inside the `/screenshots` folder to make every step crystal clear.

### 🟩 Step 1: Create DynamoDB Table

* Go to DynamoDB in AWS Console → Create table
* Table name: `HighAvailabilityTable`
* Partition Key: `ItemId` (String)

![Step 1](https://github.com/user-attachments/assets/d39af8bb-0ab8-4910-be55-1e2be25be25c)

### 🟩 Step 2: Enable Global Table (Add Replica Region)

* Actions → Manage Global Tables → Add Region → Choose `ap-northeast-1`

![Step 2](https://github.com/user-attachments/assets/907643d8-56ee-42d8-a9bb-eedf98c9675f)

### 🟩 Step 3: Create IAM Role for Lambda

* Go to IAM → Create Role → Choose Lambda
* Attach:

  * `AWSLambdaBasicExecutionRole`
  * `AmazonDynamoDBFullAccess`

![10  Role Created](https://github.com/user-attachments/assets/87607286-79f9-4945-8cdf-9f2c32094290)

### 🟩 Step 4: Write Lambda Functions (read & write)

* Create two Lambda functions (`ReadFunction`, `WriteFunction`)
* Use `read_function.py` and `write_function.py` from `/lambda` folder

Find python scripts for lambda in `/python-scripts/`

![Step 4](https://github.com/user-attachments/assets/016637f7-bb77-4dff-80e6-9e78ab02d337)

### 🟩 Step 5: Setup API Gateway

* Create a REST API
* Add `/read` (GET) and `/write` (POST) resources
* Integrate each with respective Lambda

![Step 5](https://github.com/user-attachments/assets/368e2be0-bf63-42d5-bdd9-376b68216014)


### 🟩 Step 6: Enable CORS and Deploy API

* Enable CORS for both endpoints
* Deploy to a stage called `prod`
* Note down your endpoint URLs

![Step 6](https://github.com/user-attachments/assets/ae930c04-ad3a-4e45-a568-c618f723fc69)

### 🟩 Step 7: Repeat Lambda + API in Second Region

* Repeat steps 4–6 in `ap-northeast-1` region

### 🟩 Step 8: Create Route 53 Hosted Zone + Health Checks

* Create Hosted Zone (like `vaibhav-lab.com`)
* Add 2 Health Checks pointing to `/read` endpoints

![Step 8](https://github.com/user-attachments/assets/887a248c-ea4f-49a0-94a0-2379b5a18a56)

![Step 8](https://github.com/user-attachments/assets/d9bdada2-11dd-436e-87d7-a582ecbb66a2)

### 🟩 Step 9: Create Failover DNS Records

* Use routing policy = Failover
* Primary → primary API Gateway URL
* Secondary → secondary API Gateway URL
* Attach correct health checks

### 🟩 Step 10: Build Frontend

* Use `frontend/index.html`
* Replace the API URL in JavaScript
* Upload to S3
* Enable Static Website Hosting
* Make index.html public

---

## 🐛 Common Issues and Fixes

### ❌ Health Check Shows Unhealthy

* Cause: You entered full URL or missed `/read` in path
* Fix: Use domain name like `xyz.execute-api.amazonaws.com` and add `/prod/read` in the "Path"

### ❌ Lambda Returns 500 Error

* Cause: JSON body parsing failed
* Fix: Use `json.loads(event.get('body', '{}'))` instead of `event['body']`

### ❌ CORS error on frontend

* Fix: Enable CORS for each method in API Gateway + redeploy

More issues inside `issues-faced.md`

---

## 🧺 Cleanup Instructions (Avoid Billing)

After testing, delete:

* Hosted Zone
* Route 53 Health Checks
* DynamoDB tables
* Lambda functions
* S3 bucket
* API Gateway stages

See `aws-resources-to-delete.md` for checklist

---

## 📸 Screenshots

All 55+ screenshots are available in `/screenshots` and linked in steps above.

---

## 📚 Who Is This For?

* ✅ Beginners who want to understand AWS hands-on
* ✅ Students looking for a real project
* ✅ Interview candidates preparing system design
* ✅ Anyone learning Serverless & DR in cloud

---

## 🙌 Author & Credits

* Project by **[Vaibhav Parekh](https://www.linkedin.com/in/vaibhav-parekh08/)**
* Built with guidance from ChatGPT
* Thanks to the AWS community & docs
