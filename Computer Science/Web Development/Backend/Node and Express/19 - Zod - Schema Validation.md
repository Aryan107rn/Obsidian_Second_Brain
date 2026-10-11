# 19 - Zod - Schema Validation

## What is Zod?
**Zod** is a TypeScript-first **schema declaration and validation** library for JavaScript. You describe the exact shape and rules of your data once (a *schema*), and Zod checks real data against it, returning either clean, trusted data or detailed errors. It works in plain JavaScript too — you just don't get the type benefits.

## Why validation matters
Everything coming from a client — `req.body`, `req.query`, `req.params`, headers — is **untrusted**. Without checks:
- Missing/wrong-typed fields crash your code (`Cannot read properties of undefined`).
- Garbage gets written to your database.
- Attackers send unexpected shapes (e.g. `{ "email": { "$gt": "" } }` NoSQL-injection style, or `role: "admin"`).

TypeScript types disappear at runtime, so they can't protect you from incoming JSON. A runtime validator can. Validation is the 400/422 half of [[14 - Data Persistence with fs, Dynamic IDs & Validation]] done properly (status codes in [[REST APIs]]).

## Manual checks vs Zod
```js
// Manual — grows messy, inconsistent, easy to forget a case
if (!email || typeof email !== 'string' || !email.includes('@')) ...
if (!password || password.length < 8) ...
if (age && (typeof age !== 'number' || age < 13)) ...
```
```js
// Zod — declare once, reuse everywhere
const signupSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
  age: z.number().int().min(13).optional(),
});
```

## Setup and basics
```bash
npm install zod
```
```js
const { z } = require('zod');   // ES modules: import { z } from 'zod'

const userSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),
  email: z.string().email('Invalid email'),
  age: z.number().int().min(13).optional(),
  role: z.enum(['user', 'admin']).default('user'),
});
```

### Common building blocks
| Schema | Notes |
|---|---|
| `z.string()` | `.min() .max() .length() .email() .url() .uuid() .regex() .trim()` |
| `z.number()` | `.int() .positive() .min() .max()` |
| `z.boolean()`, `z.date()` | |
| `z.enum(['a','b'])` | one of a fixed set |
| `z.array(z.string())` | `.min(1)` for non-empty |
| `z.object({...})` | nested objects allowed |
| `.optional()` | field may be missing |
| `.nullable()` | value may be `null` |
| `.default(x)` | fill in when missing |
| `z.coerce.number()` | converts input (e.g. `"21"` → `21`) before validating |

Custom rules and cross-field checks:
```js
const passwordsMatch = z.object({
  password: z.string().min(8),
  confirm: z.string(),
}).refine(d => d.password === d.confirm, {
  message: 'Passwords do not match',
  path: ['confirm'],
});
```

## `parse` vs `safeParse`
```js
// parse: returns the data or THROWS a ZodError
const data = userSchema.parse(input);

// safeParse: never throws — returns a result object (preferred in request handlers)
const result = userSchema.safeParse(input);
if (!result.success) {
  console.log(result.error.issues);
  // [{ path: ['email'], message: 'Invalid email', code: 'invalid_format' ... }]
} else {
  console.log(result.data);   // validated (and cleaned) data
}
```
Always use `result.data`, not the original input: Zod returns a **new object** with defaults applied, coerced values, and **unknown keys stripped** (so an attacker's extra `isAdmin: true` is dropped). Use `.strict()` on an object to reject unknown keys instead.

## Express validation middleware
Reusable middleware that validates and replaces `req.body` (middleware concepts: [[11 - Middleware in Express.js (Fundamentals)]]; body parsing: [[12 - express.urlencoded, express.json & req.body]]):
```js
const validate = (schema) => (req, res, next) => {
  const result = schema.safeParse(req.body);
  if (!result.success) {
    return res.status(400).json({
      error: 'Validation failed',
      details: result.error.issues.map(i => ({
        field: i.path.join('.'),
        message: i.message,
      })),
    });
  }
  req.body = result.data;  // trusted, cleaned data from here on
  next();
};

app.post('/signup', validate(signupSchema), signupHandler);
```
The handler now runs **only** with valid data, so it needs no defensive checks. Validate `req.query` and `req.params` the same way (query values are always strings, so use `z.coerce.number()` for numbers).

## Inferring TypeScript types
```ts
type Signup = z.infer<typeof signupSchema>;   // { email: string; password: string; age?: number }
```
One schema gives both runtime validation and the compile-time type — no duplicated definitions.

## Version note
Zod 4 is current; the code above works in it. In v4 top-level helpers like `z.email()` are the preferred form (`z.string().email()` still works but is deprecated), and `.issues` is the stable way to read errors in both v3 and v4.

## Alternatives
Joi, Yup, Valibot, and express-validator solve the same problem. Zod's appeal is TypeScript inference and a small, composable API.

## Common mistakes
- Validating on the frontend only — attackers skip your UI. **Always validate on the server.**
- Using the raw `req.body` after validation instead of `result.data` (loses defaults, coercion and stripping).
- Using `parse` without a `try/catch` — an uncaught `ZodError` becomes a 500.
- Forgetting the body-parsing middleware first — `req.body` is `undefined`, so validation fails for the wrong reason.
- Returning 500 for bad input — client mistakes are 400 (or 422).
- Forgetting query strings are strings — `z.number()` fails on `"5"`; use `z.coerce.number()`.
- Validating shape but not business rules (duplicate email is a 409 from your DB logic, not Zod).

## Related concepts
- [[17 - Authentication Fundamentals & Password Hashing (bcrypt)]] · [[18 - JWT (JSON Web Tokens)]]
- [[20 - Building Authentication from Scratch]] — Zod + bcrypt + JWT together
- [[14 - Data Persistence with fs, Dynamic IDs & Validation]] — the manual validation this improves on
- [[REST APIs]] — 400 vs 422 vs 409
