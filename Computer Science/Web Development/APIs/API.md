---
tags: [api, web-development, backend, networking, computer-science, moc, master-guide]
aliases: [APIs, Types of APIs, API MOC, API Architecture]
created: 2026-08-09
updated: 2026-08-16
---

# API — Architecture, Protocols & Master Guide

An **API (Application Programming Interface)** is a formalized software contract that enables two independent systems to exchange data and execute logic without either system needing to understand the underlying implementation, database schema, or memory layout of the other.

---

## 🖼️ System Architecture Diagram

![[api-architecture-diagram.svg|960]]

---

## 🍽️ The Core Mental Model (The Restaurant Analogy)

To understand why APIs exist and how they work, consider a restaurant:

```mermaid
flowchart TD
    User["Client: browser or mobile app"]
    API["API contract: request, validation, response"]
    Server["Server: business logic"]
    DB["Database or external service"]

    User -->|"1. Sends request"| API
    API -->|"2. Calls server logic"| Server
    Server -->|"3. Reads or writes data"| DB
    DB -->|"4. Returns data"| Server
    Server -->|"5. Builds response"| API
    API -->|"6. Sends response"| User

    classDef client fill:#DBEAFE,stroke:#2563EB,color:#0F172A,stroke-width:2px
    classDef api fill:#FEF3C7,stroke:#D97706,color:#0F172A,stroke-width:2px
    classDef server fill:#DCFCE7,stroke:#16A34A,color:#0F172A,stroke-width:2px
    classDef data fill:#FCE7F3,stroke:#DB2777,color:#0F172A,stroke-width:2px
    class User client
    class API api
    class Server server
    class DB data
```

> [!NOTE]
> **Why this matters:** The customer never walks into the kitchen to chop onions (direct database access). As long as the menu (API documentation) remains unchanged, the chef can reorganize the kitchen, upgrade appliances, or change recipes internally without confusing the customer.

---

## 🧭 The Two Independent Dimensions of APIs

Every API in the software industry is classified across two **independent axes**:

```mermaid
flowchart TD
    Root["API Classification"]
    Root --> Protocol["Protocol and style"]
    Root --> Access["Scope and access"]

    Protocol --> REST["REST: HTTP plus JSON"]
    Protocol --> GQL["GraphQL: client-shaped query"]
    Protocol --> GRPC["gRPC: HTTP/2 plus Protobuf"]
    Protocol --> WS["WebSocket: persistent two-way pipe"]
    Protocol --> SOAP["SOAP: XML plus WSDL contract"]

    Access --> Pub["Public: open external consumers"]
    Access --> Priv["Private: internal services"]
    Access --> Part["Partner: contracted B2B access"]
    Access --> Comp["Composite: combines multiple APIs"]

    classDef root fill:#EDE9FE,stroke:#7C3AED,color:#111827,stroke-width:2px
    classDef axis fill:#DBEAFE,stroke:#2563EB,color:#111827,stroke-width:2px
    classDef proto fill:#DCFCE7,stroke:#16A34A,color:#111827,stroke-width:2px
    classDef access fill:#FEF3C7,stroke:#D97706,color:#111827,stroke-width:2px
    class Root root
    class Protocol,Access axis
    class REST,GQL,GRPC,WS,SOAP proto
    class Pub,Priv,Part,Comp access
```

---

## ⚖️ Comprehensive Protocol Comparison Matrix

