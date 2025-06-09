```
This guide walks you through building the entire project from scratch using **AWS Console** (no code editors required at first).

You can follow this even if you’re new to AWS!
```

> 📸 I’ve included 50+ screenshots inside the `/screenshots` folder to make every step crystal clear.

---

### 🟢 Step 1: Create DynamoDB Table
- Go to DynamoDB in AWS Console → Create table
- Table name: `HighAvailabilityTable`
- Partition Key: `ItemId` (String)

![Step 1](https://github.com/user-attachments/assets/19267b00-b16c-4723-974a-f81ff480c444)

---

### 🟢 Step 2: Enable Global Table (Add Replica Region)
- Actions → Manage Global Tables → Add Region → Choose `ap-northeast-1`

![Step 2](https://github.com/user-attachments/assets/a46f477a-2693-46d1-a9c7-07a32c1e3a69)

---

### 🟡 Step 3: Create IAM Role for Lambda
- Go to IAM → Create Role → Choose Lambda
- Attach:
  - `AWSLambdaBasicExecutionRole`
  - `AmazonDynamoDBFullAccess`

![Step 3](https://github.com/user-attachments/assets/2078c500-3f6f-4b6f-8f44-8ec61b8ab5bc)

---

### 🔵 Step 4: Write Lambda Functions (read & write)
- Create two Lambda functions (`ReadFunction`, `WriteFunction`)
- Use `read_function.py` and `write_function.py` from `/lambda` folder

![Step 4](screenshots/04-lambda-write.png)

> Don't forget to assign the IAM Role you created earlier.

---

### 🟣 Step 5: Setup API Gateway
- Create a REST API
- Add `/read` (GET) and `/write` (POST) resources
- Integrate each with respective Lambda

![Step 5](screenshots/05-api-read-method.png)

---

### 🔴 Step 6: Enable CORS and Deploy API
- Enable CORS for both endpoints
- Deploy to a stage called `prod`
- Note down your endpoint URLs

![Step 6](https://github.com/user-attachments/assets/6b992614-e621-49a0-9de3-7fd51ad0b539)

![Step 7](https://github.com/user-attachments/assets/bfe6787e-76c9-4e0e-8aca-5d7cf042db63)

---

### 🟤 Step 7: Repeat Lambda + API in Second Region
- Repeat steps 4–6 in `ap-northeast-1` region

---

### 🟡 Step 8: Create Route 53 Hosted Zone + Health Checks
- Create Hosted Zone (like `vaibhav-lab.com`)
- Add 2 Health Checks pointing to `/read` endpoints

---

### 🔵 Step 9: Create Failover DNS Records
- Use routing policy = Failover
- Primary → primary API Gateway URL
- Secondary → secondary API Gateway URL
- Attach correct health checks

![Step 9](https://github.com/user-attachments/assets/9c77420f-79ad-4c1f-98b8-b924b50b3fa8)

![Step 10](https://github.com/user-attachments/assets/b4c84946-8929-4087-ab2d-46dc231c5a83)

---

### 🟢 Step 10: Build Frontend
- Use `frontend/index.html`
- Replace the API URL in JavaScript
- Upload to S3
- Enable Static Website Hosting
- Make index.html public
---

### ✅ Done! Your App:
- Shows a working frontend
- Can read/write data to DynamoDB
- Handles region failure automatically
