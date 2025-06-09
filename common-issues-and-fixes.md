### ❌ Health Check Shows Unhealthy
- Cause: You entered full URL or missed `/read` in path
- Fix: Use domain name like `xyz.execute-api.amazonaws.com` and add `/prod/read` in the "Path"

### ❌ Lambda Returns 500 Error
- Cause: JSON body parsing failed
- Fix: Use `json.loads(event.get('body', '{}'))` instead of `event['body']`

### ❌ CORS error on frontend
- Fix: Enable CORS for each method in API Gateway + redeploy

