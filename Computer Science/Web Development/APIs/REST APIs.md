---
tags: [api, rest, web-development, backend, networking, computer-science, placement-prep, interview-favorite]
aliases: [REST, RESTful API, Representational State Transfer, HTTP REST]
created: 2026-08-08
updated: 2026-08-14
---

# REST APIs — Comprehensive Architecture & Design Guide

A **REST API** (Representational State Transfer) is an architectural style designed by Roy Fielding in 2000 for distributed hypermedia systems. It is not a rigid library or protocol, but rather a set of **design principles** that map operations on resources to standard [[HTTP]] methods.

---

## 🖼️ RESTful Request & Response Lifecycle Diagram

![[rest-architecture-diagram.svg|960]]

---

## 🏛️ The 6 Architectural Constraints of REST

```mermaid
flowchart TD
    subgraph Constraints["Fielding's 6 Constraints for True RESTful Systems"]
        C1["<b>1. Client-Server Separation</b><br/>UI concerns are separated from data storage"]
        C2["<b>2. Statelessness</b><br/>No client session context stored on server between calls"]
        C3["<b>3. Cacheability</b><br/>Responses must explicitly define if/how they can be cached"]
        C4["<b>4. Uniform Interface</b><br/>Resources identified via URIs; standardized HTTP verbs"]
        C5["<b>5. Layered System</b><br/>Client cannot tell if connected to end-server or proxy/cache"]
        C6["<b>6. Code on Demand (Optional)</b><br/>Servers can extend client logic by sending executable code"]
    end
```

---

## 🔄 HTTP Verbs: Safety vs. Idempotency

```mermaid
flowchart LR
    subgraph SafeZone["Safe Methods (Read-Only)"]
        direction TB
        GET["GET"]
        HEAD["HEAD"]
        OPTIONS["OPTIONS"]
        SafeNote["• Do NOT mutate server state<br/>• Safe to pre-fetch or cache"]
    end

    subgraph IdempotentZone["Idempotent Methods (f(f(x)) = f(x))"]
        direction TB
        PUT["PUT (Full Replace)"]
        DELETE["DELETE (Remove)"]
        IdemNote["• Calling 1 time or 100 times<br/>  produces identical server state"]
    end

    subgraph NonIdempotentZone["Non-Idempotent (State Mutating)"]
        direction TB
        POST["POST (Create / Append)"]
        PATCH["PATCH (Partial Update*)"]
        NonIdemNote["• Calling N times creates N resources<br/>  or applies N relative increments"]
    end
```

### Complete HTTP Methods Matrix

| Method | CRUD Equivalent | Safe? | Idempotent? | Body in Request? | Response Cachable? | Example URI |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| `GET` | Read | ✅ Yes | ✅ Yes | ❌ No | ✅ Yes | `GET /articles/101` |
| `POST` | Create | ❌ No | ❌ No | ✅ Yes | ⚠️ Rarely | `POST /articles` |
| `PUT` | Replace / Upsert | ❌ No | ✅ Yes | ✅ Yes | ❌ No | `PUT /articles/101` |
| `PATCH` | Partial Update | ❌ No | ⚠️ Contextual | ✅ Yes | ❌ No | `PATCH /articles/101` |
| `DELETE` | Delete | ❌ No | ✅ Yes | ❌ Optional | ❌ No | `DELETE /articles/101` |
| `HEAD` | Read Headers | ✅ Yes | ✅ Yes | ❌ No | ✅ Yes | `HEAD /articles/101` |
| `OPTIONS`| Query Capabilities| ✅ Yes | ✅ Yes | ❌ No | ❌ No | `OPTIONS /articles` |

> [!IMPORTANT]
> **PUT vs. PATCH Difference:**
> - `PUT /users/42` replaces the **entire user object**. Any fields you omit are wiped or reset to defaults.
> - `PATCH /users/42` updates **only the fields provided** in the payload (e.g. `{ "email": "new@site.com" }`), leaving other fields untouched.

---

## 🚦 HTTP Status Codes Cheat Sheet

