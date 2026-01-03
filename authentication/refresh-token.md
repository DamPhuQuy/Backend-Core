# Refresh Token

A Refresh Token is a long-lived credential used to obtain new access tokens without requiring the user to log in again.

A refresh token cannot access protected APIs directly.

## Access Token and Refresh Token Flow

<img src="../img/f42604aa1b79625a189fdef38837f8bea7f5fe0a-1999x1053.png">

1. **Accessing a protected resource**
   The client sends a request to a protected API.
   If the client is not authenticated or the access token is missing/invalid, the server responds with **401 Unauthorized**.

2. **User login**
   The client sends **username and password** to the server.

3. **Token generation**
   After verifying the credentials, the server generates:

   - An **Access Token** (short-lived, e.g. ~5 minutes)
   - A **Refresh Token** (long-lived, e.g. months or 1 year)

   The refresh token is stored in the **database**, while the access token is not.

4. **Token delivery & storage**
   The server returns both tokens to the client.
   The client stores them securely (e.g. cookies or local storage).

5. **Authenticated requests**
   For subsequent requests, the client includes the **access token** in the request headers.
   The server verifies the access token and grants access if it is valid.

6. **Access token expiration**
   When the access token expires, the client sends the **refresh token** to a refresh endpoint to request a new access token.

7. **Token refresh & rotation**
   The server validates the refresh token and checks it against the database.
   If valid:

   - The old refresh token is deleted
   - A new refresh token (with the same expiration date) is issued and stored
   - A new access token is generated

8. **Silent re-authentication**
   The client stores the new tokens and continues making requests using the new access token, without forcing the user to log in again.

9. **Logout**
   When the user logs out:

   - The server deletes the refresh token from the database
   - The client removes both access and refresh tokens from storage

10. **Refresh token expiration or invalidation**
    If the refresh token is expired or invalid, the server rejects the request.
    The client clears stored tokens and transitions to a logged-out state.

---

[← Back to Authentication](README.md) | [← Back to Main Index](../README.md)
