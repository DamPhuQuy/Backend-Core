# REST Concepts

An API is considered **RESTful** when it follows these principles:

## 1. Resource-based URLs

- URLs represent **resources (nouns)**, not actions
- Examples:

  - `/products`
  - `/users/123/orders`

- Avoid verbs in URLs:

  - `/getProducts`
  - `/createUser`

---

## 2. HTTP methods define actions

Use **HTTP methods** to express the operation:

| Method      | Action              |
| ----------- | ------------------- |
| GET         | Retrieve a resource |
| POST        | Create a resource   |
| PUT / PATCH | Update a resource   |
| DELETE      | Remove a resource   |

Example:

```http
GET /products
POST /products
DELETE /products/123
```

---

## 3. Proper HTTP status codes

APIs should return **meaningful HTTP status codes**:

| Status Code | Meaning               |
| ----------- | --------------------- |
| 200         | OK                    |
| 201         | Created               |
| 204         | No Content            |
| 400         | Bad Request           |
| 401         | Unauthorized          |
| 403         | Forbidden             |
| 404         | Not Found             |
| 500         | Internal Server Error |

---

## 4. Stateless communication

- Each request must contain **all information needed** to process it
- The server **does not store client state** between requests

This usually means:

- No server-side sessions
- Use **tokens** (e.g. JWT) sent with every request

Example:

```http
Authorization: Bearer <token>
```

---

### 5. Separation of client and server

- Client and server are independent
- They communicate only via HTTP requests and responses
- This allows better scalability and maintainability

---

### 6. Uniform interface (optional but important)

- Consistent request/response formats
- Predictable endpoint behavior
- Clear error messages

---

## Important clarification

> **REST does NOT forbid sessions by definition**,
> but **statelessness is a core REST constraint**.

In practice:

- Session-based auth breaks statelessness
- Token-based auth fits REST well

---

[← Back to Index](README.md)
