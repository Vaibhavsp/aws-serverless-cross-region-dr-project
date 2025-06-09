# 🐞 Troubleshooting: Issues Faced During Development

This file includes real issues we encountered while building this serverless cross-region DR system and how we solved them. It is meant to help future developers avoid common mistakes and debug faster.

---

## ❌ Issue 1: Health Check Shows Unhealthy

### 🔍 Cause

Incorrect domain or missing path when setting up Route 53 Health Check. The full endpoint wasn't specified.

### ✅ Fix

Use the full domain only (not the full URL), and specify `/prod/read` as the path.

```
✔️ Domain: xyz.execute-api.ap-south-1.amazonaws.com
✔️ Path: /prod/read
```

---

## ❌ Issue 2: Lambda Function Returns 500 Error

### 🔍 Cause

The Lambda function expected a JSON body but the body was not parsed properly.

### ✅ Fix

Safely parse the body using:

```python
body = json.loads(event.get('body', '{}'))
```

Also, add try-except around it to catch errors.

---

## ❌ Issue 3: CORS Error in Browser

### 🔍 Cause

CORS (Cross-Origin Resource Sharing) was not enabled in API Gateway.

### ✅ Fix

* Go to each method (`GET`, `POST`) → Enable CORS
* Redeploy API to `prod`
* Also add headers manually in Lambda response:

```python
"Access-Control-Allow-Origin": "*"
```

---

## ❌ Issue 4: Health Check Flips from Healthy to Unhealthy Randomly

### 🔍 Cause

API Gateway stage was temporarily down or the URL was unreachable from health checker locations.

### ✅ Fix

* Double-check API URL
* Wait for DNS to propagate fully
* Use `curl` or Postman to verify API works from outside AWS

---

## ❌ Issue 5: Route 53 Not Failing Over

### 🔍 Cause

Incorrect failover record type or missing health check association.

### ✅ Fix

* Create **two A or CNAME records** with same name
* Mark one as **Primary** and attach health check
* Mark the second as **Secondary** (no health check needed)

---

## ❌ Issue 6: S3 Frontend Shows Access Denied

### 🔍 Cause

index.html file is not public or bucket permissions are incorrect.

### ✅ Fix

* Add a bucket policy to allow public read:

```json
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::your-bucket-name/*"
}
```

* Enable static website hosting in bucket properties

---

More issues will be added here as needed. PRs are welcome if you're replicating this project and face new problems.