| Protocol | Transport | Serialization | Communication Style | Performance | Best Used For | Dedicated Note |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **REST** | HTTP/1.1 or HTTP/2 | JSON, XML, Text | Request / Response (Stateless) | ⭐⭐⭐ | Standard CRUD web services, public APIs | [[REST APIs]] |
| **GraphQL** | HTTP (POST) | JSON | Request / Response (Client-shaped) | ⭐⭐⭐ | Complex frontend dashboards, mobile apps | [[GraphQL]] |
| **gRPC** | HTTP/2 | Protobuf (Binary) | Unary & Bi-directional Streaming | ⭐⭐⭐⭐⭐ | Internal high-throughput microservices | [[gRPC]] |
| **WebSocket** | TCP (Upgraded HTTP) | Text / Binary frames | Persistent Full-Duplex Bi-directional | ⭐⭐⭐⭐ | Real-time chat, multiplayer games, live tickers | [[WebSocket]] |
| **SOAP** | HTTP, SMTP, TCP | XML (Strict Envelope) | Request / Response | ⭐⭐ | Legacy banking, insurance, enterprise transactions | [[SOAP]] |

---

## 🎯 How to Choose the Right API Style

```mermaid
flowchart TD
    Start["What is your primary requirement?"] --> Q1{"Who is the consumer?"}
    
    Q1 -->|Browser, mobile, or public| Q2{"Data shape requirement?"}
    Q1 -->|Internal microservices| Q3{"Low latency and typed contract?"}
    Q1 -->|Real-time two-way push| WS_Pick["Choose WebSocket"]

    Q2 -->|Standard CRUD and HTTP caching| REST_Pick["Choose REST"]
    Q2 -->|Nested client-shaped data| GQL_Pick["Choose GraphQL"]

    Q3 -->|Yes| GRPC_Pick["Choose gRPC"]
    Q3 -->|Formal WSDL contract required| SOAP_Pick["Choose SOAP"]

    classDef question fill:#E0F2FE,stroke:#0284C7,color:#0F172A,stroke-width:2px
    classDef pick fill:#DCFCE7,stroke:#16A34A,color:#0F172A,stroke-width:2px
    classDef start fill:#F3E8FF,stroke:#9333EA,color:#0F172A,stroke-width:2px
    class Start start
    class Q1,Q2,Q3 question
    class WS_Pick,REST_Pick,GQL_Pick,GRPC_Pick,SOAP_Pick pick
```

---

## 🔗 Deep-Dive Vault Notes
- [[REST APIs]] — Resources, HTTP verbs, status codes, and idempotency (Featuring ![[nodejs.svg|16]] Express & ![[fastapi-rest.svg|16]] FastAPI implementations)
- ![[graphql-logo.png|16]] [[GraphQL]] — Schema definition language, resolvers, and avoiding N+1 queries
- ![[grpc-logo.svg|16]] [[gRPC]] — Protocol Buffers, `.proto` compilation, and HTTP/2 streaming
- ![[websocket.svg|16]] [[WebSocket]] — Handshake upgrade, duplex messaging, and connection management
- [[SOAP]] — XML Envelopes, WSDL contracts, and enterprise WS-Security

## 🧠 API Selection Cheat Flow

```mermaid
flowchart TD
    Need["Need system communication"] --> Realtime{"Continuous real-time updates?"}
    Realtime -->|Yes| WebSocket["Use WebSocket"]
    Realtime -->|No| Consumer{"Main consumer?"}
    Consumer -->|Public / Browser / Mobile| Shape{"Client needs flexible nested data?"}
    Shape -->|Yes| GraphQL["Use GraphQL"]
    Shape -->|No| REST["Use REST"]
    Consumer -->|Internal services| Perf{"Strict latency + typed contract?"}
    Perf -->|Yes| GRPC["Use gRPC"]
    Perf -->|No| RESTInternal["REST is acceptable"]
    Consumer -->|Legacy enterprise| SOAPPick["Use SOAP when WSDL/XML contract is required"]

    classDef decision fill:#E0F2FE,stroke:#0284C7,color:#0F172A,stroke-width:2px
    classDef choice fill:#DCFCE7,stroke:#16A34A,color:#0F172A,stroke-width:2px
    classDef start fill:#F3E8FF,stroke:#9333EA,color:#0F172A,stroke-width:2px
    class Need start
    class Realtime,Consumer,Shape,Perf decision
    class WebSocket,GraphQL,REST,GRPC,RESTInternal,SOAPPick choice
```
