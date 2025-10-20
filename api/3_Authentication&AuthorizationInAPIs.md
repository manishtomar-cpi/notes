# Phase 3: Authentication & Authorization in APIs

## 1. Authentication vs Authorization

- **Authentication (AuthN)** = Who are you? (identity verification)  
- **Authorization (AuthZ)** = What are you allowed to do? (permissions)

**Healthcare Example:**  
AuthN: Is this Dr. Smith?  
AuthZ: Can Dr. Smith view patient 101’s ECG?

**Interview Answer:**  
AuthN checks identity; AuthZ checks permissions.

---

## 2. Common Authentication Methods

| Method | Description | Example |
|--------|--------------|----------|
| **API Key** | Static key sent in header; identifies the app, not the user. | `x-api-key: abc123` |
| **Basic Auth** | Sends username and password encoded in base64. | `Authorization: Basic dXNlcjpwYXNz` |
| **Bearer Token** | Short-lived token used after login; passed in header. | `Authorization: Bearer <token>` |
| **JWT (JSON Web Token)** | Signed token that carries user info securely. | `Authorization: Bearer <jwt>` |

---

## 3. JWT (JSON Web Token)

### What is JWT?

JWT (JSON Web Token) is a compact, digitally signed token used to securely transfer user data between a client and server.  
It proves identity and permissions without needing to store session data.

Think of it as a **digital ID card** that says who you are and what you can do.

---

### Structure of a JWT

A JWT has three parts separated by dots:

```
xxxxx.yyyyy.zzzzz
```

- **Header** → how the token is signed  
- **Payload** → user data (claims)  
- **Signature** → verifies it wasn’t changed

Example:
```
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMDEiLCJyb2xlIjoiZG9jdG9yIiwic2NvcGUiOiJyZWFkOnBhdGllbnRzIiwiZXhwIjoxNzM2NTUwMDB9.
U2lnbmVkV2l0aFByaXZhdGVLZXk
```

---

### 1. Header

Defines the algorithm and type:

```json
{
  "alg": "RS256",
  "typ": "JWT"
}
```

- `alg`: Algorithm used for signing (e.g., HS256 or RS256)  
- `typ`: Type of token (JWT)

Encoded with Base64 — readable, not encrypted.

---

### 2. Payload (Claims)

Contains the actual user data and permissions:

```json
{
  "sub": "101",
  "name": "Dr. Smith",
  "role": "doctor",
  "scope": "read:patients",
  "ward_id": "C3",
  "exp": 1736550000
}
```

**Claim Types:**  
- **Registered Claims**: `iss`, `sub`, `exp`, `iat`  
- **Public Claims**: `role`, `scope`, etc.  
- **Private Claims**: Internal custom data like `ward_id`

---

### 3. Signature

Ensures the token has not been changed.

```
HMACSHA256(base64(header) + "." + base64(payload), secret)
```

For RS256 (asymmetric key):
```
RSASHA256(base64(header) + "." + base64(payload), private_key)
```

The server verifies the signature with the secret or public key.

---

### Why JWT Is Useful

1. **Stateless authentication** — server doesn’t need to store sessions.  
2. **Portable** — used across microservices.  
3. **Fast** — quick verification using signature.  
4. **Secure** — tampering is detected automatically.

---

### Important: JWT Is Signed, Not Encrypted

- Anyone can **decode** a JWT.  
- Only the issuer can **sign** it correctly.  
- If you need data privacy, encrypt the content (use JWE).

---

### Example Flow (Healthcare)

1. Doctor logs in with credentials.  
2. Auth server verifies credentials and issues JWT:
   ```json
   {
     "sub": "101",
     "role": "doctor",
     "scope": "read:patients write:reports",
     "exp": 1736550000
   }
   ```
3. Doctor app calls:
   ```
   GET /api/v1/patients
   Authorization: Bearer <JWT>
   ```
4. API verifies signature, expiry, and scope.  
5. Access granted if valid.

---

### Short Interview Q&A on JWT

