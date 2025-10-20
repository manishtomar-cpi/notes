# Phase 6: Advanced Topics (GraphQL)

## Goal  
Understand how GraphQL differs from REST and how it can simplify data fetching for complex systems like healthcare wearables.

---

##  What is GraphQL?

GraphQL is a **query language for APIs** developed by Facebook.  
It allows the **client to decide exactly what data** it needs — nothing more, nothing less.  
Unlike REST (which has many endpoints), GraphQL usually has **one single endpoint**, for example:
```
POST /graphql
```
The client sends a **query** describing what data it wants.

---

##  REST vs GraphQL Example

### Using REST:
To get patient info and heart rate data:
```
GET /api/v1/patients/101
GET /api/v1/patients/101/heart-rate
```
You call **two endpoints**, and may get extra unwanted fields.

### Using GraphQL:
```
{
  patient(id: 101) {
    name
    age
    heartRate {
      value
      timestamp
    }
  }
}
```
Response:
```json
{
  "data": {
    "patient": {
      "name": "John Doe",
      "age": 45,
      "heartRate": {
        "value": 88,
        "timestamp": "2025-10-20T11:45:00Z"
      }
    }
  }
}
```
 Only the requested data is returned — in one request.

---

##  The Meaning of “Single Endpoint” in GraphQL

In REST, we have many endpoints for different data sources, for example:

| Function | REST Endpoint |
|-----------|----------------|
| Get patient info | `GET /api/v1/patients/101` |
| Get vitals | `GET /api/v1/patients/101/heart-rate` |
| Get assigned doctor | `GET /api/v1/patients/101/doctor` |

GraphQL simplifies this using just **one endpoint**:
```
POST /graphql
```

This endpoint accepts **queries** from the client, processes them, and returns exactly what was requested.

---

##  What Happens Inside `/graphql`

| Step | What Happens |
|------|---------------|
| 1️⃣ **Request received** | `/graphql` endpoint gets the query. |
| 2️⃣ **Parsed & validated** | GraphQL checks if requested fields exist in the schema. |
| 3️⃣ **Resolver functions run** | Each field (like `patient`, `doctor`, `heartRate`) maps to backend logic or DB calls. |
| 4️⃣ **Response combined** | Data from resolvers is merged into one response. |
| 5️⃣ **Response sent** | API returns data exactly as the client requested. |

---

### Example Flow

#### Request sent to:
```
POST /graphql
```
#### Request body:
```graphql
{
  patient(id: 101) {
    name
    heartRate {
      value
    }
  }
}
```
#### Response returned:
```json
{
  "data": {
    "patient": {
      "name": "John Doe",
      "heartRate": {
        "value": 88
      }
    }
  }
}
```
 Only name and heart rate value returned — no extra data.

---

##  Key Concepts in GraphQL

| Term | Meaning | Example |
|------|----------|---------|
| **Schema** | Blueprint of all data types and queries available | Defines what “patient” or “doctor” means |
| **Type** | Data structure (like a class) | `type Patient { id: ID, name: String, age: Int }` |
| **Query** | Read operation (fetch data) | `patient(id: 101) { name age }` |
| **Mutation** | Write operation (change data) | `updatePatient(id:101, age:46)` |
| **Subscription** | Real-time updates | `heartRateChanged(patientId:101)` |
| **Resolver** | Function that fetches actual data | Fetches from DB, API, etc. |
| **Single Endpoint** | `/graphql` handles all queries/mutations | Unlike REST’s many endpoints |

---

##  Example Schema and Query (Healthcare)

Schema:
```graphql
type Patient {
  id: ID
  name: String
  age: Int
  doctor: Doctor
  heartRate: HeartRate
}

type Doctor {
  id: ID
  name: String
  department: String
}

type HeartRate {
  value: Int
  timestamp: String
}

type Query {
  patient(id: ID!): Patient
}
```

Client Query:
```graphql
{
  patient(id: 101) {
    name
    age
    doctor { name }
    heartRate { value }
  }
}
```

Response:
```json
{
  "data": {
    "patient": {
      "name": "John Doe",
      "age": 45,
      "doctor": { "name": "Dr. Smith" },
      "heartRate": { "value": 92 }
    }
  }
}
```

---

##  Mutations (For Updating Data)

Example mutation:
```graphql
mutation {
  updateHeartRate(patientId: 101, value: 95) {
    patientId
    newValue
  }
}
```
Response:
```json
{
  "data": {
    "updateHeartRate": {
      "patientId": 101,
      "newValue": 95
    }
  }
}
```

---

##  Subscriptions (Real-Time Updates)

Example:
```graphql
subscription {
  heartRateChanged(patientId: 101) {
    value
    timestamp
  }
}
```
 The client receives continuous updates — useful for live dashboards.

---

##  Why Use GraphQL

| Benefit | Explanation |
|----------|--------------|
| **Single endpoint** | One `/graphql` endpoint instead of many REST URLs |
| **Client control** | Client asks for only the needed fields |
| **Less data transfer** | Saves bandwidth — good for wearables |
| **Fewer requests** | Combine data from multiple sources |
| **Strongly typed schema** | Ensures predictable structure |
| **Auto-documentation** | GraphiQL tools show available queries automatically |

---

## When to Use GraphQL

| Situation | Is GraphQL good? | Why |
|------------|-----------------|-----|
| **Mobile or wearable app** | ✅ Yes | Reduces over-fetching |
| **Complex UI** | ✅ Yes | Combines multiple data sources |
| **Simple CRUD system** | ❌ No | REST is simpler |
| **Need caching (CDN)** | ❌ No | GraphQL has one endpoint, harder to cache |
| **Bulk updates** | ⚠️ Maybe | REST often better |

---

##  Drawbacks of GraphQL

| Drawback | Description |
|-----------|--------------|
| **Complex setup** | Needs schema and resolver configuration |
| **Caching issues** | Harder to cache because only one endpoint |
| **Overload risk** | Clients can query too much data unless limited |
| **Security** | Must validate fields and prevent deep queries |
| **Not ideal for simple CRUD** | REST is easier |

---

##  REST vs GraphQL Summary

| Feature | REST | GraphQL |
|----------|------|----------|
| **Data fetching** | Fixed endpoints | Single flexible endpoint |
| **Over-fetching** | Common | Avoided |
| **Number of requests** | Often multiple | Usually one |
| **Schema** | Not required | Strongly typed |
| **Caching** | Easy | Harder |
| **Complexity** | Simple | More setup |
| **Best use case** | Simple CRUD | Complex data relationships |

---

##  Interview Answer

> GraphQL is a query language for APIs that allows the client to specify exactly what data it needs.  
> It’s useful in complex or data-heavy systems like healthcare dashboards or wearable integrations, where the client may need patient info, doctor details, and vitals in one call.  
> I’d use REST for simple CRUD APIs, and GraphQL for flexible, real-time, and data-driven applications.

---

End of Phase 6 (GraphQL) Notes
