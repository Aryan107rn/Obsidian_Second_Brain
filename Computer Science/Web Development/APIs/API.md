---
tags: [api, web-development, backend, networking, computer-science, moc]
aliases: [APIs, Types of APIs]
created: 2026-08-09
updated: 2026-08-09
---

# API

## What is it?

An **API (Application Programming Interface)** is a contract that lets two pieces of software communicate without either one needing to know how the other is built internally. When you check the weather in an app, the app isn't storing weather data itself — it sends a request to a weather API and gets a response back. The API defines *what* requests are allowed and *what shape* the responses take, hiding all the internal implementation.

APIs come in different flavors depending on two independent questions:

1. **How is it built?** — the architecture/protocol (REST, GraphQL, SOAP, gRPC, WebSocket)
2. **Who can access it?** — the scope (public, internal, partner, composite)

A REST API can be public or private; a public API can be REST or GraphQL. These two classifications are separate axes, not a single hierarchy.

## Why does it exist?

Without APIs, every piece of software that wanted to use another service's data or functionality would need to understand that service's internal database, code, and infrastructure — and any internal change would break every consumer. An API is a stable, documented boundary: the internals can change freely as long as the contract (the API) stays the same.

---

## Classification by architecture / protocol

Each style is covered in full depth in its own note:

- [[REST APIs]] — resources manipulated via HTTP methods (`GET`/`POST`/`PUT`/`PATCH`/`DELETE`), stateless, JSON. The default for general-purpose public and internal web APIs.
- [[GraphQL]] — client specifies exactly what data shape it wants in a single query, over one endpoint. Solves REST's over-fetching/under-fetching problem; best for complex nested data and multiple client types.
- [[SOAP]] — strict, XML-based protocol with a formal WSDL contract and built-in security/transaction standards. Common in banking, insurance, and government/legacy enterprise systems.
- [[gRPC]] — high-performance RPC framework using binary Protocol Buffers over HTTP/2. Built for fast internal microservice-to-microservice communication, not public/browser-facing APIs.
- [[WebSocket]] — a persistent, two-way connection instead of request-response. Built for real-time push: chat, live dashboards, multiplayer games, collaborative editing.

### Choosing between them

| Need | Choice |
|---|---|
| Public-facing, general-purpose | [[REST APIs\|REST]] |
| Flexible/nested data, many client types | [[GraphQL]] |
| Enterprise/legacy system, strict contracts | [[SOAP]] |
| Fast internal microservice-to-microservice calls | [[gRPC]] |
| Real-time, two-way updates | [[WebSocket]] |

---

## Classification by access / scope

This is an orthogonal axis — it describes *who* can call the API, regardless of which protocol above it uses.

| Type | Who can use it | Example |
|---|---|---|
| **Open / Public API** | Anyone, often with an API key | Google Maps API, OpenWeather API |
| **Internal / Private API** | Only within the same company/system | A company's internal service-to-service API |
| **Partner API** | Specific external businesses with an agreement | An airline exposing booking APIs to travel agencies |
| **Composite API** | Combines multiple underlying API calls into one request, reducing round trips | A "checkout" endpoint that internally calls inventory, payment, and shipping APIs at once |

---

## Common mistakes

- Treating "REST" and "API" as synonyms — REST is one style among several.
- Choosing GraphQL by default for its flexibility without accounting for its weaker HTTP-level cacheability and added server complexity.
- Using gRPC for a public-facing API where browser clients need direct access (poor native browser support without a grpc-web proxy).
- Building a WebSocket connection for data that only changes occasionally — polling a REST endpoint is often simpler and sufficient.
- Assuming SOAP is obsolete — it's still the standard in much of banking, insurance, and government backend integration.

## Related Concepts
- [[REST APIs]], [[GraphQL]], [[SOAP]], [[gRPC]], [[WebSocket]] — each architecture style in depth
- [[HTTP]] — the transport protocol REST, GraphQL, SOAP (usually), and the WebSocket handshake are built on
- [[JSON]] — dominant data format for REST/GraphQL payloads
- [[Client-Server Architecture]]

## Open Questions / To Explore Later
- Authentication patterns across API types (API keys, OAuth, JWT)
- Rate limiting and API versioning strategies
- Building a small GraphQL vs REST example against the same dataset
