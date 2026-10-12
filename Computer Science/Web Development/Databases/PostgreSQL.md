# PostgreSQL

## What is it?

PostgreSQL ("Postgres") is an open-source **relational database management system (RDBMS)**. It stores data in **tables** made of **rows** and **columns**, and you talk to it using **SQL** (Structured Query Language).

Think of it as a very strict, very powerful collection of spreadsheets:
- Each **table** is a sheet (e.g. `users`).
- Each **column** has a fixed name and data type (e.g. `email TEXT`).
- Each **row** is one record (e.g. one user).
- Sheets can be **linked** to each other (an `orders` row points to a `users` row), and the database enforces that the links are valid.

```
SQL / Postgres              MongoDB
──────────────              ───────
Database          ───────►  Database
Table             ───────►  Collection
Row               ───────►  Document
Column            ───────►  Field
Foreign key + JOIN ──────►  Embedded doc / manual reference
```

Postgres is a **client–server** database: a server process runs (default port **5432**), and clients (psql, your Node app, a GUI) connect to it over the network.

## Why does it exist? What problem does it solve?

Applications need to store data so that it:
1. **Survives restarts** (persistence).
2. **Stays correct** even when many users write at once, or the server crashes mid-write (consistency).
3. **Can be queried flexibly** ("all orders over ₹500 from users in Nagpur, newest first").
4. **Has enforced relationships** (you can't create an order for a user that doesn't exist).

Postgres solves all of these with:
- **ACID transactions** (see below) so money/inventory/bookings stay correct.
- **A strict schema** so bad data is rejected at the database level, not just in your app code.
- **Powerful querying** (JOINs, aggregation, window functions, full-text search, JSON queries).
- **Extensibility** (extensions like PostGIS for maps, pgvector for AI embeddings).

## Key concepts

| Term | Meaning |
|---|---|
| **Database** | A container for schemas/tables. One server can host many. |
| **Schema** | A namespace inside a database (default: `public`). Groups tables. |
| **Table** | Structured collection of rows with fixed columns. |
| **Row / tuple** | One record. |
| **Column** | A named, typed field. |
| **Primary key (PK)** | Column(s) that uniquely identify a row. Cannot be NULL or duplicate. |
| **Foreign key (FK)** | Column referencing another table's PK; enforces valid links. |
| **Constraint** | A rule the DB enforces (`NOT NULL`, `UNIQUE`, `CHECK`, PK, FK). |
| **Index** | A lookup structure that makes queries on a column fast. |
| **Transaction** | A group of statements that succeed or fail together. |
| **Role / user** | An account with permissions. |
| **psql** | Postgres's official command-line client. |
| **NULL** | "Unknown / no value". Not zero, not empty string. |

### ACID (why Postgres is trusted for important data)
- **Atomicity** — all statements in a transaction happen, or none do. (Transfer ₹100: debit AND credit, never just one.)
- **Consistency** — data always satisfies the constraints; invalid states are rejected.
- **Isolation** — concurrent transactions don't see each other's half-finished work.
- **Durability** — once committed, data survives a crash (written to the **WAL**, write-ahead log).

## Installing and connecting

```bash
# Ubuntu / Debian
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql

# Open a shell as the postgres superuser
sudo -u postgres psql
```

Or use Docker (cleanest for projects):
```bash
docker run --name pg -e POSTGRES_PASSWORD=secret -p 5432:5432 -d postgres:16
```

Hosted options: Neon, Supabase, Render, Railway, AWS RDS (these give you a connection string).

**Connection string format:**
```
postgresql://USER:PASSWORD@HOST:PORT/DATABASE
postgresql://app_user:secret@localhost:5432/shop
```

### Essential `psql` commands (these start with a backslash, not SQL)
| Command | Does |
|---|---|
| `\l` | list databases |
| `\c dbname` | connect to a database |
| `\dt` | list tables |
| `\d tablename` | describe a table (columns, indexes, constraints) |
| `\du` | list roles |
| `\dn` | list schemas |
| `\x` | toggle expanded output (useful for wide rows) |
| `\i file.sql` | run a SQL file |
| `\timing` | show query execution time |
| `\q` | quit |

