# Backend Core

A comprehensive guide to backend development concepts, Node.js, Express, and authentication patterns.

## Table of Contents

### Introduction

- [Stateless and Stateful](core_concepts/introduction/stateless-stateful.md)
- [Client-Server Model](core_concepts/introduction/client-server.md)
- [HTTP Methods](core_concepts/introduction/http-methods.md)
- [Header and Body](core_concepts/introduction/header-body.md)

### Node.js & Express

- [Basic Objects of Node.js](core_concepts/Nodejs_Express/nodejs-basics.md) - `req`, `res`, `next`
- [Middleware](core_concepts/Nodejs_Express/middleware.md) - How to define and use middleware in Express
- [Backend with Node.js](core_concepts/Nodejs_Express/backend-nodejs.md) - Event-driven programming and IoC

### API Styles

- [REST Concept](core_concepts/api/rest-concept.md)

### Database

- [PostgreSQL](core_concepts/database/postgreSQL.md)

### Authentication

- [Authentication Overview](core_concepts/authentication/README.md)
  - [Basic Authentication Flow](core_concepts/authentication/basic-auth.md)
  - [Cookie and Session](core_concepts/authentication/cookie-session.md)
  - [JWT (JSON Web Token)](core_concepts/authentication/jwt.md)
  - [Access Token](core_concepts/authentication/access-token.md)
  - [Refresh Token](core_concepts/authentication/refresh-token.md)

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
