# The gap between theory and real-world practice

The original goal of **JWT authentication** is to be **stateless**.
However, in real implementations, we often **store refresh tokens in a database**, which means the server must keep **user-related state**. This breaks the idea of being fully stateless.

This shows a **conflict between theory and practice**.

---

# Stateless vs Stateful in reality

If we strictly follow stateless principles:

- The server should not store any authentication state
- Tokens should be self-contained and verifiable without storage

But in practice:

- Pure stateless JWT is **hard to secure**
- Long-lived tokens introduce serious risks

Therefore, a **hybrid approach** is more realistic and safer:

- **Access Token → Stateless**
- **Refresh Token → Stateful**

This balances:

- Scalability (stateless access tokens)
- Security and control (stateful refresh tokens)

---

# Why store Refresh Tokens in the database?

Refresh tokens usually have a **long lifetime** (weeks or months).
Storing them in the database allows the server to:

- Revoke tokens if they are compromised
- Invalidate tokens on logout
- Manage user sessions across devices

For example:

- If a user’s refresh token is leaked, the server can **delete it from the database**
- Any future refresh attempt will fail
- The system becomes safer

---

# Logout behavior and its limitation

When a user logs out:

- The server deletes the refresh token from the database
- The client removes stored tokens

However:

- Logout is **not immediate**
- The user remains authenticated **until the access token expires**

This is a known limitation of JWT-based systems.

---

# Possible improvements

To reduce this delay, we can:

- Use **short-lived access tokens**
- Combine with **real-time mechanisms** such as WebSocket
- Actively notify the client to log out immediately

This improves security and user control, at the cost of extra complexity.

---

[← Back to Authentication](README.md) | [← Back to Main Index](../README.md)
