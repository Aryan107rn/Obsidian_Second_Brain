---
tags: [http, networking, api, web-development, backend, computer-science]
aliases: [HTTP/2, HTTP QUERY method, RFC 10008, HTTP Protocol Versions]
created: 2026-08-31
---

# HTTP Versions (HTTP/1.1 vs HTTP/2) & the QUERY Method

## What is it?

HTTP itself has evolved over time — the **methods** (`GET`, `POST`, etc. — see [[REST APIs]]) describe *what* a request wants to do, while the **protocol version** (HTTP/1.1, HTTP/2, HTTP/3) describes the underlying *transport mechanics* of how requests and responses actually travel between client and server. These are independent concerns: the same `GET` request can travel over HTTP/1.1 or HTTP/2 without your application code changing.

This note also covers **`QUERY`** — a genuinely new HTTP method (RFC 10008, published June 2026) that solves a long-standing gap between `GET` and `POST`.

## Why does HTTP/2 exist? What problem does it solve?

HTTP/1.1 was the standard for decades, but it has real performance bottlenecks for modern web pages that load dozens of resources (scripts, images, stylesheets) per page:

- **One request per connection at a time.** HTTP/1.1 technically supports "pipelining" multiple requests over one connection, but responses still have to come back in order — a slow response blocks everything queued behind it (**head-of-line blocking**). To work around this, browsers historically opened several parallel TCP connections per domain (typically 6) — which has its own overhead (each connection needs its own handshake).
- **Repetitive, uncompressed headers.** Every request resends full headers as plain text, even when most of them (cookies, user-agent, etc.) are identical to the last request.

**How HTTP/2 fixes this:**

| Feature | HTTP/1.1 | HTTP/2 |
|---|---|---|
| Connections | Multiple parallel TCP connections per domain | A **single** TCP connection per origin |
| Requests | One at a time per connection (blocking) | **Multiplexing** — many requests/responses interleaved simultaneously over that one connection |
| Headers | Sent as plain text every time | Compressed via **HPACK**, removing redundant data |
| Format | Text-based | **Binary framing** — faster to parse, less error-prone |
| Server Push | Not supported | Was included in the original spec (server proactively sends resources before being asked) but has since been **deprecated** in most browsers/servers — didn't deliver the expected real-world performance gains and added complexity |

**Key point:** upgrading to HTTP/2 is a transport-layer change, not an application-layer one — your route handlers, methods, and status codes work identically. It's typically enabled at the web server/reverse-proxy level (Nginx, load balancers, or a hosting platform), not something you write differently in your `http.createServer` callback.

## The `QUERY` method (RFC 10008, June 2026)

### The problem it solves

For complex **read-only** operations — think a search endpoint with many filters (`category`, `price range`, `tags`, `sort`, `date range`, etc.) — developers have historically had two imperfect choices:

- **`GET`** — correctly marked as **safe** (doesn't change server state) and **idempotent** (repeating it has no additional effect) and thus cacheable/retryable by proxies. But all the filter data has to be squeezed into the URL's query string (see [[07 - URL Structure & Parsing in Node.js]]), which has practical length limits that vary unpredictably across intermediary proxies, and URLs get logged far more often than request bodies (server logs, browser history, analytics) — a problem if the query contains sensitive filter values.
- **`POST`** — can carry a large, structured JSON body with no size awkwardness. But nothing in the protocol tells an intermediary "this is just a read" — `POST` is assumed by default to potentially change server state, so caches and proxies can't safely retry it automatically or cache its response.

### What `QUERY` actually is

Think of it as **"GET with a body."** `QUERY` is formally defined as both:
- **Safe** — the client isn't requesting or expecting any state change.
- **Idempotent** — sending it once or many times has the same effect, so it can be safely retried automatically.

...while still allowing a request **body**, like `POST` does — solving the URL-length and data-sensitivity problems of `GET` without losing the safety/cacheability guarantees that made `GET` trustworthy to intermediaries in the first place.

```
GET    /search?category=shoes&price_min=20&price_max=80&tags=running,trail...
       (works, but breaks down fast with many filters — URL length limits, logged everywhere)

POST   /search   { category: "shoes", price_min: 20, ... }
       (works for the body, but caches/proxies can't assume it's safe to retry or cache)

QUERY  /search   { category: "shoes", price_min: 20, ... }
       (best of both — safe, idempotent, AND has a body)
```

### Current status (as of this note)

`QUERY` was published as an IETF Proposed Standard in **June 2026** — the **first new HTTP method since `PATCH`** was added in 2010 (a 16-year gap). Because it's this new:
- Framework support is still rolling out — Express doesn't have first-class routing support for it yet at time of writing; handling it manually via `req.method === 'QUERY'` works the same way as any other method (see [[08 - HTTP Methods & Method-Based Routing in Node.js]]), since the underlying `http` module doesn't restrict which method strings it can receive.
- Older infrastructure (WAFs, reverse proxies, method allowlists) written before mid-2026 may not recognize `QUERY` as a distinct method and could reject or mishandle it — worth checking if you deploy behind such infrastructure.
- It is **not** a CORS-safelisted method, meaning browser JavaScript calling it cross-origin triggers a preflight `OPTIONS` request, same as custom methods/headers normally do.
- Particularly well-suited to GraphQL-style query endpoints (see [[GraphQL]]) — a GraphQL *query* (as opposed to a *mutation*) is inherently safe/idempotent but is currently sent via `POST` purely for body-size reasons; `QUERY` is a more semantically correct fit.

## Common mistakes / things to watch for

- Assuming HTTP/2's multiplexing means you need multiple TCP connections for parallel requests — it's actually the opposite; HTTP/2 deliberately consolidates onto **one** connection and gets parallelism a different way (interleaved streams).
- Treating `QUERY` as equivalent to `POST` when building server logic — this defeats its purpose. If a `QUERY` handler causes a side effect (writes to a database, sends an email), that violates its safe/idempotent contract and will break caching/retry assumptions made by any intermediary in the request path.
- Assuming universal support for `QUERY` in production infrastructure this early — always check whether your specific framework, proxy, and CDN understand it before depending on it.

## Related concepts
[[REST APIs]] — the established HTTP verbs and status code conventions this method extends
[[08 - HTTP Methods & Method-Based Routing in Node.js]] — how `req.method` checks work in raw Node, applies identically to `QUERY`
[[GraphQL]] — a natural use case for `QUERY`'s safe-request-with-body semantics
[[gRPC]] — another protocol that relies on HTTP/2 specifically
