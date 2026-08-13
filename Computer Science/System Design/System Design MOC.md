System design interview prep tracker. Source: uploaded roadmap + gaps identified for full 2026 interview readiness. Check items off and turn into `[[links]]` as notes are created — don't create notes ahead of learning them.

Legend: ⬜ not covered · 🟨 covered in chat, not saved · ✅ note exists

## Tier 1 — Must know (core interview readiness)

### Fundamentals & Requirements
- ⬜ Functional vs non-functional requirements
- ⬜ Architecture styles (monolith, microservices, event-driven, layered, hexagonal, serverless)
- ⬜ Scaling (vertical/horizontal, stateless vs stateful, autoscaling)
- ⬜ Back-of-the-envelope estimation (QPS, storage, bandwidth)

### Networking & APIs
- ⬜ Core networking (TCP/UDP, HTTP/HTTPS, TLS, DNS)
- ⬜ HTTP deep dive (methods, status codes, HTTP/1.1 vs 2 vs 3)
- ⬜ REST API design (resources, pagination — offset/cursor/keyset, versioning, rate limiting)
- ⬜ GraphQL (N+1 problem, when to use vs REST)
- ⬜ gRPC (protobuf, streaming, REST vs gRPC)
- ⬜ WebSockets & real-time communication
- ⬜ Long polling / SSE / polling comparison

### Databases
- ⬜ SQL fundamentals (joins, transactions, indexes)
- ⬜ Database internals (B-trees, LSM trees, ACID, isolation levels, locking)
- ⬜ SQL vs NoSQL decision framework
- ⬜ Database scaling (replication, sharding, partitioning)
- ⬜ CAP theorem
- ⬜ Consistency models (strong, eventual, causal, quorum)

### Caching & Load Balancing
- ⬜ Caching (strategies, eviction, cache stampede/penetration/avalanche)
- ⬜ Load balancing (L4 vs L7, algorithms, consistent hashing)
- ⬜ CDN

### Messaging & Reliability
- ⬜ Message queues (Kafka, RabbitMQ, SQS concepts)
- ⬜ Kafka deep dive (partitions, consumer groups, delivery guarantees)
- ⬜ Message delivery semantics & idempotency
- ⬜ Rate limiting algorithms (token bucket, sliding window, etc.)
- ⬜ Reliability patterns (retry, backoff, circuit breaker, bulkhead)
- ⬜ Microservices trade-offs

## Tier 2 — Very important

- ⬜ Distributed systems fundamentals (partial failure, failure detection)
- ⬜ Consensus (Raft, Paxos — conceptual)
- ⬜ Consistent hashing (deep dive)
- ⬜ Service discovery
- ⬜ API gateway & reverse proxy
- ⬜ Distributed locking
- ⬜ Distributed transactions (2PC, Saga, orchestration vs choreography)
- ⬜ CQRS
- ⬜ Event sourcing
- ⬜ Observability (logs, metrics, traces, SLIs/SLOs)
- ⬜ Security (authN/authZ, OAuth2, OIDC, encryption, CORS/CSRF/XSS)
- ⬜ Secrets management
- ⬜ High availability & disaster recovery (RPO/RTO)
- ⬜ Multi-region architecture
- ⬜ Object storage & file upload systems (multipart, pre-signed URLs)
- ⬜ Search systems (inverted index, Elasticsearch)
- ⬜ Notification systems & fanout (fanout-on-write vs read)
- ⬜ Scheduling systems (distributed cron, job locking)
- ⬜ Distributed ID generation (Snowflake, UUID)
- ⬜ Distributed counters

## Tier 3 — Advanced / senior depth

- ⬜ Vector clocks & logical clocks
- ⬜ Bloom filters
- ⬜ HyperLogLog / Count-Min Sketch
- ⬜ Merkle trees
- ⬜ Stream processing (Kafka Streams, Flink, windowing)
- ⬜ Batch processing (MapReduce)
- ⬜ Data warehousing (OLTP vs OLAP, ETL/ELT)
- ⬜ Data pipelines (batch vs streaming trade-offs)
- ⬜ Kubernetes (pods, services, deployments, HPA)
- ⬜ Containers (Docker fundamentals)

## Gaps not in the original roadmap (added)

- ⬜ **ML-adjacent system design** — feature stores, model serving, recommendation system architecture
- ⬜ **Deployment strategies** — blue-green, canary, rolling deploys
- ⬜ **Schema migration strategies** — backward-compatible changes, zero-downtime migrations
- ⬜ **Feature flags / experimentation platforms** — A/B testing infrastructure
- ⬜ **Data privacy & compliance** — GDPR, data residency, PII handling, right-to-be-forgotten
- ⬜ **Chaos engineering** — fault injection, game days
- ⬜ **NewSQL / geo-distributed relational DBs** — Spanner, CockroachDB, Vitess
- ⬜ **Client-side / offline-first design** — sync, conflict resolution (CRDTs), relevant for collaborative editors
- ⬜ **Load testing tools & practice** — k6, JMeter, running actual capacity tests
- ⬜ **Time-series databases** — as a storage category (InfluxDB, TimescaleDB), distinct from generic metrics tools

## Practice problems (from roadmap)

**Beginner:** URL shortener · Pastebin · File storage · Rate limiter · Notification service · Web crawler · Key-value store

**Intermediate:** Twitter/X · Instagram · WhatsApp · YouTube · Netflix · Uber · Swiggy/Zomato · Google Drive · Dropbox · Ticket booking · E-commerce · Chat system

**Advanced:** Payment system · Distributed cache · Distributed message queue · Search engine · Distributed scheduler · Google Docs (collaborative editor) · Google Maps · Ad-serving · News feed · Ride matching · Multi-region database · Real-time collaboration

## How this MOC works

When we cover a topic in chat and you ask to save it, the note gets created in this folder and the corresponding checkbox above is updated to ✅ with a link, e.g. `- ✅ [[Caching]]`. Notes here follow the same depth/structure rules as the rest of the vault (beginner-friendly, complete, diagrams where useful).

## Related
- System design is a separate track from DSA prep — per the roadmap: "System design does not replace DSA."
