# Backend Core

A comprehensive guide to backend development concepts, Node.js, Express, and authentication patterns.

## Table of Contents

### Core Concepts

- [Stateless and Stateful](stateless-stateful.md)
- [Client-Server Model](client-server.md)
- [HTTP Methods](http-methods.md)
- [Header and Body](header-body.md)
- [REST Concept](rest-concept.md)

### Node.js & Express

- [Basic Objects of Node.js](nodejs-basics.md) - `req`, `res`, `next`
- [Middleware](middleware.md) - How to define and use middleware in Express
- [Backend with Node.js](backend-nodejs.md) - Event-driven programming and IoC

### Authentication

- [Authentication Overview](authentication/README.md)
  - [Basic Authentication Flow](authentication/basic-auth.md)
  - [Cookie and Session](authentication/cookie-session.md)
  - [JWT (JSON Web Token)](authentication/jwt.md)
  - [Access Token](authentication/access-token.md)
  - [Refresh Token](authentication/refresh-token.md)

---

## Quick Reference

### HTTP Methods

| Method | Purpose                |
| ------ | ---------------------- |
| GET    | Retrieve data          |
| POST   | Create new resource    |
| PUT    | Update entire resource |
| PATCH  | Partial update         |
| DELETE | Remove resource        |

### Authentication Methods

| Method        | Type      | Storage               |
| ------------- | --------- | --------------------- |
| Session       | Stateful  | Server                |
| JWT           | Stateless | Client                |
| Access Token  | Stateless | Client (short-lived)  |
| Refresh Token | Stateful  | Database (long-lived) |
