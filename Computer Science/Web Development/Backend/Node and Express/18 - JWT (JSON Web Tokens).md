# 18 - JWT (JSON Web Tokens)

## What is a JWT?
A **JSON Web Token** (pronounced "jot") is a compact, **signed** string that carries information (claims) about a user. After a successful login the server hands the client a JWT; the client sends it back with every later request, and the server verifies the signature to trust it — without keeping any login record of its own.

## Why it exists
HTTP is **stateless**: each request is independent, so after `/login` the server has no built-in memory of who you are (see [[17 - Authentication Fundamentals & Password Hashing (bcrypt)]]). Two ways to solve that:
- **Sessions (stateful):** server stores "session 91 = Aryan" and gives the client the ID. Needs a shared store once you run several servers.
- **JWT (stateless):** the *token itself* says who you are and is **cryptographically signed**, so any server holding the secret can verify it with no lookup. This fits stateless REST APIs and horizontal scaling. Full stateful-vs-stateless comparison and trade-offs: [[17 - Authentication Fundamentals & Password Hashing (bcrypt)]].

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Express server
    C->>S: POST /login (email, password)
    S->>S: verify password (bcrypt)
    S-->>C: 200 + JWT (signed with secret)
    C->>S: GET /profile  (Authorization: Bearer JWT)
    S->>S: verify signature + expiry
    S-->>C: 200 profile data
```

## Structure: three parts, separated by dots
```
xxxxx.yyyyy.zzzzz
header.payload.signature
```
Example (decoded):
```json
// Header
{ "alg": "HS256", "typ": "JWT" }

// Payload (claims)
{ "sub": "42", "email": "a@x.com", "role": "user", "iat": 1760000000, "exp": 1760003600 }

// Signature = HMAC_SHA256( base64url(header) + "." + base64url(payload), SECRET )
```
- **Header** — token type and signing algorithm.
- **Payload** — the claims. Standard ones: `sub` (subject/user id), `iat` (issued at), `exp` (expiry), `iss` (issuer), `aud` (audience). You can add custom ones (`role`).
- **Signature** — proves the header+payload weren't altered and were made by someone with the secret.

### The most important fact: a JWT is signed, NOT encrypted
Header and payload are only **Base64URL-encoded**, which anyone can decode (paste a token into jwt.io). So:
- ✅ Tampering is detectable — change the payload and the signature no longer matches.
- ❌ Contents are **readable by anyone** — never put passwords, secrets, or sensitive data in the payload.

## Signing algorithms
| | HS256 (symmetric) | RS256 (asymmetric) |
|---|---|---|
| Keys | One shared secret signs *and* verifies | Private key signs, public key verifies |
| Use when | One backend issues and verifies | Many services verify but only one may issue |
| Simplicity | Simplest — the usual choice for a single Express app | More setup |

## Using it in Express: `jsonwebtoken`
```bash
npm install jsonwebtoken dotenv
```
```
# .env  (add to .gitignore!)
JWT_SECRET=use-a-long-random-string-at-least-32-chars
```
```js
require('dotenv').config();
const jwt = require('jsonwebtoken');

// 1. SIGN — after a successful login
const token = jwt.sign(
  { sub: user.id, role: user.role },   // payload: minimal, non-sensitive
  process.env.JWT_SECRET,
  { expiresIn: '1h' }                  // always set an expiry
);
res.json({ token });

// 2. VERIFY — on protected requests
try {
  const payload = jwt.verify(token, process.env.JWT_SECRET); // throws if invalid or expired
  // payload.sub, payload.role ...
} catch (err) {
  // err.name === 'TokenExpiredError' | 'JsonWebTokenError'
}
```
`jwt.decode(token)` only decodes **without verifying** — never trust its result for authentication.

## Protecting routes with middleware
Clients send the token in the `Authorization` header using the **Bearer** scheme (headers recap: [[15 - HTTP Headers in Express.js]]):
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```
```js
function requireAuth(req, res, next) {
  const header = req.get('Authorization') || '';
  const [scheme, token] = header.split(' ');
  if (scheme !== 'Bearer' || !token)
    return res.status(401).json({ error: 'Missing token' });

  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET); // attach identity for later handlers
    next();
  } catch (err) {
    const msg = err.name === 'TokenExpiredError' ? 'Token expired' : 'Invalid token';
    return res.status(401).json({ error: msg });
  }
}

app.get('/profile', requireAuth, (req, res) => {
  res.json({ message: 'Private data', userId: req.user.sub });
});
```
This uses exactly the `(req, res, next)` pattern from [[11 - Middleware in Express.js (Fundamentals)]]. A simple role check (authorization) is another middleware:
```js
const requireRole = (role) => (req, res, next) =>
  req.user.role === role ? next() : res.status(403).json({ error: 'Forbidden' });

app.delete('/users/:id', requireAuth, requireRole('admin'), handler);
```

## Where should the client store the token?
| Storage | Risk |
|---|---|
| `localStorage` | Readable by any JS on the page → stolen via **XSS** |
| **httpOnly cookie** (`Secure`, `SameSite`) | JS can't read it (blocks XSS theft) but needs **CSRF** protection |
| Memory only | Safest from theft, lost on refresh |
There's no perfect answer; httpOnly cookies are generally recommended for browser apps. (XSS also relates to EJS output escaping in [[16 - Server-Side Rendering (SSR) & EJS]].)

## Access + refresh tokens
A JWT can't easily be revoked before `exp`. The standard pattern: a **short-lived access token** (e.g. 15 min) used on every request, plus a **long-lived refresh token** (days) stored securely and used only to get new access tokens. Steal an access token and the damage window is small; the refresh token can be revoked server-side.

## Common mistakes
- Putting sensitive data in the payload — it's readable.
- No `expiresIn`, so a leaked token works forever.
- Weak or hard-coded secret, or committing it to Git (see [[01 - Git Basics]] on `.gitignore`/secrets). Use env vars.
- Using `jwt.decode` instead of `jwt.verify`.
- Not restricting accepted algorithms; pass `{ algorithms: ['HS256'] }` to `verify` to block algorithm-confusion attacks (including `alg: none`).
- Trusting a role sent by the client instead of the one in the *verified* token.
- Returning 403 for a missing/invalid token — that's 401.
- Assuming logout is automatic: with plain JWTs "logout" just means the client deletes the token.

## Related concepts
- [[17 - Authentication Fundamentals & Password Hashing (bcrypt)]]
- [[19 - Zod - Schema Validation]]
- [[20 - Building Authentication from Scratch]]
- [[11 - Middleware in Express.js (Fundamentals)]] · [[15 - HTTP Headers in Express.js]]
- [[REST APIs]] — statelessness and status codes
