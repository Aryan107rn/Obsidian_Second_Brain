---
tags: [jwt, authentication, authorization, security, api, backend, computer-science]
aliases: [JSON Web Token]
---

# JWT (JSON Web Token)

## Definition

A **JSON Web Token (JWT)** is a compact, URL-safe format for carrying claims between parties. A signed JWT (JWS) protects integrity and proves that the token was created by a holder of the signing key; its payload is normally readable, not secret. Encrypted JWTs (JWE) provide confidentiality. JWT is a format, not an authentication protocol or a complete session system.

## Why it matters

JWTs are widely used as access tokens in APIs and identity systems. Understanding their structure and validation requirements helps avoid treating client-controlled token data as trusted or exposing sensitive information in a readable payload.

## Explanation

A common signed JWT has three Base64URL-encoded parts separated by dots:

```text
header.payload.signature
```

- **Header:** metadata such as token type and signing algorithm.
- **Payload:** claims, such as `sub` (subject), `iss` (issuer), `aud` (audience), `exp` (expiration), and application-specific data.
- **Signature:** cryptographic integrity protection over the encoded header and payload.

Base64URL encoding is not encryption. Anyone who obtains a typical signed token can decode and read its claims. Do not put passwords, secrets, or sensitive personal data in a normal JWT payload.

For a bearer-token API, a client commonly sends the JWT over HTTPS:

```http
Authorization: Bearer <access-token>
```

The server must validate the signature using an explicitly allowed algorithm and trusted key, and check the claims required by its token profile—commonly expiration, issuer, and audience. A decoded payload alone is untrusted. The resource server then performs a separate authorization check for the requested action.

JWTs are often self-contained, so revocation and immediate permission changes need deliberate design (for example, short access-token lifetimes, refresh-token rotation/revocation, or a server-side denylist/session check). A traditional server-side session or opaque token can be simpler when central revocation is important.

## Example

```js
// Pseudocode: use a maintained JWT library and fixed, trusted configuration.
const claims = verifyJwt(token, {
  algorithms: ["EdDSA"],
  issuer: "https://identity.example.com",
  audience: "billing-api",
});

if (!claims.sub) throw unauthorized();
if (!(await canReadInvoice(claims.sub, invoiceId))) throw forbidden();
```

The `verifyJwt` function above stands for a library that performs cryptographic verification and required claim checks. Do not implement JWT cryptography yourself.

## Common mistakes

- Assuming a signed JWT is encrypted or its payload is private.
- Trusting claims after merely decoding the token.
- Accepting whatever algorithm the token requests, or failing to pin allowed algorithms and trusted keys.
- Skipping `exp`, `iss`, or `aud` validation when required by the application.
- Treating a valid token as proof that the caller may access every resource.
- Putting bearer tokens in query strings, where they can leak through history, logs, analytics, or referrer data.
- Making access tokens long-lived while expecting immediate logout or revocation.
- Storing tokens without considering browser threat models; `HttpOnly` cookies reduce JavaScript access but require CSRF defenses appropriate to the application.

## Interview questions

1. What are the three parts of a signed JWT?
2. Does a signature encrypt the payload?
3. Which checks should a resource server perform before trusting a JWT?
4. What does stateless JWT validation make harder than server-side sessions?
5. Why are authentication and authorization separate even when an access token is valid?

## Related notes

- [[Authentication and Authorization]] — identity verification and access decisions
- [[REST APIs]] — transmitting credentials and handling API responses
- [[15 - HTTP Headers in Express.js]] — the `Authorization` header
- [[11 - Middleware in Express.js (Fundamentals)]] — validating credentials in a request pipeline

## Resources

- [RFC 7519: JSON Web Token (JWT)](https://www.rfc-editor.org/rfc/rfc7519.html)
- [RFC 8725: JSON Web Token Best Current Practices](https://www.rfc-editor.org/rfc/rfc8725.html)
- [OWASP JSON Web Token Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_Cheat_Sheet.html)