## SQL syntax — from scratch

SQL statements end with `;`. Keywords are case-insensitive (convention: UPPERCASE keywords). Unquoted identifiers fold to lowercase. Strings use **single quotes** `'text'`; double quotes are for identifiers.

SQL is conventionally split into groups:
- **DDL** (Data Definition): `CREATE`, `ALTER`, `DROP` — shape of the data.
- **DML** (Data Manipulation): `INSERT`, `UPDATE`, `DELETE` — change the data.
- **DQL** (Query): `SELECT` — read the data.
- **DCL** (Control): `GRANT`, `REVOKE` — permissions.
- **TCL** (Transaction): `BEGIN`, `COMMIT`, `ROLLBACK`.

### Databases
```sql
CREATE DATABASE shop;
DROP DATABASE shop;          -- irreversible
```

### Data types (most important)
| Category | Types | Notes |
|---|---|---|
| Integer | `SMALLINT`, `INTEGER`, `BIGINT` | 2 / 4 / 8 bytes |
| Auto ID | `SERIAL`, `BIGSERIAL`, or `GENERATED ALWAYS AS IDENTITY` | Prefer `IDENTITY` in new code |
| Exact decimal | `NUMERIC(10,2)` | Use for money. Never use float for money |
| Float | `REAL`, `DOUBLE PRECISION` | Approximate |
| Text | `TEXT`, `VARCHAR(n)`, `CHAR(n)` | In Postgres `TEXT` is just as fast as `VARCHAR`; use `TEXT` unless you need a limit |
| Boolean | `BOOLEAN` | `TRUE` / `FALSE` / `NULL` |
| Date/time | `DATE`, `TIME`, `TIMESTAMP`, `TIMESTAMPTZ` | Prefer `TIMESTAMPTZ` (timezone-aware) |
| UUID | `UUID` | `gen_random_uuid()` generates one |
| JSON | `JSON`, `JSONB` | `JSONB` is binary, indexable, preferred |
| Array | `INTEGER[]`, `TEXT[]` | Native arrays |
| Enum | `CREATE TYPE ... AS ENUM` | Fixed set of values |

### CREATE TABLE
```sql
CREATE TABLE users (
    id          BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name        TEXT        NOT NULL,
    email       TEXT        NOT NULL UNIQUE,
    age         INTEGER     CHECK (age >= 0),
    is_active   BOOLEAN     NOT NULL DEFAULT TRUE,
    created_at  TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE orders (
    id          BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    user_id     BIGINT        NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    total       NUMERIC(10,2) NOT NULL CHECK (total >= 0),
    status      TEXT          NOT NULL DEFAULT 'pending',
    created_at  TIMESTAMPTZ   NOT NULL DEFAULT now()
);
```
`CREATE TABLE IF NOT EXISTS ...` avoids an error when it already exists.

### Constraints explained
| Constraint | Meaning |
|---|---|
| `PRIMARY KEY` | unique + not null; identifies the row |
| `NOT NULL` | value required |
| `UNIQUE` | no duplicates allowed (NULLs are allowed multiple times) |
| `CHECK (cond)` | custom rule, e.g. `price > 0` |
| `DEFAULT x` | value used if none supplied |
| `REFERENCES t(col)` | foreign key |

**Foreign key actions** (what happens when the referenced row is deleted):
- `ON DELETE CASCADE` — delete child rows too
- `ON DELETE SET NULL` — set the FK to NULL
- `ON DELETE RESTRICT` / `NO ACTION` (default) — block the delete

