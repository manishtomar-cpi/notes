# Phase 5: Error Handling, Monitoring & Versioning

##  Goal  
Understand how APIs are maintained, monitored, and improved after being developed.  
Learn how to make APIs more reliable, traceable, and easy to evolve over time.

---

##  Error Handling (4xx, 5xx, and Custom Messages)

Good APIs don’t just fail — they fail gracefully.  
Error handling helps the client understand what went wrong and how to fix it.

---

### 🔹 Common HTTP Status Codes

| Code | Meaning | Example Use |
|------|----------|--------------|
| **200 OK** | Successful request | Data fetched successfully |
| **201 Created** | Resource created | New patient added |
| **204 No Content** | Success but no data to return | After deleting a record |
| **400 Bad Request** | Client sent invalid data | Missing patient ID |
| **401 Unauthorized** | No valid credentials | Missing/invalid token |
| **403 Forbidden** | Authenticated but not allowed | Nurse trying to delete a patient |
| **404 Not Found** | Resource doesn’t exist | Patient ID not found |
| **409 Conflict** | Duplicate record or version mismatch | Same patient data submitted twice |
| **429 Too Many Requests** | Rate limit exceeded | Too many heart-rate uploads |
| **500 Internal Server Error** | Bug or issue in backend code | Server crashed |
| **503 Service Unavailable** | API temporarily down | Maintenance mode |

---

### 🔹 Error Response Format (Best Practice)

```json
{
  "error": {
    "code": 404,
    "message": "Patient record not found",
    "details": "No patient exists with ID 101"
  }
}
```

---

### 🔹 Custom Error Codes

```json
{
  "error": {
    "code": "PATIENT_NOT_FOUND",
    "http_status": 404,
    "message": "No patient exists with ID 101"
  }
}
```

 Makes debugging faster and easier for client developers.

---

### 🔹 Healthcare Example

If a wearable device tries to send an update for a non-existent patient:

```
POST /api/v1/patients/999/vitals
```
Response:
```json
{
  "error": {
    "code": 404,
    "message": "Patient ID 999 not found"
  }
}
```

---

##  Logging and Monitoring

Logs and monitors help you understand API health, trace issues, and measure performance.

### 🔹 Logging

**Best Practices:**
- Log every request (method, path, status code)
- Log errors with timestamps
- Mask sensitive data (like passwords)
- Use correlation IDs to trace requests

**Example Log:**
```
[2025-10-20T11:30:00Z] INFO: GET /api/v1/patients/101 -> 200 OK
[2025-10-20T11:31:00Z] ERROR: POST /api/v1/patients/999/vitals -> 404 Not Found
```

---

### 🔹 Monitoring Tools

| Tool | Purpose |
|------|----------|
| **Postman Monitors** | Schedule test runs and monitor responses |
| **Swagger (OpenAPI)** | Interactive API documentation |
| **AWS API Gateway Logs** | Tracks usage and latency in AWS APIs |
| **Datadog / New Relic / ELK** | For real-time performance and error tracking |
| **CloudWatch (AWS)** | Stores logs, metrics, and alarms |

**Interview Answer:**  
I use structured logging, Postman monitors, and AWS CloudWatch to track API usage, latency, and failures.

---

##  API Versioning Strategies

Versioning allows you to make improvements to your API without breaking existing clients.

### 🔹 Why Versioning Is Needed

- You may change the response format  
- Add or remove parameters  
- Update validation rules  

Versioning ensures old clients still work while new clients use updated logic.

---

### 🔹 Common Versioning Methods

| Method | Example | Notes |
|--------|----------|-------|
| **URI Versioning** | `/api/v1/patients` | Most common, easy to read |
| **Header Versioning** | `Accept: application/vnd.api.v2+json` | Keeps URL clean but harder to manage |
| **Query Parameter Versioning** | `/api/patients?version=2` | Simple, but not RESTful |
| **Domain-based Versioning** | `v2.api.example.com` | Used by large enterprises |

---

### 🔹 Example

**Old version:**
```
GET /api/v1/patients/101
Response:
{
  "name": "John Doe"
}
```

**New version:**
```
GET /api/v2/patients/101
Response:
{
  "fullName": "John Doe",
  "age": 45
}
```

 Older clients still use `/v1`, new ones use `/v2`.

---

##  API Documentation and Testing

APIs are only as good as their documentation and tests.

### 🔹 Documentation

Use tools like:
- **Swagger (OpenAPI)** → auto-generate docs from code  
- **Postman Collections** → share request/response examples  
- **Markdown Docs** → for internal technical teams  

Each endpoint should include:
- URL  
- Method (GET, POST, etc.)  
- Parameters  
- Request example  
- Response example  
- Error cases  

**Example (Swagger snippet):**
```yaml
paths:
  /patients/{id}:
    get:
      summary: Get patient details
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Patient found
        '404':
          description: Patient not found
```

---

### 🔹 Testing APIs

Use tools like:
- **Postman** (manual testing)
- **Newman** (automated Postman tests)
- **Jest / Mocha** (for automated unit tests)
- **k6 / JMeter** (for load testing)

**Interview Answer:**  
I document all endpoints using Swagger and verify them with Postman and automated integration tests.

---

##  Outcome

You can now confidently explain:
- How to design clear, meaningful errors  
- How to monitor and log API activity  
- How to safely version APIs  
- How to document and test them like a professional developer

---

##  Common Interview Questions

| Question | Answer |
|-----------|---------|
| What’s the difference between 4xx and 5xx errors? | 4xx = client-side issue, 5xx = server-side issue. |
| What is 409 Conflict? | When a request conflicts with existing data (duplicate record). |
| Why is versioning important? | It allows updates without breaking existing clients. |
| How do you version APIs? | Using paths like `/api/v1/`, or headers. |
| What’s the role of logging? | Track and debug API behavior, detect errors early. |
| What is rate limiting related error? | 429 Too Many Requests. |
| What’s your monitoring setup? | Use CloudWatch for logs, Postman for tests, and alerts for failures. |
| What’s Swagger used for? | API documentation and testing. |
| What’s in a good error message? | Code, message, and details field with context. |
| Example of a healthcare error case? | Sending vitals for a patient ID that doesn’t exist returns 404. |

---

End of Phase 5 Notes
