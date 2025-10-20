# Phase 2: RESTful Design Principles

## 1. What is REST?

REST (Representational State Transfer) is a set of design principles that make web APIs simple, scalable, and easy to use.  
It uses standard HTTP methods (GET, POST, PUT, DELETE) and communicates mostly in JSON.

In simple terms: REST defines how systems should communicate using standard web rules.

Example (Healthcare):  
Your wearable sends patient vitals to `/api/v1/patients/101/readings` — this is a RESTful API request.

**Interview Question:**  
Q: What does REST stand for?  
A: Representational State Transfer — it defines rules for designing scalable APIs.

---

## 2. REST Principles

| Principle                 | Meaning                                                                    | Example                                                    |
| ------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Statelessness             | Each request is independent; the server doesn’t store client session data. | Every API call must include authentication (like a token). |
| Client-Server             | Client and server are separate; one focuses on UI, the other on data.      | Mobile app = client, Hospital DB = server.                 |
| Cacheable                 | Responses can be stored (cached) to improve speed.                         | Doctor dashboard caches patient data for quick access.     |
| Uniform Interface         | API follows consistent structure and naming.                               | Always use `/patients`, not `/getAllPatients`.             |
| Layered System            | API can include layers (auth, load balancer, DB) without affecting client. | Server hides internal structure from wearable devices.     |
| Code on Demand (Optional) | Server can send code to client (rarely used).                              | JavaScript snippets returned to web client.                |

**Interview Question:**  
Q: What does “stateless” mean in REST?  
A: Each request is independent — the server doesn’t remember previous ones.

---

## 3. Resource-Based URLs

APIs should represent resources (nouns), not actions (verbs). Each resource should have its own endpoint.

**Good Example:**

```
GET /patients
GET /patients/101
POST /patients
PUT /patients/101
DELETE /patients/101
```

**Bad Example:**

```
GET /getAllPatients
POST /addNewPatient
DELETE /removePatient
```

**Why?**  
REST APIs should describe what you want (the resource), not what action to perform.

---

## 4. URL Naming Conventions

| Guideline                    | Example                                     |
| ---------------------------- | ------------------------------------------- |
| Use lowercase letters        | `/patients/101/readings`                    |
| Use hyphens, not underscores | `/patient-reports` ✅ `/patient_reports` ❌ |
| Use plural nouns             | `/patients` not `/patient`                  |
| Group related resources      | `/patients/101/reports/5`                   |
| Version your API             | `/api/v1/patients`                          |

**Interview Question:**  
Q: Why do we use plural nouns in REST URLs?  
A: To represent collections of resources (like all patients).

---

## 5. Idempotency and Safe Methods

| Method | Safe? | Idempotent? | Meaning                                             |
| ------ | ----- | ----------- | --------------------------------------------------- |
| GET    | ✅    | ✅          | Fetching doesn’t change data.                       |
| POST   | ❌    | ❌          | Creates new data — not safe.                        |
| PUT    | ❌    | ✅          | Replacing same data gives same result.              |
| PATCH  | ❌    | ✅          | Updating same field repeatedly gives same result.   |
| DELETE | ❌    | ✅          | Deleting same resource multiple times = same state. |

**Interview Question:**  
Q: What does idempotent mean?  
A: Performing the same request multiple times gives the same outcome.

---

## 6. Detailed Explanation: Idempotency

Idempotent means performing the same operation multiple times has the same effect as performing it once.  
It does not mean "no data ever changes"; it means "after the first change, repeating the same request keeps the data in the same final state."

Example (Healthcare):

Initial data:

```json
{ "id": 101, "heartRate": 90 }
```

**PUT Request:**

```
PUT /api/v1/patients/101
{ "heartRate": 92 }
```

- 1st call → updates heartRate from 90 → 92
- 2nd call → data already 92 → no further change  
  Final state remains 92.  
  So, although the first call changed data, repeating it keeps the same result — this is **idempotent**.

POST vs PUT:

| Method | Behavior                                 | Idempotent? |
| ------ | ---------------------------------------- | ----------- |
| POST   | Creates new record each time             | ❌          |
| PUT    | Sets data to one fixed value             | ✅          |
| PATCH  | Updates partial data to same final value | ✅          |
| DELETE | Removes record and stays deleted         | ✅          |

Analogy – Light Switch:

- **POST** → Adds new bulbs each time (new data).
- **PUT/PATCH** → Sets brightness to a level (changes once, then stays same).
- **DELETE** → Removes bulb (once gone, stays gone).

Why PUT and PATCH Are Idempotent:

They may change the data on the first call, but not afterward.  
Idempotency focuses on the **final state** — if repeated requests leave the data unchanged beyond the first, it’s idempotent.

Example (Thermostat Analogy):

```
PUT /api/v1/ward1/temperature
{ "value": 25 }
```

- 1st → sets temp to 25°C
- 2nd → already 25°C → no change  
  Final result same → idempotent.

---

## 7. Why Idempotency Is Important

1. **Networks Are Unreliable**  
   APIs can experience timeouts or retries. Without idempotency, a retry could create duplicates or errors.

2. **Example (Healthcare)**

   - Non-idempotent: `POST /payments` → ₹500 payment might be added twice if retried.
   - Idempotent: `PUT /balance` → Balance stays ₹500 no matter how many retries.