### ALTER / DROP
```sql
ALTER TABLE users ADD COLUMN phone TEXT;
ALTER TABLE users DROP COLUMN phone;
ALTER TABLE users RENAME COLUMN name TO full_name;
ALTER TABLE users ALTER COLUMN age SET NOT NULL;
ALTER TABLE users ALTER COLUMN age TYPE SMALLINT;
ALTER TABLE users ADD CONSTRAINT age_ok CHECK (age < 150);
ALTER TABLE users RENAME TO customers;

DROP TABLE orders;                 -- deletes table + data
DROP TABLE IF EXISTS orders CASCADE;  -- also drops dependent objects
TRUNCATE TABLE orders;             -- remove all rows fast, keep table
```

### INSERT
```sql
INSERT INTO users (name, email, age)
VALUES ('Aryan', 'aryan@example.com', 21);

-- multiple rows
INSERT INTO users (name, email) VALUES
  ('Riya', 'riya@example.com'),
  ('Sam',  'sam@example.com');

-- return the created row (very useful in apps)
INSERT INTO users (name, email) VALUES ('Neha', 'neha@example.com')
RETURNING id, created_at;

-- UPSERT: insert or update on conflict
INSERT INTO users (email, name) VALUES ('aryan@example.com', 'Aryan N')
ON CONFLICT (email) DO UPDATE SET name = EXCLUDED.name;

-- ignore duplicates
INSERT INTO users (email, name) VALUES ('aryan@example.com', 'X')
ON CONFLICT (email) DO NOTHING;
```
`EXCLUDED` refers to the row you tried to insert.

### SELECT (reading)
```sql
SELECT * FROM users;                       -- all columns (avoid in production)
SELECT name, email FROM users;
SELECT name AS full_name FROM users;       -- alias
SELECT DISTINCT status FROM orders;        -- unique values
```

**Filtering with WHERE**
```sql
SELECT * FROM users WHERE age >= 18;
SELECT * FROM users WHERE age BETWEEN 18 AND 30;
SELECT * FROM users WHERE name IN ('Aryan', 'Riya');
SELECT * FROM users WHERE email LIKE '%@gmail.com';   -- % = any chars, _ = one char
SELECT * FROM users WHERE name ILIKE 'ar%';           -- case-insensitive (Postgres)
SELECT * FROM users WHERE age IS NULL;                -- NULL needs IS, not =
SELECT * FROM users WHERE age >= 18 AND is_active;
SELECT * FROM users WHERE NOT is_active OR age IS NULL;
```
Comparison: `=`, `<>` or `!=`, `<`, `>`, `<=`, `>=`.

**Sorting, limiting, paging**
```sql
SELECT * FROM users ORDER BY created_at DESC;
SELECT * FROM users ORDER BY age DESC, name ASC;
SELECT * FROM users ORDER BY id LIMIT 10;
SELECT * FROM users ORDER BY id LIMIT 10 OFFSET 20;   -- page 3
```
Large `OFFSET` gets slow; for big tables use **keyset pagination**: `WHERE id > :last_seen_id ORDER BY id LIMIT 10`.

