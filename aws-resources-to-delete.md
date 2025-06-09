# 🧹 AWS Cleanup Checklist – Avoid Unwanted Billing

If you’re done testing or demonstrating this project, make sure to clean up all resources that may incur charges.

---

## ✅ Services to Delete

### 🟥 Route 53

* [ ] Hosted Zone (custom domain, if created)
* [ ] Health Checks (primary and secondary)

### 🟦 DynamoDB

* [ ] Global Table (both regions)
* [ ] Check replicas and delete both

### 🟨 Lambda

* [ ] ReadFunction (Mumbai region)
* [ ] WriteFunction (Mumbai region)
* [ ] ReadFunction (Tokyo region)
* [ ] WriteFunction (Tokyo region)

### 🟧 API Gateway

* [ ] REST API in both regions
* [ ] Stages (like `prod`)

### 🟪 S3

* [ ] Frontend bucket
* [ ] Remove static website hosting
* [ ] Empty and delete bucket

### 🔐 IAM

* [ ] LambdaDynamoDBRole (if only used for this project)

---

## 💳 Billing Check

After deleting, go to:

* AWS Console → Billing → Free Tier Usage
* Set budgets and alarms if you're continuing to experiment

---

## 🧠 Tips

* Always tag resources with the project name
* Review Resource Groups before deleting
* Double-check across **both regions** used (e.g., ap-south-1 and ap-northeast-1)

You’re now safe to exit the lab 💼✅
