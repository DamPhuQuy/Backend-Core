# Access Token

- An Access Token is a short-lived credential used by a client to access protected resources on a server.

- It represents:

  - Who the user is
  - What the user is allowed to do

- The method using token for authenticating is called "Token Based Authentication".

- Access Token is implemented as JWT.

## Access Token contains

Typical JWT access token payload

```json
{
  "sub": "123",
  "role": "user",
  "iat": 1700000000,
  "exp": 1700003600
}
```

| Claim  | Meaning            |
| ------ | ------------------ |
| `sub`  | User identifier    |
| `role` | Authorization role |
| `iat`  | Issued at          |
| `exp`  | Expiration time    |

## Access Token Flow

<img src="../img/unnamed.jpg">

1. **Step 1: Login**
   The client sends username and password to the server.

2. **Step 2: Token issuance**
   If credentials are valid:

   - The server generates an access token
   - The token contains user-related claims (e.g. userId, role)
   - The token is signed by the server

3. **Step 3: Client stores token**
   The client stores the access token:

   - In memory (recommended for browsers)
   - Or in a secure cookie

4. **Step 4: Access protected resources**
   For each protected request, the client sends:
   `Authorization: Bearer <access_token>`

5. **Step 5: Server verification**
   The server:
   - Verifies the token signature
   - Checks expiration and claims
   - Grants or denies access

## The problem of using only access token

| Issue               | Impact                                                                                   |
| ------------------- | ---------------------------------------------------------------------------------------- |
| Short lifetime      | Improve security but Frequent logouts, bad user experience                               |
| Long lifetime       | Improve user experience but High security risk                                           |
| No revocation       | Stolen tokens remain valid                                                               |
| No session tracking | No device control, don't know how many active logins exist or devices that are logged in |
| Token leakage       | Full account compromise                                                                  |

---

[← Back to Authentication](README.md) | [← Back to Main Index](../README.md)