```mermaid
flowchart TD
    Code["HTTP Response Status Code"]
    
    Code --> 2xx["<b>2xx: Success</b><br/>• <b>200 OK:</b> Standard success (GET/PUT/PATCH)<br/>• <b>201 Created:</b> Resource created (POST)<br/>• <b>204 No Content:</b> Succeeded, empty body (DELETE)"]
    
    Code --> 3xx["<b>3xx: Redirection</b><br/>• <b>301 Moved Permanently:</b> Permanent redirect<br/>• <b>304 Not Modified:</b> Cached copy is valid"]
    
    Code --> 4xx["<b>4xx: Client Error</b><br/>• <b>400 Bad Request:</b> Malformed syntax / validation failure<br/>• <b>401 Unauthorized:</b> Missing / invalid authentication token<br/>• <b>403 Forbidden:</b> Authenticated, but lacking permission<br/>• <b>404 Not Found:</b> Resource does not exist<br/>• <b>409 Conflict:</b> State conflict (e.g. duplicate email)<br/>• <b>429 Too Many Requests:</b> Rate limit exceeded"]
    
    Code --> 5xx["<b>5xx: Server Error</b><br/>• <b>500 Internal Error:</b> Unhandled server crash<br/>• <b>502 Bad Gateway:</b> Upstream service failure<br/>• <b>503 Service Unavailable:</b> Overloaded or maintenance<br/>• <b>504 Gateway Timeout:</b> Upstream server timed out"]
```

---

## ⚖️ Statelessness & Horizontal Scaling

```mermaid
flowchart TD
    Client["Client (Sends Request + JWT Token)"]
    LB["Load Balancer (Round Robin)"]
    
    subgraph AppCluster["Stateless Backend Pool"]
        S1["Server Instance #1"]
        S2["Server Instance #2"]
        S3["Server Instance #3"]
    end
    
    DB[(Shared Database)]
    Cache[(Shared Redis Session Cache)]

    Client --> LB
    LB -->|Req 1| S1
    LB -->|Req 2| S3
    LB -->|Req 3| S2

    S1 <--> Cache
    S2 <--> Cache
    S3 <--> Cache
    S1 <--> DB
    S2 <--> DB
    S3 <--> DB
```

---

## 📐 REST URI Design Rules & Best Practices

| Best Practice Rule | ✅ Good URI Example | ❌ Bad / Anti-Pattern Example | Why? |
| :--- | :--- | :--- | :--- |
| **Use Nouns, Not Verbs** | `GET /orders` | `GET /getOrders` | HTTP verbs (`GET`/`POST`) already define the action |
| **Plural Naming** | `GET /users/5` | `GET /user/5` | Consistency across collection vs. singleton routes |
| **Represent Hierarchy** | `GET /authors/4/books` | `GET /getBooksByAuthor?id=4` | Sub-resources naturally show parent-child ownership |
| **Filter via Query Params** | `GET /products?category=shoes&sort=price_asc` | `GET /products/shoes/sortPrice` | Keep base resource clean; use query strings for filters |
| **Use Kebab-Case** | `GET /order-history` | `GET /order_history` or `/orderHistory` | Web URIs are case-insensitive and standardized on dashes |

---

## 🚀 REST Implementation Examples

To make REST concrete, here is how you build a standard `GET /users/:id` endpoint in two of the most popular modern ecosystems.

### ![[nodejs.svg|24]] Node.js (Express)
```javascript
const express = require('express');
const app = express();
const PORT = 3000;

// Mock database
const users = {
  42: { id: 42, name: "Alice", email: "alice@example.com" }
};

// REST Endpoint: GET resource by ID
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  const user = users[userId];
  
  if (user) {
    res.status(200).json(user); // 200 OK with JSON payload
  } else {
    res.status(404).json({ error: "User not found" }); // 404 Not Found
  }
});

app.listen(PORT, () => console.log(`REST Server running on port ${PORT}`));
```

### ![[fastapi-rest.svg|24]] Python (FastAPI)
```python
from fastapi import FastAPI, HTTPException, status
from pydantic import BaseModel

app = FastAPI()

# Resource Schema
class User(BaseModel):
    id: int
    name: str
    email: str

# Mock database
users = {
    42: User(id=42, name="Alice", email="alice@example.com")
}

# REST Endpoint: GET resource by ID
@app.get("/users/{user_id}", response_model=User, status_code=status.HTTP_200_OK)
def get_user(user_id: int):
    user = users.get(user_id)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND, 
            detail="User not found"
        )
    return user
```

---

## 🔗 Related Vault Concepts
- [[API]] — The overarching classification guide (REST vs GraphQL vs gRPC vs WebSocket)
- [[GraphQL]] — How GraphQL solves REST over-fetching and under-fetching
- [[gRPC]] — The binary high-performance RPC alternative for microservices
- [[WebSocket]] — For real-time bi-directional messaging where REST polling is inefficient
