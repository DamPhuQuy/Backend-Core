# Middleware

> A middleware is a function that intercepts requests, performs logic, and either passes control forward or ends the response.

---

A **middleware** in Express is a function that has access to:

```ts
req, res, next;
```

It runs **between the incoming request and the outgoing response**.

---

## 1. Basic middleware definition

```ts
function myMiddleware(req, res, next) {
  // Do something with req or res
  next();
}
```

- `req` → request object
- `res` → response object
- `next` → passes control to the next middleware

If `next()` is **not called** and no response is sent, the request will **hang**.

---

## 2. Registering middleware

### Global middleware (runs for every request)

```ts
app.use(myMiddleware);
```

---

### Route-level middleware

```ts
app.get("/profile", myMiddleware, (req, res) => {
  res.send("Profile page");
});
```

---

### Multiple middleware in one route

```ts
app.post("/orders", authMiddleware, validateOrder, createOrder);
```

Middleware runs **from left to right**.

---

## 3. Middleware using arrow functions

```ts
app.use((req, res, next) => {
  console.log(req.method, req.path);
  next();
});
```

This is the most common style.

---

## 4. Conditional middleware

```ts
function authMiddleware(req, res, next) {
  if (!req.user) {
    return res.status(401).json({ message: "Unauthorized" });
  }
  next();
}
```

- Ends request → send response
- Or continues → `next()`

---

## 5. Async middleware

```ts
async function asyncMiddleware(req, res, next) {
  try {
    await someAsyncTask();
    next();
  } catch (err) {
    next(err);
  }
}
```

Always forward errors with `next(err)`.

---

## 6. Error-handling middleware

Error middleware has **4 parameters**:

```ts
function errorMiddleware(err, req, res, next) {
  res.status(500).json({ error: err.message });
}
```

Registered **after all routes**:

```ts
app.use(errorMiddleware);
```

---

## 7. Built-in middleware

Express provides built-in middleware:

```ts
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static("public"));
```

---

## 8. Third-party middleware

Example:

```ts
import cookieParser from "cookie-parser";

app.use(cookieParser());
```

---

## 9. Middleware lifecycle (mental model)

```
Request
  ↓
Global middleware
  ↓
Route middleware
  ↓
Route handler
  ↓
Response
```

---

## 10. Common mistakes

- Forgetting `next()`
- Sending response and calling `next()`
- Throwing errors in async middleware without handling

---

[← Back to Index](README.md)
