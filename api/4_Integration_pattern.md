# Phase 4: Integration Patterns

##  Goal
Understand how APIs connect systems together.  
By the end of this phase, you’ll be able to explain how apps talk to each other and what tools or techniques are used to make those integrations reliable and secure.

---

##  Direct API Integration (App ↔ API)

This is the most basic type of integration — one system directly calls another system’s API.

### Example (Healthcare)
A patient monitoring app directly calls your wearable device API:
```
GET /api/v1/patients/101/heart-rate
```
The API replies:
```json
{ "heartRate": 82, "timestamp": "2025-10-20T10:20:00Z" }
```

### When to use:
- Simple data fetch or update  
- Real-time needs (e.g., patient vitals on screen)

### Pros:
- Easy to set up  
- Real-time communication  

### Cons:
- Tight coupling (if one system changes, the other breaks)  
- Hard to scale when many clients call directly

---

##  Webhooks (Event-driven communication)

Instead of constantly calling the API to ask for updates, webhooks let the server notify another system when something happens.

### Example (Healthcare)
Your wearable device server sends a webhook when a patient’s heart rate crosses a threshold:
```
POST https://hospital-api.com/webhook/alerts
{
  "patientId": "101",
  "event": "HIGH_HEART_RATE",
  "value": 130
}
```
The hospital’s system receives the event instantly and alerts the nurse.

### When to use:
- When systems need real-time alerts  
- When you want to avoid polling APIs every few seconds

### Pros:
- Event-driven and efficient  
- Instant communication  
- Reduces unnecessary traffic  

### Cons:
- Requires public endpoint to receive data  
- Harder to test and debug  
- Must handle retries (in case webhook fails)

---

##  Middleware and API Gateways

When multiple systems connect, you don’t want every app calling every API directly.  
Instead, a middleware or API gateway sits in between.

### Example
You have APIs from:
- Patient Devices  
- Hospital Management  
- Billing System  
- Doctor App  

All requests go through one API Gateway.

**API Gateway Responsibilities:**
- Authentication & Authorization  
- Rate limiting & logging  
- Request routing (decides which service to call)  
- Load balancing (manages multiple backend instances)  
- Response transformation (formats data consistently)

**Middleware** can also:
- Validate incoming data  
- Add business logic (like “if patient heart rate > 120, trigger alert”)

### Why use it:
- Better control and security  
- Simplifies client logic  
- Centralized monitoring

---

##  Third-Party API Consumption

Sometimes, your system uses external APIs to add features instead of building them.

### Example (Healthcare)
- Use Google Maps API to locate patient homes for home visits  
- Use AWS S3 API to store wearable data images  
- Use Stripe API to handle payments for device subscriptions  

### Best Practices:
- Always store API keys securely (e.g., in environment variables)  
- Implement retry with exponential backoff if the third-party fails  
- Handle rate limits gracefully  
- Cache static data (like map results) to reduce calls

### Pros:
- Save development time  
- Leverage proven services  
- Easier maintenance  

### Cons:
- Dependency on external uptime  
- API changes may break integration  
- Cost may increase with API usage

---

##  Rate Limiting and Throttling

When too many requests come at once, it can overload your system.  
So APIs use rate limiting or throttling to protect themselves.

| Term | Meaning |
|------|----------|
| **Rate Limiting** | Maximum number of requests allowed per time window (e.g., 100 requests/minute) |
| **Throttling** | Slows down requests when nearing the limit instead of blocking them completely |

### Example:
Your hospital API allows:
```
1000 requests per minute per client
```
If a wearable sends 1500 requests:  
The extra 500 get delayed or rejected with:
```
HTTP 429 Too Many Requests
```

### Why important:
- Protects server from overload  
- Ensures fair usage  
- Prevents accidental or malicious traffic spikes

---

##  Integration Design Tips

1. Use consistent formats — JSON or XML  
2. Version your APIs — e.g., `/api/v1/`  
3. Retry safely — make APIs idempotent  
4. Secure communication — always HTTPS  
5. Add logs — for debugging and audit trails  
6. Use message queues (like RabbitMQ, Kafka) when data can be processed asynchronously  

---

##  Healthcare Example – All Together

Imagine your wearable system integrated with:
- Hospital app (Direct API) → fetches patient vitals  
- Alert system (Webhook) → sends event when abnormal reading  
- Billing system (API Gateway) → handles authentication and routing  
- AWS S3 (Third-party API) → stores device images  
- Rate limiting → avoids too many data uploads from devices  

 Together, this creates a robust, scalable integration setup.

---

##  Interview Summary Answer

I use direct API calls for real-time data, webhooks for event-driven updates, and API gateways for managing security and routing.  
For external services, I handle rate limits and retries, and I secure all communications using HTTPS and token-based auth.  
This ensures reliable and scalable integrations between systems.

---

##  Common Interview Questions (with short answers)

| Question | Answer |
|-----------|---------|
| What is a webhook? | Server sends real-time notification to another system when an event occurs. |
| Difference between polling and webhook? | Polling repeatedly asks; webhook pushes automatically. |
| What is an API Gateway? | A single entry point that handles routing, auth, rate limits, and monitoring. |
| What is middleware? | Software that connects APIs and adds logic between them. |
| How do you handle third-party API failures? | Use retries, caching, and fallback mechanisms. |
| What is rate limiting? | Restricting how many requests can be made in a given time. |
| Why is throttling used? | To slow down or delay requests instead of rejecting them outright. |
| How do you secure integrations? | Use HTTPS, auth tokens, and secret management. |
| Example of healthcare API integration? | Wearable device sending vitals to hospital system with alerts via webhooks. |

---

End of Phase 4 Notes
