# 13 - API Testing with Postman

*(Following Piyush Garg's Node.js playlist.)*

## What is it, and why does it exist?

A browser's address bar can only easily trigger **`GET`** requests — typing a URL and hitting enter is always a `GET`. Even HTML forms are limited to `GET`/`POST`. There's no way to type a `PATCH` or `DELETE` request into a browser address bar, and constructing a `POST` with a specific JSON body and custom headers isn't practical that way either.

**Postman** is a dedicated API client — a GUI tool that lets you construct **any** HTTP request by hand: choose the method, set the URL, add headers, set the body in whatever format you need, and send it — independent of what a browser's UI happens to support. This makes it the standard tool for testing backend endpoints during development, before a frontend even exists to call them.

## Setting up a request

1. **Method dropdown** — choose `GET`, `POST`, `PATCH`, `PUT`, `DELETE`, etc.
2. **URL field** — e.g. `http://localhost:8000/users`.
3. **Body tab** (for methods that carry data) — choose the format matching what your server's middleware expects (see [[12 - express.urlencoded, express.json & req.body]]):
   - `x-www-form-urlencoded` — if your server uses `express.urlencoded()`.
   - `raw` → `JSON` — if your server uses `express.json()`.
4. **Headers tab** — add custom headers if needed (Postman sets `Content-Type` automatically based on your Body tab choice, which is usually what you want — manual overrides here can conflict with that).
5. **Send** — fires the request and displays the response.

## Analyzing the response

Postman's response panel shows more than just the returned data — three things worth actively checking every time during development:

| What to check | Why it matters |
|---|---|
| **Status code** (e.g. `200 OK`, `201 Created`, `404 Not Found`) | Immediate confirmation of what actually happened — see [[REST APIs]]'s status code cheat sheet for the full picture. A `200` when you expected a `404` (or vice versa) is often the fastest way to spot a routing or logic bug. |
| **Response duration** (ms) | Flags slow endpoints early — useful for catching accidentally-expensive operations (e.g. a blocking `fs` call, see [[04 - File Handling in Node.js (fs module)]]) while the project is still small and easy to fix. |
| **Response size** (payload size, e.g. KB) | Flags **over-fetching** — sending back far more data than the client actually needs. Worth noticing early; this exact problem is part of what motivated GraphQL's design (see [[GraphQL]]) for APIs with more complex client data needs. |

## Common mistakes

- Choosing the wrong Body tab format relative to what the server's middleware expects — the request "looks right" in Postman but `req.body` comes back empty or malformed server-side (see [[12 - express.urlencoded, express.json & req.body]]).
- Not organizing requests into a Postman **Collection** as the number of endpoints grows — makes retesting tedious and error-prone; collections let you save and rerun requests instead of rebuilding them each time.
- Ignoring response duration/size during development, only to discover performance problems once real users hit the API in production, when they're far more expensive to diagnose and fix.

## Related concepts
[[12 - express.urlencoded, express.json & req.body]]
[[REST APIs]] — full status code reference
[[14 - Data Persistence with fs, Dynamic IDs & Validation]] — the endpoints you'll actually be testing with Postman