**Logical execution order** (why you can't use a `SELECT` alias in `WHERE`):
```
FROM → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

### UPDATE
```sql
UPDATE users SET age = 22 WHERE email = 'aryan@example.com';
UPDATE users SET age = age + 1, is_active = TRUE WHERE id = 5
RETURNING *;
```
⚠️ **No `WHERE` = every row is updated.**

### DELETE
```sql
DELETE FROM users WHERE id = 5;
DELETE FROM orders WHERE created_at < now() - INTERVAL '1 year' RETURNING id;
```
⚠️ **No `WHERE` = every row is deleted.** Tip: run the `WHERE` as a `SELECT` first, or wrap in a transaction.

### Aggregation and GROUP BY
Aggregate functions: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `ARRAY_AGG`, `STRING_AGG`.
```sql
SELECT COUNT(*) FROM users;                       -- count rows
SELECT COUNT(age) FROM users;                     -- counts non-NULL ages only
SELECT AVG(total), MAX(total) FROM orders;

SELECT user_id, COUNT(*) AS order_count, SUM(total) AS spent
FROM orders
WHERE status = 'delivered'
GROUP BY user_id
HAVING SUM(total) > 1000        -- HAVING filters groups; WHERE filters rows
ORDER BY spent DESC;
```
Rule: every selected column must either be in `GROUP BY` or inside an aggregate.

### JOINs — combining tables
A JOIN matches rows from two tables using a condition (usually PK = FK).

Sample data:
```
users:  (1, Aryan) (2, Riya) (3, Sam)
orders: (101, user 1) (102, user 1) (103, user 2)
```
```sql
-- INNER JOIN: only rows with a match on both sides
SELECT u.name, o.id AS order_id, o.total
FROM users u
INNER JOIN orders o ON o.user_id = u.id;
-- Aryan(101), Aryan(102), Riya(103). Sam is missing (no orders).

-- LEFT JOIN: all rows from the left table, NULLs where no match
SELECT u.name, o.id
FROM users u
LEFT JOIN orders o ON o.user_id = u.id;
-- includes Sam with order id NULL

-- Find users with NO orders (anti-join)
SELECT u.* FROM users u
LEFT JOIN orders o ON o.user_id = u.id
WHERE o.id IS NULL;
```
| Join | Returns |
|---|---|
| `INNER JOIN` | only matching rows |
| `LEFT JOIN` | all left + matches (NULL if none) |
| `RIGHT JOIN` | all right + matches |
| `FULL OUTER JOIN` | all rows from both sides |
| `CROSS JOIN` | every combination (cartesian product) |
| Self join | table joined with itself (e.g. employee → manager) |

### Subqueries and CTEs
```sql
-- Subquery
SELECT * FROM users
WHERE id IN (SELECT user_id FROM orders WHERE total > 1000);

-- CTE (WITH): a named temporary result, more readable
WITH big_spenders AS (
    SELECT user_id, SUM(total) AS spent
    FROM orders GROUP BY user_id HAVING SUM(total) > 1000
)
SELECT u.name, b.spent
FROM big_spenders b JOIN users u ON u.id = b.user_id;
```
`WITH RECURSIVE` handles hierarchies (org charts, category trees).

### Set operations
`UNION` (combine, dedupe), `UNION ALL` (combine, keep dupes), `INTERSECT`, `EXCEPT`.

### Window functions
Compute across related rows **without collapsing them** (unlike `GROUP BY`).
```sql
SELECT user_id, total,
       ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at DESC) AS rn,
       SUM(total)   OVER (PARTITION BY user_id) AS user_total,
       RANK()       OVER (ORDER BY total DESC) AS rank_overall,
       LAG(total)   OVER (PARTITION BY user_id ORDER BY created_at) AS prev_total
FROM orders;
```
Classic use: "latest order per user" → filter `rn = 1` in an outer query/CTE.

### CASE and COALESCE
```sql
SELECT name,
  CASE WHEN age < 18 THEN 'minor'
       WHEN age < 60 THEN 'adult'
       ELSE 'senior' END AS group_name,
  COALESCE(phone, 'N/A') AS phone      -- first non-NULL
FROM users;
```

### Useful built-in functions
```sql
now(), CURRENT_DATE, date_trunc('month', created_at),
EXTRACT(YEAR FROM created_at), AGE(created_at),
LENGTH(name), LOWER(name), UPPER(name), TRIM(name),
SUBSTRING(name, 1, 3), CONCAT(a,b) or a || b, REPLACE(s,'a','b'),
ROUND(x, 2), CEIL(x), FLOOR(x), ABS(x),
gen_random_uuid()
```

## Transactions

A transaction groups statements so they succeed or fail as one unit.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;        -- make permanent
-- or ROLLBACK; -- undo everything since BEGIN
```
- Errors inside a transaction abort it; you must `ROLLBACK`.
- `SAVEPOINT name` / `ROLLBACK TO name` allow partial rollback.
- Postgres uses **MVCC** (multi-version concurrency control): readers don't block writers and writers don't block readers; each transaction sees a consistent snapshot.
- **Isolation levels:** `READ COMMITTED` (default), `REPEATABLE READ`, `SERIALIZABLE`. Higher levels prevent more anomalies but may cause serialization failures you must retry.
- **Row locking:** `SELECT ... FOR UPDATE` locks selected rows until the transaction ends (prevents two users buying the last item). `FOR UPDATE SKIP LOCKED` is the standard job-queue pattern.

