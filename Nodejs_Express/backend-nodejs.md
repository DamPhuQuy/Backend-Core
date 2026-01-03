# Backend with Node.js

## 1. Node.js creates an HTTP server

```ts
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(3000);
```

Under the hood, Express creates an HTTP server using Node.js.
A **callback function** is registered to handle incoming HTTP requests.

This callback is **not called manually**.
It is executed automatically when an HTTP request arrives.

---

## 2. Incoming request = an event

When a client sends an HTTP request:

- Node.js emits an internal `"request"` event
- The **event loop** detects the event
- The registered callback is executed
- The request and response objects (`req`, `res`) are passed to the callback

You did **not** explicitly call the handler function —
the **runtime environment** did.

This programming model is called **event-driven programming**.

---

## 3. Express uses middleware and inversion of control

Express does not handle requests with a single function.
Instead, it uses a **middleware pipeline**.

Each middleware is a function with the signature:

```ts
(req, res, next) => void
```

Example:

```ts
app.use(express.json());
app.use(cookieParser());

app.get("/profile", (req, res) => {
  res.send("Protected resource");
});
```

For every incoming request:

1. Express automatically executes each registered middleware
2. Each middleware can:

   - Read or modify `req` and `res`
   - End the response
   - Pass control to the next middleware using `next()`

3. The final route handler sends the response

This is an example of **Inversion of Control (IoC)**:

- You define _what should happen_
- The framework controls _when it happens_

---

## 4. Why you don't "call" request handlers

You never write code like:

```ts
handleRequest(req, res);
```

Because:

- Node.js listens for events
- Express registers handlers
- The framework executes your functions **in response to events**

Your code is **reactive**, not imperative.

---

[← Back to Index](README.md)
