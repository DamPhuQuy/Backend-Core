# JWT (JSON Web Token)

- JWT is a token-based authentication mechanism used to securely transmit information between a client and a server.

- JWT is commonly used to implement stateless authentication in backend systems.

- JWT belongs to stateless authentication, all required authentication information is stored inside the token.

## JWT structure

consists of three parts, seperated by dots:
`header.payload.signature`

E.g: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.eyJ1c2VySWQiOjEyMywiaWF0IjoxNzAwMDAwMDB9
.xxxxxxxx

| Part      | Purpose                        |
| --------- | ------------------------------ |
| Header    | Token type & signing algorithm |
| Payload   | User data (claims)             |
| Signature | Prevents tampering             |

Header using HMAC SH256 or RSA algorithm to encode.

In JWT, payload is in another context, payload is Base64 encoded, not encrypted.

Signature using HMACSHA256 algorithm with content is Base64 encoded Header + Base64 encoded Payload, and "secret key". Only server knows "secret key" so only server could verify.

---

[← Back to Authentication](README.md) | [← Back to Main Index](../README.md)
