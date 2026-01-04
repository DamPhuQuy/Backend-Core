# Basic Objects of Node.js

## 1. `req` (Request Object)

`req` represents the **incoming HTTP request**.
It contains all data sent by the client.

### Common `req` attributes

#### Request data

| Property     | Description                         |
| ------------ | ----------------------------------- |
| `req.method` | HTTP method (`GET`, `POST` e,tc.)   |
| `req.url`    | Request URL                         |
| `req.path`   | URL path                            |
| `req.query`  | Query parameters                    |
| `req.params` | Route parameters                    |
| `req.body`   | Request body (via `express.json()`) |

```ts
// GET /users/123?active=true
req.params.id; // "123"
req.query.active; // "true"
```

---

#### Headers & cookies

| Property            | Description                 |
| ------------------- | --------------------------- |
| `req.headers`       | All HTTP headers            |
| `req.get(name)`     | Read a specific header      |
| `req.cookies`       | Cookies (via cookie-parser) |
| `req.signedCookies` | Signed cookies              |

---

#### Authentication / metadata

| Property       | Description                 |
| -------------- | --------------------------- |
| `req.ip`       | Client IP address           |
| `req.protocol` | http / https                |
| `req.hostname` | Hostname                    |
| `req.user`     | Authenticated user (custom) |

`req.user` is **not built-in** — it's usually added by auth middleware.

---

## 2. `res` (Response Object)

`res` is used to **send data back to the client**.

### Common `res` methods

#### Sending responses

| Method             | Description                    |
| ------------------ | ------------------------------ |
| `res.send()`       | Send text, JSON, or buffer     |
| `res.json()`       | Send JSON response             |
| `res.status(code)` | Set HTTP status                |
| `res.end()`        | End response                   |
| `res.download()`   | Prompt a file to be downloaded |

```ts
res.status(200).json({ message: "OK" });
```

---

#### Headers & cookies

| Method              | Description          |
| ------------------- | -------------------- |
| `res.set()`         | Set response headers |
| `res.get()`         | Get response header  |
| `res.cookie()`      | Set a cookie         |
| `res.clearCookie()` | Remove a cookie      |

```ts
res.cookie("sessionId", "abc123", {
  httpOnly: true,
  secure: true,
});
```

---

#### Redirects & files

| Method           | Description      |
| ---------------- | ---------------- |
| `res.redirect()` | Redirect client  |
| `res.sendFile()` | Send a file      |
| `res.type()`     | Set Content-Type |

---

## 3. `next` (Next Function)

`next` is a **function that passes control** to the next middleware.

### How `next()` works

```ts
app.use((req, res, next) => {
  console.log("Middleware runs");
  next();
});
```

- If `next()` is called → control continues
- If response is sent → request lifecycle ends
- If `next(err)` is called → error middleware runs

---

### Error handling with `next`

```ts
app.use((req, res, next) => {
  const error = new Error("Unauthorized");
  next(error);
});
```

Error-handling middleware:

```ts
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});
```

Error middleware has **4 parameters**.

---

## 4. Typical middleware lifecycle

```
Request
  ↓
req created
  ↓
middleware 1 (req, res, next)
  ↓
middleware 2
  ↓
route handler
  ↓
res sent
```

---

[← Back to Index](README.md)
