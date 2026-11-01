---
tags: [api, web-development, backend, networking, computer-science, moc, master-guide]
aliases: [APIs, Types of APIs, API MOC, API Architecture]
created: 2026-08-09
updated: 2026-08-31
---

# API — Architecture, Protocols & Master Guide

An **API (Application Programming Interface)** is a formalized software contract that enables two independent systems to exchange data and execute logic without either system needing to understand the underlying implementation, database schema, or memory layout of the other.

---

## 🖼️ System Architecture Diagram

![[api-architecture-diagram.svg|960]]

---

## 🍽️ The Core Mental Model (The Restaurant Analogy)

To understand why APIs exist and how they work, consider a restaurant:

![[SVG Diagrams/Computer-Science--Web-Development--Backend--APIs--API--mermaid-01.svg]]

> [!NOTE]
> **Why this matters:** The customer never walks into the kitchen to chop onions (direct database access). As long as the menu (API documentation) remains unchanged, the chef can reorganize the kitchen, upgrade appliances, or change recipes internally without confusing the customer.

---

## 🧭 The Two Independent Dimensions of APIs

Every API in the software industry is classified across two **independent axes**:

![[SVG Diagrams/Computer-Science--Web-Development--Backend--APIs--API--mermaid-02.svg]]

---

## ⚖️ Comprehensive Protocol Comparison Matrix

| Protocol | Transport | Serialization | Communication Style | Performance | Best Used For | Dedicated Note |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **REST** | HTTP/1.1 or HTTP/2 | JSON, XML, Text | Request / Response (Stateless) | ⭐⭐⭐ | Standard CRUD web services, public APIs | [[REST APIs]] |
| **GraphQL** | HTTP (POST) | JSON | Request / Response (Client-shaped) | ⭐⭐⭐ | Complex frontend dashboards, mobile apps | [[GraphQL]] |
| **gRPC** | HTTP/2 | Protobuf (Binary) | Unary & Bi-directional Streaming | ⭐⭐⭐⭐⭐ | Internal high-throughput microservices | [[gRPC]] |
| **WebSocket** | TCP (Upgraded HTTP) | Text / Binary frames | Persistent Full-Duplex Bi-directional | ⭐⭐⭐⭐ | Real-time chat, multiplayer games, live tickers | [[WebSocket]] |
| **SOAP** | HTTP, SMTP, TCP | XML (Strict Envelope) | Request / Response | ⭐⭐ | Legacy banking, insurance, enterprise transactions | [[SOAP]] |

> For the transport-layer distinction between HTTP/1.1 and HTTP/2 referenced above, plus the new `QUERY` method, see [[HTTP Versions & the QUERY Method]].

---

## 🔐 Security and Identity

- [[Authentication and Authorization]] — authentication vs. authorization, sessions and tokens, and server-side permission checks
- [[JWT]] — token structure, claim validation, and security trade-offs

## 🎯 How to Choose the Right API Style

![[SVG Diagrams/Computer-Science--Web-Development--Backend--APIs--API--mermaid-03.svg]]

---

## 🔗 Deep-Dive Vault Notes

- [[REST APIs]] — Resources, HTTP verbs, status codes, and idempotency (Featuring ![[nodejs.svg|16]] Express & ![[fastapi-rest.svg|16]] FastAPI implementations)
- ![[graphql-logo.png|16]] [[GraphQL]] — Schema definition language, resolvers, and avoiding N+1 queries
- ![[grpc-logo.svg|16]] [[gRPC]] — Protocol Buffers, `.proto` compilation, and HTTP/2 streaming
- ![[websocket.svg|16]] [[WebSocket]] — Handshake upgrade, duplex messaging, and connection management
- [[SOAP]] — XML Envelopes, WSDL contracts, and enterprise WS-Security
- [[HTTP Versions & the QUERY Method]] — HTTP/1.1 vs HTTP/2 (multiplexing, HPACK, binary framing), and the new `QUERY` method (RFC 10008, 2026)

## 🧠 API Selection Cheat Flow

![[SVG Diagrams/Computer-Science--Web-Development--Backend--APIs--API--mermaid-04.svg]]
