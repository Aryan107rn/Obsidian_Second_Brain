# 07 - URL Structure & Parsing in Node.js

*(Following Piyush Garg's Node.js playlist.)*

## What is a URL?

A **URL** (Uniform Resource Locator) is the address of a resource on the web — it tells a client exactly where to find something and how to ask for it. Every URL breaks down into a few distinct parts:

```
https://example.com/products/42?category=shoes&sort=price
└──┬──┘ └────┬─────┘└──────┬──────┘└──────────┬───────────┘
 protocol   domain         path            query parameters
```

| Part | What it is | Example |
|---|---|---|
| **Protocol / scheme** | The rules governing how client and server communicate. `https` is encrypted (uses an SSL/TLS certificate to secure data in transit); plain `http` is unencrypted. `ws`/`wss` are the equivalents for real-time WebSocket connections (see [[WebSocket]]). | `https://` |
| **Domain name** | A human-friendly alias for a server's IP address. IP addresses (e.g. `172.217.14.206`) are hard to memorize, so DNS (Domain Name System) resolves a domain like `example.com` to the actual server address behind the scenes. | `example.com` |
| **Path** | Identifies the specific resource on the server. The root path `/` is the home page; further segments (`/products/42`) point to nested/specific resources. | `/products/42` |
| **Query parameters** | Key–value pairs appended after a `?`, separated by `&`, used to pass extra instructions to the server **without** changing which resource is being addressed — e.g. filters, search terms, pagination. | `?category=shoes&sort=price` |

**Why query parameters matter:** they let one path serve many variations of a request. `/products` can return all products, filtered products, sorted products, paginated results — all through the same route, just with different query values. This is the mechanism behind search engines, video platforms fetching a specific video by ID, and filtered product listings.

## The problem: Node's raw `http` module doesn't parse any of this for you

When a request comes in, `req.url` (see [[06 - Building an HTTP Server with the http Module]]) gives you the **entire path + query string as one raw string** — nothing more:

```js
// Request: GET /products/42?category=shoes&sort=price
console.log(req.url);
// → "/products/42?category=shoes&sort=price"   (just a plain string, unparsed)
```

There's no built-in `req.pathname` or `req.query` object — you have to parse `req.url` yourself to separate the path from the query, and turn the query string into a usable JavaScript object.

## Parsing the URL — two approaches

### Modern approach: the WHATWG `URL` class (recommended)

Node supports the same `URL` API used in browsers. Since `req.url` is only the **relative** part (no domain), you must supply a base URL for it to resolve against:

```js
const server = http.createServer((req, res) => {
  const baseURL = `http://${req.headers.host}`;      // e.g. "http://localhost:8000"
  const parsedUrl = new URL(req.url, baseURL);

  console.log(parsedUrl.pathname);        // "/products/42"
  console.log(parsedUrl.searchParams.get('category'));  // "shoes"
  console.log(parsedUrl.searchParams.get('sort'));       // "price"
});
```
- **`.pathname`** — just the path, with the query string stripped off. **Always route based on this, never on the raw `req.url`** — otherwise `/about` and `/about?ref=email` would be treated as two different routes.
- **`.searchParams`** — a `URLSearchParams` object; use `.get('key')` to read individual query values, or iterate it with `.entries()` to get all of them.

### Legacy approach: the built-in `url` module

Older code (and many existing tutorials) use Node's original `url` module instead:

```js
const url = require('url');

const parsedUrl = url.parse(req.url, true);
// the `true` second argument tells it to also parse the query string into an object

console.log(parsedUrl.pathname);   // "/products/42"
console.log(parsedUrl.query);      // { category: 'shoes', sort: 'price' }
```
This still works, but Node's docs mark `url.parse()` as **legacy** in favor of the WHATWG `URL` class — new code should prefer `new URL()`.

## Practical routing steps, put together

1. **Parse** `req.url` into `pathname` + query data (using either approach above).
2. **Route** using `pathname` only, typically with `switch`/`if-else` (see [[06 - Building an HTTP Server with the http Module]]).
3. **Extract query values** as needed inside the matched route — e.g. to look up a specific record in a database, filter a list, or process a search term.

```js
const server = http.createServer((req, res) => {
  const { pathname, searchParams } = new URL(req.url, `http://${req.headers.host}`);

  if (pathname === '/search') {
    const term = searchParams.get('q');
    res.end(`Searching for: ${term}`);
  } else {
    res.end('Home');
  }
});
```

## Common mistakes

- **Forgetting the base URL argument** to `new URL(req.url)` — since `req.url` is relative (no protocol/domain), this throws `TypeError: Invalid URL`. You must pass a base, typically built from `req.headers.host`.
- **Routing against the raw `req.url`** instead of the parsed `pathname` — breaks the moment any request includes a query string.
- **Using `url.parse()` without the `true` flag** — without it, `.query` comes back as a raw, unparsed string instead of a usable object.
- Assuming query parameter values are anything other than strings — `searchParams.get('page')` returns `"2"` (a string), not the number `2`; convert explicitly with `Number(...)` if you need to do arithmetic with it.

## Related concepts
[[06 - Building an HTTP Server with the http Module]]
[[08 - HTTP Methods & Method-Based Routing in Node.js]]
[[REST APIs]] — URI design best practices (nouns not verbs, query params for filters) for once you're designing real endpoints
