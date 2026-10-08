---
tags: [authentication, authorization, security, backend, api, computer-science]
aliases: [Auth, AuthN and AuthZ]
---

# Authentication and Authorization

## Definition

**Authentication (AuthN)** establishes who a user or service is. **Authorization (AuthZ)** decides what that authenticated identity is allowed to do. A login may authenticate a user; a policy check then determines whether that user can read a particular invoice or administer a system.

## Why it matters

An API must verify identity and enforce access rules on the server for every protected operation. Hiding a button or protecting a frontend route improves the interface, but does not protect the underlying endpoint.

## Explanation

A typical request flow is:

1. The client presents credentials, such as a password during login, a session cookie, or an access token.
2. The server validates the credentials and establishes an authenticated identity.
3. The server checks the requested action against that identity's permissions and the target resource.
4. The server performs the operation or rejects it.

Common approaches include:

- **Server-side sessions:** the browser holds an opaque session identifier, usually in a cookie; session state lives on the server.
- **Bearer access tokens:** the client presents a token, often in the `Authorization` header. A [[JWT]] is one possible token format; opaque tokens are another.
- **Federated login:** OAuth 2.0 is primarily an authorization framework; OpenID Connect (OIDC) adds an identity layer for sign-in.

Authorization can use roles (RBAC), attributes or context (ABAC), or explicit resource ownership and policy checks. Authentication alone never implies permission to access every resource.

### HTTP responses

- `401 Unauthorized`: credentials are missing or invalid; authenticate or present valid credentials.
- `403 Forbidden`: the request is understood, but the authenticated identity lacks permission.

Use HTTPS for credentials and tokens. Validate permissions on every request, deny by default, and avoid exposing secrets in URLs or logs. For browser sessions, configure cookies with appropriate `Secure`, `HttpOnly`, and `SameSite` attributes and protect state-changing requests against CSRF as needed.

## Example

A user sends:

```http
GET /api/invoices/42
Authorization: Bearer <access-token>
```

The server first validates the credential (authentication), then checks whether the identified user may view invoice `42` (authorization). If the credential is invalid, return `401`; if valid but access is denied, return `403` (or a deliberate `404` policy when resource existence is concealed).

## Common mistakes

- Treating authentication and authorization as the same step.
- Relying on frontend-only route guards or hidden controls.
- Checking that a user is logged in but failing to check ownership or permission for the specific resource.
- Assuming every access token is a JWT, or that every JWT is encrypted.
- Storing long-lived credentials in browser storage without considering XSS, CSRF, and token theft risks.
- Returning `401` for every denial instead of distinguishing invalid identity from insufficient permission.

## Interview questions

1. What is the difference between authentication and authorization?
2. When should an API return `401` versus `403`?
3. Why must authorization be checked server-side for each resource?
4. How do sessions, opaque tokens, and JWTs differ?
5. What does OIDC add on top of OAuth 2.0?

## Related notes

- [[JWT]] — compact claims format often used for access tokens
- [[REST APIs]] — HTTP status codes and API request patterns
- [[15 - HTTP Headers in Express.js]] — `Authorization` request header
- [[11 - Middleware in Express.js (Fundamentals)]] — route-level authentication and authorization checks
- [[09 - Global State Management & Context API]] — representing signed-in state in React
- [[System Design MOC]] — security topics

## Resources

- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
- [OAuth 2.0 Security Best Current Practice (RFC 9700)](https://www.rfc-editor.org/rfc/rfc9700.html)