## Indexes

Without an index, Postgres does a **sequential scan** (reads every row). An index is a sorted structure (default **B-tree**) allowing fast lookup, like a book's index.

```sql
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE UNIQUE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_created ON orders(user_id, created_at DESC); -- composite
CREATE INDEX idx_active_users ON users(email) WHERE is_active;            -- partial
CREATE INDEX idx_users_lower_email ON users (LOWER(email));               -- expression
DROP INDEX idx_orders_user_id;
```
| Index type | Best for |
|---|---|
| **B-tree** (default) | `=`, `<`, `>`, `BETWEEN`, `ORDER BY` |
| **Hash** | equality only |
| **GIN** | `JSONB`, arrays, full-text search |
| **GiST** | geometric/range data, PostGIS |
| **BRIN** | huge, naturally ordered tables (time series) |

Key facts:
- PK and `UNIQUE` automatically create an index. **Foreign keys do NOT** — index them yourself, or JOINs/cascades get slow.
- Composite index `(a, b)` helps queries filtering on `a` or `a AND b`, **not** `b` alone (leftmost-prefix rule).
- Indexes speed reads but slow writes and use disk. Don't index everything.
- Low-selectivity columns (e.g. boolean) rarely benefit.

### EXPLAIN — understanding performance
```sql
EXPLAIN SELECT * FROM orders WHERE user_id = 5;          -- plan only
EXPLAIN ANALYZE SELECT * FROM orders WHERE user_id = 5;  -- actually runs it, shows real timings
```
Look for `Seq Scan` on large tables (maybe missing index) vs `Index Scan` / `Index Only Scan`. `ANALYZE` on `DELETE/UPDATE` really executes — wrap in a transaction and roll back.

## Design: normalization and relationships

**Normalization** = organizing tables to avoid duplicated data.
- **1NF:** each column holds one atomic value (no comma-separated lists in a cell); rows unique.
- **2NF:** every non-key column depends on the *whole* primary key (matters for composite keys).
- **3NF:** non-key columns depend only on the key, not on other non-key columns.

**Relationship types**
| Type | How to model | Example |
|---|---|---|
| One-to-many | FK on the "many" side | user → orders |
| One-to-one | FK + `UNIQUE` | user → profile |
| Many-to-many | **junction table** with two FKs | students ↔ courses |

```sql
CREATE TABLE enrollments (
    student_id BIGINT REFERENCES students(id) ON DELETE CASCADE,
    course_id  BIGINT REFERENCES courses(id)  ON DELETE CASCADE,
    PRIMARY KEY (student_id, course_id)       -- composite PK also blocks duplicates
);
```

```mermaid
erDiagram
    USERS ||--o{ ORDERS : places
    ORDERS ||--|{ ORDER_ITEMS : contains
    PRODUCTS ||--o{ ORDER_ITEMS : "appears in"
    USERS {
        bigint id PK
        text name
        text email UK
    }
    ORDERS {
        bigint id PK
        bigint user_id FK
        numeric total
        timestamptz created_at
    }
    ORDER_ITEMS {
        bigint order_id FK
        bigint product_id FK
        int quantity
    }
    PRODUCTS {
        bigint id PK
        text name
        numeric price
    }
```

**Denormalization** (deliberately duplicating data for read speed) is a valid tradeoff, but do it consciously.

## Powerful Postgres-specific features

### JSONB (document-style data inside a relational DB)
```sql
CREATE TABLE events (id BIGSERIAL PRIMARY KEY, payload JSONB NOT NULL);
INSERT INTO events (payload) VALUES ('{"type":"click","meta":{"page":"/home"}}');

SELECT payload->'meta'->>'page' FROM events;       -- -> returns JSON, ->> returns text
SELECT * FROM events WHERE payload @> '{"type":"click"}';   -- containment
CREATE INDEX idx_events_payload ON events USING GIN (payload);
```
Use JSONB for genuinely flexible parts; keep core, queried, relational fields as real columns.

