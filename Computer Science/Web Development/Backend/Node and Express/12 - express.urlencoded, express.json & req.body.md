# 12 - express.urlencoded, express.json & req.body

*(Following Piyush Garg's Node.js playlist.)*

## What problem does this solve?

When a client sends data in a request body (an HTML form submission, or a JSON payload from an API client), Express — much like raw `http` (see [[06 - Building an HTTP Server with the http Module]]) — does **not** hand you a ready-made object by default. The body arrives as a raw stream of bytes; something has to read that stream and turn it into a usable JavaScript object before your route handler can do anything useful with it. That "something" is body-parsing **middleware** (see [[11 - Middleware in Express.js (Fundamentals)]]).

## How the client tells the server what format the body is in

Every request with a body includes a `Content-Type` header declaring its format. The two you'll hit constantly:

| Content-Type | When it's used | Example raw body |
|---|---|---|
| `application/x-www-form-urlencoded` | Classic HTML `<form method="POST">` submissions | `name=John&age=21` — looks just like a query string, but sent in the body instead of the URL |
| `application/json` | Modern API clients — `fetch`, `axios`, Postman's JSON body mode | `{"name": "John", "age": 21}` |

Express needs a **different parser for each format** — this is exactly why there are two separate built-in middlewares.

## `express.urlencoded({ extended })`

Parses `application/x-www-form-urlencoded` bodies into `req.body`.

```js
app.use(express.urlencoded({ extended: true }));

app.post('/signup', (req, res) => {
    console.log(req.body); // { name: 'John', age: '21' }
});
```

The `extended` option controls which underlying parsing library is used:
- **`extended: true`** — uses the `qs` library, which supports **nested objects and arrays** in form data (e.g. `user[name]=John`).
- **`extended: false`** — uses Node's built-in `querystring` module, which only supports **flat key-value pairs**.

For most everyday forms, either works — `extended: true` is the more common default choice since it doesn't restrict you if the form shape gets more complex later.

## `express.json()`

The JSON counterpart — parses `application/json` bodies into `req.body`.

```js
app.use(express.json());

app.post('/api/users', (req, res) => {
    console.log(req.body); // { name: 'John', age: 21 }  -- note: age is a NUMBER here, unlike urlencoded parsing
});
```

Most modern APIs register **both**, since you may want to accept form submissions in some routes and JSON API calls in others:
```js
app.use(express.urlencoded({ extended: true }));
app.use(express.json());
```

## Common mistakes

- **Forgetting to register the middleware at all** — `req.body` is `undefined`, and any code assuming it's an object throws errors (`Cannot read properties of undefined`).
- **Mismatched Content-Type and parser** — if the client sends JSON but only `express.urlencoded()` is registered (or vice versa), the matching parser never runs for that request, and `req.body` ends up empty or malformed. The parser is chosen based on the actual `Content-Type` header of the incoming request, not by which middleware you "meant" to use.
- **Registering the middleware after the routes** that need it — see [[11 - Middleware in Express.js (Fundamentals)]]: order matters, this must come first.
- In Postman specifically: picking the wrong Body tab option (e.g. choosing "raw / JSON" when your server only has `express.urlencoded()` registered) — the request looks fine in Postman but `req.body` will be empty server-side.

## Related concepts
[[11 - Middleware in Express.js (Fundamentals)]]
[[13 - API Testing with Postman]] — matching the request body type to the correct middleware
[[14 - Data Persistence with fs, Dynamic IDs & Validation]] — using `req.body` to actually create new data