3. **Prevents Data Duplication**  
   Protects systems like hospital billing or patient monitoring from double entries.

4. **Safe Retries**  
   Clients, IoT devices, or gateways can safely retry requests without fear of corrupting data.

5. **Consistency and Predictability**  
   Developers know what will happen even if the same call is made multiple times.

**Interview Answer Example:**  
Idempotency ensures that repeating the same request doesn’t cause unexpected changes. It’s important in healthcare systems where retries can happen due to weak networks. It keeps patient data and transactions consistent.

---

## 8. REST Versioning (Detailed Explanation)

When APIs evolve, older clients should still work. Versioning means maintaining multiple API versions for backward compatibility.

Example (Healthcare):

**Version 1:**

```
GET /api/v1/patients/101
{ "name": "John", "age": 60 }
```

**Version 2:**

```
GET /api/v2/patients/101
{ "name": "John", "age": 60, "bloodGroup": "O+" }
```

Older apps still use v1, while new ones use v2.

| Type                       | Example                                     | Use Case                  |
| -------------------------- | ------------------------------------------- | ------------------------- |
| URI Versioning             | `/api/v1/patients`                          | Most common and clear     |
| Header Versioning          | `Accept: application/vnd.healthapp.v2+json` | Internal APIs             |
| Query Parameter Versioning | `/patients?version=2`                       | Simple but less preferred |
| Content Negotiation        | Based on headers                            | Flexible communication    |

Best Practices:

1. Start with `/v1` and never expose unversioned endpoints.
2. Maintain backward compatibility and announce deprecations early.
3. Document version changes clearly.
4. Use semantic versioning when needed.
5. Keep logic separate for each version.

Interview Answer Example:  
Versioning prevents breaking old clients and allows introducing new features safely.

---

## 9. Summary Table

| Concept    | Description                      | Example                 |
| ---------- | -------------------------------- | ----------------------- |
| REST       | API design principle             | JSON over HTTP          |
| Stateless  | Server doesn’t store client info | Each request has token  |
| Resource   | Entity represented by URL        | `/patients`, `/devices` |
| Idempotent | Repeating request = same result  | PUT, DELETE             |
| Versioning | Maintain API compatibility       | `/api/v1`               |

---

### Q: How do you make your API idempotent?

**Simple Answer:**  
I design APIs so that repeating the same request doesn’t change the final state.  
I use correct HTTP methods, handle retries safely, and prevent duplicate operations using techniques like idempotency keys, upserts, and conditional updates.

---

###  Steps to Make an API Idempotent

1. **Use correct HTTP methods**

   - **GET** → Read-only (safe).
   - **PUT/PATCH** → Update or set a specific resource (repeating gives same result).
   - **DELETE** → Removing the same resource multiple times changes nothing.
   - **POST** → Normally not idempotent, but can be made idempotent with a key.

2. **Use an Idempotency Key**

   - Client sends a header like:
     ```
     Idempotency-Key: <unique-id>
     ```
   - Server stores the first response for that key.  
     If the same key is received again, it returns the stored response instead of processing it again.

3. **Set state instead of changing state**

   - Example: Use “set status to active” instead of “toggle status.”
   - This ensures the same input always leads to the same final result.

4. **Use unique constraints or upserts**

   - Use database constraints (like unique patient ID + timestamp).
   - If the same request repeats, the record is not duplicated.

5. **Support conditional updates**

   - Use headers like:
     ```
     If-Match: <ETag>
     ```
   - This ensures the update only happens if the version matches, avoiding conflicts.

6. **Consistent delete behavior**

   - Deleting a resource that’s already deleted should return the same response (like 204 No Content).
   - It should not fail or change any data again.

7. **Control external side effects**
   - For actions like sending an email or processing a payment, use the same idempotency key or a record log so retries don’t trigger the same action twice.

---

###  Healthcare Example

```http
PUT /api/v1/patients/101/status
{
  "status": "discharged"
}
Idempotency-Key: 12345-abcde



- 1st request → updates status to “discharged.”
- 2nd request (same key) → returns same success response, no new change.
 Final system state remains consistent.

---

###  Interview Summary Answer

> I make APIs idempotent by using proper HTTP methods, idempotency keys, upserts, and conditional headers.
> This ensures that even if a client retries due to a network issue, the final state of data remains consistent.


## 10. Interview Questions

1. What does REST mean?
   Representational State Transfer — a design principle for scalable APIs.

2. What are REST’s main principles?
   Stateless, client-server, cacheable, uniform interface, layered system, optional code on demand.

3. What’s the difference between stateless and stateful?
   Stateless APIs don’t store session data; stateful ones do.

4. What is an example of a REST endpoint?
   `/api/v1/patients/101`

5. Why should REST APIs use nouns instead of verbs?
   To represent resources clearly (like `/patients` instead of `/getPatients`).

6. What is an idempotent API call?
   A request that can be repeated without changing the final state of data.

7. Why is idempotency important?
   To prevent duplication and maintain data integrity during retries.

8. Why use versioning in REST APIs?
   To maintain backward compatibility.

9. Which format is most used in REST APIs?
   JSON.

10. Give one healthcare example of REST.
    `/api/v1/patients/101/readings` — gets a patient’s wearable data.

---

End of Phase 2 Notes
```