### Arrays, enums
```sql
tags TEXT[]   →  WHERE 'sql' = ANY(tags)   or   tags @> ARRAY['sql']
CREATE TYPE order_status AS ENUM ('pending','paid','shipped','delivered');
```

### Full-text search
```sql
SELECT * FROM articles
WHERE to_tsvector('english', title || ' ' || body) @@ plainto_tsquery('english', 'postgres index');
-- index it with GIN on the tsvector expression or a generated column
```

### Views and materialized views
```sql
CREATE VIEW active_users AS SELECT * FROM users WHERE is_active;      -- saved query
CREATE MATERIALIZED VIEW daily_sales AS SELECT ... ;                  -- stored result
REFRESH MATERIALIZED VIEW daily_sales;                                -- must refresh manually
```

### Functions, triggers
```sql
-- auto-update an updated_at column
CREATE FUNCTION set_updated_at() RETURNS trigger AS $$
BEGIN
  NEW.updated_at = now();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_users_updated
BEFORE UPDATE ON users
FOR EACH ROW EXECUTE FUNCTION set_updated_at();
```

### Extensions
```sql
CREATE EXTENSION IF NOT EXISTS pgcrypto;   -- crypto functions
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS pg_trgm;    -- fuzzy / LIKE-index search
-- others: PostGIS (geo), pgvector (AI embeddings)
```

## Users, roles and permissions

```sql
CREATE ROLE app_user WITH LOGIN PASSWORD 'strong-password';
GRANT CONNECT ON DATABASE shop TO app_user;
GRANT USAGE ON SCHEMA public TO app_user;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_user;
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO app_user;
REVOKE DELETE ON users FROM app_user;
```
Principle of least privilege: your app should connect as a limited role, **never** as the `postgres` superuser.

## Using PostgreSQL with Node.js / Express

Two common approaches:
1. **`pg` (node-postgres)** — write raw SQL; full control. Best for learning.
2. **ORMs / query builders** — Prisma, Drizzle, Sequelize, Knex — generate SQL from JS/TS code and manage migrations.

### Setup with `pg`
```bash
npm install express pg dotenv
```
`.env`
```
DATABASE_URL=postgresql://app_user:secret@localhost:5432/shop
```

`db.js` — one **connection pool** for the whole app:
```js
require('dotenv').config();
const { Pool } = require('pg');

const pool = new Pool({ connectionString: process.env.DATABASE_URL });

module.exports = { query: (text, params) => pool.query(text, params), pool };
```
A **pool** keeps several open connections and reuses them. Opening a new connection per request is slow and exhausts Postgres.

### CRUD API
```js
const express = require('express');
const db = require('./db');
const app = express();
app.use(express.json());

// CREATE
app.post('/users', async (req, res) => {
  try {
    const { name, email, age } = req.body;
    const { rows } = await db.query(
      'INSERT INTO users (name, email, age) VALUES ($1, $2, $3) RETURNING *',
      [name, email, age]
    );
    res.status(201).json(rows[0]);
  } catch (err) {
    if (err.code === '23505') return res.status(409).json({ error: 'Email already exists' });
    console.error(err);
    res.status(500).json({ error: 'Server error' });
  }
});

// READ all (with simple pagination)
app.get('/users', async (req, res) => {
  const limit = Math.min(Number(req.query.limit) || 10, 100);
  const offset = Number(req.query.offset) || 0;
  const { rows } = await db.query(
    'SELECT id, name, email FROM users ORDER BY id LIMIT $1 OFFSET $2',
    [limit, offset]
  );
  res.json(rows);
});

// READ one
app.get('/users/:id', async (req, res) => {
  const { rows } = await db.query('SELECT * FROM users WHERE id = $1', [req.params.id]);
  if (rows.length === 0) return res.status(404).json({ error: 'Not found' });
  res.json(rows[0]);
});

// UPDATE
app.put('/users/:id', async (req, res) => {
  const { name, age } = req.body;
  const { rows } = await db.query(
    'UPDATE users SET name = $1, age = $2 WHERE id = $3 RETURNING *',
    [name, age, req.params.id]
  );
  if (rows.length === 0) return res.status(404).json({ error: 'Not found' });
  res.json(rows[0]);
});

// DELETE
app.delete('/users/:id', async (req, res) => {
  const { rowCount } = await db.query('DELETE FROM users WHERE id = $1', [req.params.id]);
  if (rowCount === 0) return res.status(404).json({ error: 'Not found' });
  res.status(204).send();
});

app.listen(3000);
```

