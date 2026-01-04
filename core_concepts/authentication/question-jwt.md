# Answering “Why” Questions About JWT

## Why do we create a **new refresh token** during refresh?

The core problem is that **refresh tokens live for a long time**.
If a refresh token is leaked, an attacker can continuously obtain new access tokens.

### Solution: **Refresh Token Rotation**

Each time the client refreshes:

- Issue a **new access token**
- Issue a **new refresh token**
- **Delete the old refresh token** from the database

This limits damage if a refresh token is compromised.

### Important note

The **expiration date does NOT extend**.

Example:

- Old refresh token expires on `Oct 5, 2023`
- New refresh token also expires on `Oct 5, 2023`

This prevents infinite sessions.

---

## How can we **revoke an access token**?

### Stateless design (recommended)

Access tokens are **stateless**, so:

- They **cannot be revoked immediately**
- They expire naturally
- Revocation is done indirectly by revoking the refresh token

That’s why access tokens should have a **short lifetime**.

### Immediate revocation (trade-off)

You _can_ revoke instantly by:

- Storing access tokens in a database or blacklist
- Removing them when needed

This breaks statelessness and reduces scalability.

---

## Can two JWTs be identical?

**Yes.**

JWTs will be identical if:

- Payload is the same
- Secret key is the same
- Issued at the same second

JWT includes the `iat` (issued at) claim, measured in **seconds**.

If two tokens are created in the same second with the same payload → same JWT.

---

## Where should the client store tokens?

### Web browsers

- **Cookies** or **Local Storage**
- Cookies are slightly more secure (HttpOnly, SameSite)

Both have trade-offs; there is no perfect solution.

### Mobile apps

- Store tokens in **secure device storage**

---

## How does the client send the access token?

### Cookie storage

- Browser sends it automatically with each request

### Local Storage

Send via HTTP header:

```http
Authorization: Bearer <access_token>
```

---

## Why use the **Bearer** prefix?

Technically optional, but **recommended by standards**.

```http
Authorization: Bearer <token>
```

### Meaning of “Bearer”

- “Bearer” = whoever **possesses** the token
- No identity proof beyond the token itself

### Benefits

- Clearly indicates authentication method
- Follows HTTP standards
- Supports multiple auth methods on the server

---

## Is deleting tokens on the client enough for logout?

### User experience

Yes — the user appears logged out

### Security

No — refresh token still exists in the database

If an attacker has that refresh token, they can still get new access tokens.

### Proper logout requires

1. Client deletes access & refresh tokens
2. Server deletes refresh token from database

---

[← Back to Authentication](README.md) | [← Back to Main Index](../README.md)
