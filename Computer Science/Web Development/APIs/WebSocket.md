---
tags: [api, websocket, web-development, backend, networking, computer-science, realtime, placement-prep, interview-favorite]
aliases: [WebSockets, WS, WSS, Full Duplex]
created: 2026-08-09
updated: 2026-08-14
---

# ![[websocket.svg|40]] WebSocket — Full-Duplex Real-Time Networking

**WebSocket** is a standardized computer communications protocol that provides **full-duplex, bidirectional communication channels** over a single, persistent TCP connection. Standardized by the IETF as RFC 6455 in 2011, WebSocket enables the server to push real-time events to the client instantly without the client needing to poll.

---

## 🖼️ WebSocket Full-Duplex Connection Diagram

![[websocket-lifecycle-diagram.svg|960]]

---

## ⚡ The Real-Time Dilemma: 4 Approaches Compared

```mermaid
sequenceDiagram
    autonumber
    box LightYellow 1. Short Polling (Wasteful & Delayed)
    participant C1 as Client
    participant S1 as Server
    end
    C1->>S1: GET /messages (Any new data?)
    S1-->>C1: 200 OK: "No"
    Note over C1: Wait 3s...
    C1->>S1: GET /messages (Any new data?)
    S1-->>C1: 200 OK: "No"

    box LightBlue 2. Long Polling (Hangs open until event)
    participant C2 as Client
    participant S2 as Server
    end
    C2->>S2: GET /messages (Hanging request)
    Note over S2: Waits until new message arrives...
    S2-->>C2: 200 OK: { message: "Hello" }
    C2->>S2: GET /messages (Immediately opens next hanging request)

    box LightGreen 3. Server-Sent Events (SSE - One-Way Push)
    participant C3 as Client
    participant S3 as Server
    end
    C3->>S3: GET /events (text/event-stream)
    S3-->>C3: Stream Open
    S3-->>C3: Event 1 (Push)
    S3-->>C3: Event 2 (Push)

    box LightPink 4. WebSocket (Full-Duplex Two-Way Pipe)
    participant C4 as Client
    participant S4 as Server
    end
    C4->>S4: HTTP Upgrade Handshake
    S4-->>C4: 101 Switching Protocols
    Note over C4,S4: Bi-directional Persistent TCP Pipe
    C4->>S4: Client Frame (2-6 bytes overhead)
    S4-->>C4: Server Push Frame (Instant!)
```

---

## 🚀 Practical WebSocket Implementation Example

To illustrate the full-duplex communication cycle, here is how a connection is established and used:

### 1. The Client-Side Browser Connection
```javascript
// 1. Establish persistent connection to WebSocket Server
const socket = new WebSocket('ws://localhost:8080');

// 2. Event: Connection opened (Handshake completed successfully)
socket.onopen = (event) => {
  console.log('Connected! Handshake complete.');
  // Client can send message packets containing any text/JSON or binary data
  socket.send(JSON.stringify({ type: 'greet', text: 'Hello Server!' }));
};

// 3. Event: Server pushed a message to the client (Real-time Push)
socket.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Message from server:', data);
};

// 4. Event: Connection closed
socket.onclose = () => {
  console.log('Connection closed by remote peer.');
};
```

### 2. The Server-Side Handler (Node.js - `ws` library)
```javascript
const WebSocket = require('ws');

// Instantiate a persistent TCP server port
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  console.log('New client joined!');

  // Listen for message packets from this specific client connection
  ws.on('message', (rawPayload) => {
    const payload = JSON.parse(rawPayload);
    console.log(`Received: ${payload.text}`);

    // Instantly push data back over the SAME socket line
    ws.send(JSON.stringify({ 
      sender: 'server', 
      text: `Echo: "${payload.text}"` 
    }));
  });

  // Welcome push immediately on connection
  ws.send(JSON.stringify({ sender: 'server', text: 'Connected to Real-Time Core!' }));
});
```

---

## 🌐 Scaling WebSockets: The Horizontal Architecture

Because WebSockets are **stateful** (a connection is tied to a specific physical server memory), scaling across multiple instances requires a **Pub/Sub message broker**:

```mermaid
flowchart TD
    ClientA["📱 Client A (Connected to Server 1)"]
    ClientB["💻 Client B (Connected to Server 2)"]
    
    LB["Load Balancer (Sticky Sessions / IP Hash)"]
    
    subgraph WebSockets["WebSocket Server Fleet"]
        WS1["WebSocket Server Instance #1"]
        WS2["WebSocket Server Instance #2"]
    end

    Broker[("📡 Redis Pub/Sub / RabbitMQ Backplane")]

    ClientA <==>|Persistent WS| WS1
    ClientB <==>|Persistent WS| WS2

    WS1 <-->|"1. Client A sends message -> Publishes to channel"| Broker
    Broker <-->|"2. Broadcasts message to all server nodes"| WS2
    WS2 -->|"3. Pushes to Client B!"| ClientB
```

---

## 📊 Comparison: WebSocket vs. SSE vs. Long Polling

| Feature | WebSocket | Server-Sent Events (SSE) | Long Polling |
| :--- | :--- | :--- | :--- |
| **Communication** | Bi-directional (Full-Duplex) | Uni-directional (Server → Client) | Simulated (Request/Response) |
| **Transport** | TCP (Upgraded from HTTP) | HTTP/1.1 or HTTP/2 | Standard HTTP |
| **Data Format** | Text (JSON/UTF-8) & Binary (Buffers) | UTF-8 Text only | Text / JSON |
| **Auto-Reconnect** | Manual implementation | Built into browser `EventSource` | Manual implementation |
| **Firewall / Proxy Friendly** | Sometimes blocked by corporate proxies | 100% standard HTTP (Never blocked) | 100% standard HTTP |
| **Best For** | Gaming, live chat, shared whiteboard | Live score tickers, notification feeds | Legacy fallback systems |

---

## 🔗 Related Vault Concepts
- [[API]] — Master API architecture comparison
- [[REST APIs]] — The stateless alternative for standard CRUD
- [[GraphQL]] — Uses WebSocket for GraphQL Subscriptions
- [[System Design MOC]] — Scaling real-time distributed chat systems
