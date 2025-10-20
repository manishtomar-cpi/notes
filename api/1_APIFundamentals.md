# Phase 1: API Fundamentals

## 1. What is an API?

API (Application Programming Interface) is a way for two systems or applications to talk to each other and exchange data.

Think of it as a messenger — it takes a request from one system, sends it to another, and brings back a response.

Example (Healthcare):  
Your wearable heart-rate device collects your pulse data and sends it to a cloud server through an API.  
The mobile app then calls another API to fetch and display that heart rate to you.

So:  
- Device → API → Cloud (to send data)  
- App → API → Cloud (to get data)

**Interview Question:**  
Q: What is an API?  
A: An API allows different systems or applications to communicate and share data using defined rules.

---

## 2. REST vs SOAP

| Feature | REST | SOAP |
|----------|------|------|
| Full Form | Representational State Transfer | Simple Object Access Protocol |
| Data Format | JSON (mainly) | XML |
| Speed | Faster, lightweight | Slower, heavy |
| Flexibility | Easy to use | Strict format |
| Used in | Modern web & mobile apps | Older enterprise systems |

Example:  
- REST API → wearable sends JSON data:  
  ```json
  { "heartRate": 85 }
  ```
- SOAP API → same data in XML:  
  ```xml
  <heartRate>85</heartRate>
  ```

Most modern systems (like Fitbit, Apple Health, or your patient monitoring device) use REST APIs.

**Interview Question:**  
Q: What’s the difference between REST and SOAP?  
A: REST is lightweight and uses JSON; SOAP is heavier and uses XML with strict structure.

---

## 3. HTTP Methods

APIs use HTTP methods to define what action they perform.

| Method | Purpose | Example (Healthcare) |
|--------|----------|----------------------|
| GET | Retrieve data | Get all patients’ vitals |
| POST | Add new data | Send new heart rate reading |
| PUT | Update existing data completely | Update full patient record |
| PATCH | Update partial data | Update only blood pressure value |
| DELETE | Remove data | Delete patient’s old report |

Example Request (POST):
```json
POST /patients
{
  "name": "John Doe",
  "heartRate": 88
}
```

---

## 4. Request & Response Structure

Every API call has two parts:  
1. Request – what the client sends  
2. Response – what the server returns

Example:  
- Request → wearable device sends new heart rate  
- Response → server replies “Data saved successfully”

| Part | Example |
|------|----------|
| Request URL | https://api.healthserver.com/patients |
| Headers | Contain metadata (like content-type, auth token) |
| Body | The actual data you send (JSON, XML, etc.) |
| Response Code | 200 OK, 404 Not Found, 500 Server Error |
| Response Body | Data or message returned from server |

Example Response:
```json
{
  "message": "Data saved successfully",
  "status": 200
}
```

**Interview Question:**  
Q: What are the main parts of an HTTP request?  
A: URL, headers, method, and body.

---

## 5. Endpoints, Base URLs, and Routes

- Base URL: Main address of the API  
  Example → https://api.healthapp.com/  
- Endpoint: Specific path to a resource  
  Example → /patients, /devices, /reports  
- Route: Full path (Base URL + Endpoint)  
  Example → https://api.healthapp.com/patients/123

**Interview Question:**  
Q: What’s an API endpoint?  
A: It’s a specific path in an API used to access or modify data, like /patients/123.

---

## 6. Example: Patient Monitoring Flow

Scenario:  
Your wearable sends heart rate data to the hospital server.

Steps:  
1. Device sends a POST request to /api/v1/patients/101/readings
   ```json
   { "heartRate": 90, "timestamp": "2025-10-20T12:30:00Z" }
   ```
2. Server saves it and returns:
   ```json
   { "status": "success", "message": "Reading stored" }
   ```
3. The doctor’s dashboard uses a GET request to /api/v1/patients/101/readings  
   → It fetches all stored readings to display graphs.

That’s API integration — data flows smoothly from devices to apps through structured requests and responses.

---

## 7. Common HTTP Status Codes

| Code | Meaning | Example |
|------|----------|----------|
| 200 OK | Request successful | Data fetched or saved |
| 201 Created | New resource created | New patient added |
| 400 Bad Request | Client sent invalid data | Missing required field |
| 401 Unauthorized | Authentication failed | Wrong or missing token |
| 404 Not Found | Resource doesn’t exist | Patient ID not found |
| 500 Internal Server Error | Server crashed or failed | Database issue |

**Interview Question:**  
Q: What does HTTP 201 mean?  
A: It means a new resource was successfully created.

---

## 8. Summary Table

| Concept | Description | Example |
|----------|--------------|----------|
| API | Interface for system communication | Device ↔ Cloud ↔ App |
| REST | Lightweight, JSON-based | Used in wearable APIs |
| HTTP Methods | Define operation type | GET, POST, PUT, PATCH, DELETE |
| Endpoint | Specific API path | /patients/101/readings |
| Response Code | Shows API result | 200 OK, 404 Not Found |

---

## 9. HTTP Status Code Categories

| Code Range | Category | Meaning |
|-------------|-----------|---------|
| **1xx** | Informational | Request received, continuing process |
| **2xx** | Success | Request successfully processed |
| **3xx** | Redirection | The client must take additional action to complete the request |
| **4xx** | Client Error | The problem is from the client side (bad request, unauthorized, etc.) |
| **5xx** | Server Error | The problem is from the server side |


## 9. Interview Questions (with short answers)

1. What is an API?  
   A set of rules that allows two systems to communicate.

2. What’s the difference between REST and SOAP?  
   REST uses JSON and is lightweight; SOAP uses XML and is heavier.

3. What are common HTTP methods?  
   GET, POST, PUT, PATCH, DELETE.

4. What is an endpoint in an API?  
   The specific path where a resource is accessed or modified.

5. What is the purpose of HTTP status codes?  
   To tell if an API request succeeded or failed.

6. What’s the difference between a request and a response?  
   Request is sent by the client; response is returned by the server.

7. What happens when you send a POST request?  
   New data is created on the server.

8. What is a base URL?  
   The main address of the API (e.g., https://api.server.com).

9. Why are APIs important in healthcare systems?  
   They connect wearables, apps, and hospital databases for real-time monitoring.

10. What format is commonly used in REST APIs?  
   JSON (JavaScript Object Notation).

---

End of Phase 1 Notes