**Important points in the code**
- `$1, $2, $3` are **parameterized placeholders**. The values are sent separately from the SQL, which **prevents SQL injection**. **Never** build SQL with string concatenation or template literals using user input.
- `rows` is an array of result objects; `rowCount` is how many rows were affected.
- `RETURNING *` gives you the saved row without a second query.
- Postgres error codes: `23505` unique violation, `23503` foreign-key violation, `23502` not-null violation, `22P02` invalid input (e.g. text where an integer was expected).
- Table/column **names** can't be parameterized; if they're dynamic, whitelist them in code.

### Transactions in Node
You must use **one dedicated client** from the pool for the whole transaction (pool.query may use different connections):
```js
async function transfer(fromId, toId, amount) {
  const client = await pool.connect();
  try {
    await client.query('BEGIN');
    await client.query('UPDATE accounts SET balance = balance - $1 WHERE id = $2', [amount, fromId]);
    await client.query('UPDATE accounts SET balance = balance + $1 WHERE id = $2', [amount, toId]);
    await client.query('COMMIT');
  } catch (err) {
    await client.query('ROLLBACK');
    throw err;
  } finally {
    client.release();   // ALWAYS return the client to the pool
  }
}
```

### Request flow
```mermaid
flowchart LR
    A[Client] -->|HTTP request| B[Express route]
    B -->|validate input| C[Controller / service]
    C -->|parameterized SQL| D[pg Pool]
    D -->|TCP connection| E[(PostgreSQL)]
    E -->|rows| D --> C -->|JSON response| A
```

### Schema migrations
A **migration** is a versioned script that changes the schema (e.g. `001_create_users.sql`, `002_add_phone.sql`). Don't edit tables by hand in production. Tools: `node-pg-migrate`, Knex, Prisma Migrate, Drizzle Kit. Migrations make schema changes repeatable across dev/staging/prod.

### Prisma example (ORM approach)
```prisma
model User {
  id     BigInt  @id @default(autoincrement())
  name   String
  email  String  @unique
  orders Order[]
}
model Order {
  id     BigInt  @id @default(autoincrement())
  total  Decimal @db.Decimal(10,2)
  user   User    @relation(fields: [userId], references: [id])
  userId BigInt
}
```
```js
const user = await prisma.user.create({ data: { name: 'Aryan', email: 'a@x.com' } });
const withOrders = await prisma.user.findMany({ include: { orders: true } });
```
ORMs speed development and give type safety; raw SQL gives control for complex/performance-critical queries.

## Backup, restore, maintenance

```bash
pg_dump -U app_user -d shop -F c -f shop.dump        # backup (custom format)
pg_restore -U app_user -d shop_new shop.dump          # restore
pg_dump -U app_user -d shop > shop.sql                # plain SQL
psql -U app_user -d shop_new < shop.sql
pg_dumpall > all.sql                                  # all DBs + roles
```
- **VACUUM** reclaims space from dead rows left by MVCC (updates/deletes create new row versions). **autovacuum** does this automatically; don't disable it.
- **ANALYZE** refreshes statistics the query planner uses. Stale stats → bad query plans.
- **Replication** (streaming) provides read replicas/failover; **PITR** (point-in-time recovery) uses WAL archives.