| Question | Answer |
|-----------|---------|
| What is JWT? | A signed token used for stateless authentication. |
| What are its 3 parts? | Header, Payload, Signature. |
| What’s in the payload? | Claims like user ID, role, expiry. |
| Is JWT encrypted? | No, it’s base64-encoded and signed. |
| How is JWT verified? | Using the secret or public key to check signature. |
| What is `exp`? | Expiry time in seconds (Unix time). |
| Access vs Refresh token? | Access = short-lived; Refresh = long-lived for new access tokens. |

---

## 4. OAuth 2.0

A framework that defines how applications get tokens securely.

**Common Flows:**  
- **Authorization Code (with PKCE)** → for web/mobile apps.  
- **Client Credentials** → for server-to-server calls.  
- **Refresh Token** → for obtaining new access tokens without re-login.

**Interview Answer:**  
Use Authorization Code + PKCE for public clients, and Client Credentials for backend services.

---

## 5. OpenID Connect (OIDC)

Built on OAuth 2.0 to provide **authentication** in addition to authorization.

- Returns an **ID Token** (a JWT) with user details like name and email.  
- Used when the API needs to know the user’s identity.

**Interview Answer:**  
OAuth handles authorization, OIDC handles authentication.

---

## 6. Authorization Models

| Model | Description | Example |
|--------|--------------|----------|
| **Scopes** | Defines allowed actions in token | `read:patients` |
| **RBAC** | Role-Based Access Control | `doctor`, `nurse`, `admin` |
| **ABAC** | Attribute-Based Access | rules like `ward_id = C3` |
| **Row/Field-Level** | Restrict or mask data by ownership | Doctor sees only their patients |

---

## 7. Sessions vs Tokens

| Sessions | Tokens |
|-----------|----------|
| Server stores session | Server stores nothing |
| Suitable for browser apps | Best for APIs and mobile apps |
| Hard to scale | Easy to scale |

**Interview Answer:**  
Tokens are stateless and easier for distributed systems.

---

## 8. Transport Security (HTTPS)

Always use HTTPS to protect credentials and tokens.  
Use secure headers and key rotation.

**Interview Answer:**  
Never send tokens over HTTP; HTTPS is mandatory.

---

## 9. Token Protection and Rotation

- Store tokens securely (not in code).  
- Use short lifetimes (`exp`) and rotate frequently.  
- Revoke tokens on logout or compromise.

**Interview Answer:**  
Short-lived tokens + rotation + secret manager = safe API.

---

## 10. Common Security Practices

- Enable **CORS** properly for trusted origins.  
- Use **CSRF tokens** for cookie-based logins.  
- Apply **rate limiting** and **brute force protection**.  
- Maintain **audit logs** for all access.  
- Return clear **401/403** responses.

---

## 11. Healthcare Example Flow

1. Doctor logs in via OAuth2 + OIDC (Auth Code + PKCE).  
2. Receives ID Token (identity) + Access Token (authorization).  
3. Sends API request:  
   ```
   GET /api/v1/patients/101
   Authorization: Bearer <Access Token>
   ```
4. API verifies:  
   - Token signature ✅  
   - Expiry ✅  
   - Scope (`read:patients`) ✅  
   - Role = doctor ✅  
   - Ward check ✅  
5. Returns authorized patient data.

---

## 12. Interview Questions (Quick Recap)

1. What is Authentication vs Authorization?  
   Authentication verifies identity; Authorization checks permissions.

2. What is JWT?  
   A signed token containing claims used for stateless authentication.

3. Why use OAuth 2.0?  
   To manage secure access between apps and APIs.

4. What is the difference between OAuth and OIDC?  
   OAuth = authorization; OIDC = authentication.

5. What are Scopes and Roles?  
   Scopes define actions; roles define user permissions.

6. How do you protect tokens?  
   Use HTTPS, store securely, short expiry, rotate keys.

7. What’s 401 vs 403?  
   401 = Not authenticated; 403 = Authenticated but not allowed.

8. How do you handle refresh tokens?  
   Rotate after use and revoke on logout.

---

End of Phase 3 Notes
