# Basic Authentication Flow

<img src="../img/http-auth-sequence-diagram.jpg">

Basic authentication is the simplest form of authentication where credentials are sent with each request, typically using HTTP headers.

## How It Works

1. **Client Request**: The client sends a request to a protected resource
2. **Server Challenge**: If credentials are missing, server responds with `401 Unauthorized` and `WWW-Authenticate: Basic realm="..."` header
3. **Client Credentials**: Client resends request with credentials encoded in the `Authorization` header
4. **Server Verification**: Server decodes and validates the credentials
5. **Access Granted/Denied**: Server responds with the resource (200 OK) or denies access (403 Forbidden)

## Header Format

The credentials are sent in the `Authorization` header in the following format:

```
Authorization: Basic <base64-encoded-credentials>
```

Where `<base64-encoded-credentials>` is the Base64 encoding of `username:password`.

### Example

For username `john` and password `secret123`:

1. Concatenate with colon: `john:secret123`
2. Encode to Base64: `am9objpzZWNyZXQxMjM=`
3. Add to header: `Authorization: Basic am9objpzZWNyZXQxMjM=`

## Code Examples

### JavaScript (Client-Side)

```javascript
const username = "john";
const password = "secret123";
const credentials = btoa(`${username}:${password}`);

fetch("https://api.example.com/protected", {
  method: "GET",
  headers: {
    Authorization: `Basic ${credentials}`,
  },
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

### Node.js (Server-Side)

```javascript
const express = require("express");
const app = express();

// Basic Auth Middleware
function basicAuth(req, res, next) {
  const authHeader = req.headers.authorization;

  if (!authHeader || !authHeader.startsWith("Basic ")) {
    res.setHeader("WWW-Authenticate", 'Basic realm="Secure Area"');
    return res.status(401).json({ error: "Authentication required" });
  }

  // Decode credentials
  const base64Credentials = authHeader.split(" ")[1];
  const credentials = Buffer.from(base64Credentials, "base64").toString(
    "utf-8"
  );
  const [username, password] = credentials.split(":");

  // Validate credentials (replace with your logic)
  if (username === "john" && password === "secret123") {
    req.user = username;
    next();
  } else {
    res.status(403).json({ error: "Invalid credentials" });
  }
}

// Protected route
app.get("/protected", basicAuth, (req, res) => {
  res.json({ message: `Hello ${req.user}!` });
});
```

### Python (Flask)

```python
from flask import Flask, request, jsonify
from functools import wraps
import base64

app = Flask(__name__)

def basic_auth_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        auth_header = request.headers.get('Authorization')

        if not auth_header or not auth_header.startswith('Basic '):
            return jsonify({'error': 'Authentication required'}), 401

        # Decode credentials
        encoded_credentials = auth_header.split(' ')[1]
        decoded_credentials = base64.b64decode(encoded_credentials).decode('utf-8')
        username, password = decoded_credentials.split(':', 1)

        # Validate credentials
        if username == 'john' and password == 'secret123':
            return f(*args, **kwargs)
        else:
            return jsonify({'error': 'Invalid credentials'}), 403

    return decorated_function

@app.route('/protected')
@basic_auth_required
def protected():
    return jsonify({'message': 'Access granted!'})
```

## Advantages

**Simple to Implement**: Minimal code required on both client and server
**Widely Supported**: Built into HTTP protocol, supported by all browsers and HTTP clients
**Stateless**: No session management needed; each request is self-contained
**No Cookies Required**: Works in environments where cookies are disabled or impractical

## Disadvantages

**Not Secure Without HTTPS**: Credentials are only Base64-encoded, not encrypted
**Credentials Sent Every Request**: Increases exposure risk
**No Built-in Logout**: Browser caches credentials until the window is closed
**Limited User Experience**: Browser's default login prompt is not customizable
**No Password Complexity**: Protocol doesn't enforce password policies

## Alternatives to Consider

- **Bearer Token Authentication**: More flexible, supports expiration
- **JWT (JSON Web Tokens)**: Stateless, can carry additional claims
- **OAuth 2.0**: Industry standard for authorization
- **API Keys**: Simpler for service-to-service communication
- **Session-Based Auth**: Better for traditional web applications

## Testing Basic Auth

### Using cURL

```bash
# Method 1: Using -u flag
curl -u john:secret123 https://api.example.com/protected

# Method 2: Using Authorization header
curl -H "Authorization: Basic am9objpzZWNyZXQxMjM=" https://api.example.com/protected
```

### Using Postman

1. Open your request
2. Go to the "Authorization" tab
3. Select "Basic Auth" from the Type dropdown
4. Enter your username and password
5. Postman will automatically encode and add the header

---

[← Back to Authentication](README.md) | [← Back to Main Index](../README.md)