## PostgreSQL vs MongoDB

| | PostgreSQL | [[MongoDB]] |
|---|---|---|
| Model | Tables, rows, fixed schema | Collections, flexible JSON documents |
| Relationships | Foreign keys + JOINs | Embedding / manual references |
| Transactions | Mature, multi-table ACID | Supported, less central |
| Schema changes | Migrations required | Flexible, schema-on-read |
| Query language | SQL | Query operators + aggregation pipeline |
| Best for | Structured, relational, consistency-critical data | Varied/nested data, rapid iteration |

Postgres with `JSONB` covers many "flexible data" needs, so it's a strong default for most apps.

## When to use
- Data has clear relationships (users, orders, products, payments)
- Correctness matters (money, bookings, inventory)
- You need complex queries, reporting, analytics
- You want one database that also handles JSON, search, geo, vectors
- A solid default for almost any new web backend

## When NOT to use
- Extremely high-volume, schema-less event/log firehose with trivial querying (a specialized store may fit better)
- Truly simple key-value caching (use Redis)
- Embedded/serverless-local single-file needs (SQLite is simpler)

## Common mistakes
- **String-building SQL** with user input → SQL injection. Always use `$1` parameters.
- Forgetting `WHERE` on `UPDATE`/`DELETE`.
- Using `= NULL` instead of `IS NULL`.
- Using `FLOAT` for money instead of `NUMERIC`.
- Not indexing foreign keys or frequently filtered columns.
- Over-indexing (slows writes) or `SELECT *` everywhere.
- Creating a new `Client`/connection per request instead of using a pool.
- Using `pool.query` for multi-statement transactions (must use one checked-out client).
- Forgetting `client.release()` → pool exhausted, app hangs.
- Connecting the app as the `postgres` superuser.
- Large `OFFSET` pagination on big tables.
- Storing comma-separated values in one column (violates 1NF).
- Not using `TIMESTAMPTZ` → timezone bugs.
- Editing production schema manually instead of through migrations.
- Assuming `bigint`/`numeric` come back as JS numbers: `pg` returns `BIGINT` and `NUMERIC` as **strings** to avoid precision loss — convert deliberately.

## Edge cases / important details
- `NULL` semantics: `NULL = NULL` is not true; `NULL` in `NOT IN (...)` can make the whole condition unknown. Use `IS NULL`, `IS DISTINCT FROM`, or `NOT EXISTS`.
- `COUNT(*)` counts rows; `COUNT(col)` skips NULLs.
- `UNIQUE` allows multiple NULLs.
- Identifiers with uppercase letters must be double-quoted forever; stick to `snake_case` lowercase.
- A failed statement inside a transaction aborts the whole transaction until `ROLLBACK`.
- Sequences/identity values are **not** rolled back — gaps in IDs are normal.
- `ON DELETE CASCADE` can silently delete large amounts of data; use deliberately.
- `ALTER TABLE` can take locks and block traffic on big tables; plan migrations carefully (e.g. add column nullable first, backfill, then add constraint).

## Quick revision cheat sheet
```
Create:  CREATE TABLE / INSERT ... RETURNING
Read:    SELECT cols FROM t WHERE ... ORDER BY ... LIMIT n
Update:  UPDATE t SET col = v WHERE ... RETURNING *
Delete:  DELETE FROM t WHERE ...
Join:    FROM a JOIN b ON b.a_id = a.id
Group:   GROUP BY col HAVING agg(...) > x
Safety:  parameters ($1), constraints, transactions
Speed:   indexes (B-tree/GIN), EXPLAIN ANALYZE, pool, pagination
Node:    Pool → pool.query(sql, params) → rows
```

## Related concepts
- [[MongoDB]] — document database alternative; compare data models
- [[Express]] — routes that run these queries
- [[REST APIs]] — the CRUD routes above form a REST API
- [[JSON]] — `JSONB` stores JSON inside Postgres
