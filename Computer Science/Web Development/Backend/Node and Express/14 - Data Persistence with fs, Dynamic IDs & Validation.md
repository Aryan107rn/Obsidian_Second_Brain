# 14 - Data Persistence with fs, Dynamic IDs & Validation

*(Following Piyush Garg's Node.js playlist.)*

## Context

Before introducing a real database, a common learning step is to persist data to a flat JSON file (e.g. `mock_data.json`) acting as a stand-in "database." Building a working `POST` endpoint around this teaches the same shape of logic (read → modify → write → respond) that a real database-backed endpoint will follow later.

## The flow, step by step

### 1. Load existing data

```js
const fs = require('fs');
let users = require('./mock_data.json'); // Node can require a .json file directly — it's parsed into a JS array/object automatically
```

### 2. Assign a new ID dynamically

```js
app.post('/users', (req, res) => {
    const newUser = {
        id: users.length + 1,   // simplistic ID assignment based on current array length
        ...req.body
    };
    // ...
});
```

**Why `length + 1` is a reasonable-but-flawed shortcut:** it's simple and works fine while entries are only ever added, never removed. **The problem:** once an entry is deleted from the middle of the array, `length` no longer reflects the highest ID ever used — a later insert can end up reusing an ID that still belongs to an existing record, creating duplicates. This is exactly why real databases use dedicated ID strategies instead (auto-incrementing primary keys managed by the database engine, or globally-unique UUIDs) rather than deriving an ID from the current collection size. Fine for a prototype; not something to carry into a real backend.

### 3. Update the in-memory array

```js
users.push(newUser);
```

### 4. Persist back to disk

```js
fs.writeFile('./mock_data.json', JSON.stringify(users, null, 2), (err) => {
    if (err) {
        return res.status(500).json({ error: "Failed to save data" });
    }
    res.status(201).json(newUser); // 201 Created — see REST APIs status codes
});
```

- **`JSON.stringify(users, null, 2)`** — the `null, 2` arguments pretty-print the JSON with 2-space indentation, keeping the file human-readable when you open it directly (versus a single unreadable line of minified JSON).
- **This overwrites the *entire* file** on every single write — acceptable for a small prototype file, but obviously doesn't scale to any real dataset size; a genuine reason real applications move to an actual database rather than a growing JSON file.
- **`fs.writeFile` is asynchronous** (see [[04 - File Handling in Node.js (fs module)]]) — the response is correctly sent only *inside* the callback, once the write has actually finished. Responding before that would risk telling the client "created" even if the write later fails.

## Validation before processing

Before doing any of the above, check that `req.body` actually contains what you expect:

```js
app.post('/users', (req, res) => {
    const { name, email } = req.body;
    if (!name || !email) {
        return res.status(400).json({ error: "name and email are required" }); // 400 Bad Request
    }
    // ... proceed with ID assignment, push, and write ...
});
```

**Why this matters:** without it, a missing or malformed field can either crash the server (e.g. calling a string method on `undefined`) or silently write a broken/incomplete entry into your "database" file — a bug that's much harder to trace later than rejecting the bad request immediately with a clear `400` (see [[REST APIs]]'s status code table).

## Homework: PATCH and DELETE (not yet implemented — same pattern)

The next exercise is applying this exact **read → modify → write** shape to two more verbs, using `req.params.id` (a route parameter, e.g. `/users/:id`) to identify which record to touch:

- **`PATCH /users/:id`** — find the matching object in the array (e.g. `users.find(u => u.id == req.params.id)`), merge the provided `req.body` fields onto it (e.g. `Object.assign(existingUser, req.body)`), then write the array back to disk exactly as in step 4 above.
- **`DELETE /users/:id`** — remove the matching entry (e.g. `users = users.filter(u => u.id != req.params.id)`), then write the updated array back to disk.

Both follow the identical persistence pattern already built for `POST` — only the in-memory array operation differs. Worth returning to this note to fill in the actual code once implemented.

## Common mistakes

- Responding to the client **before** `fs.writeFile`'s callback confirms success — risks a false "success" response if the write actually fails.
- Relying on `length`-based IDs in anything beyond a throwaway prototype — leads to duplicate IDs once deletions happen.
- Skipping validation and letting bad data reach the file-write step — far harder to debug than an early, explicit `400 Bad Request`.
- Forgetting that `require('./mock_data.json')` is cached by Node's module system (see [[03 - npm, package.json & Node Modules]]) — the in-memory `users` array reflects the file's content *at server startup*; if the file is edited externally while the server is running, that in-memory copy won't automatically pick up the change.

## Related concepts
[[04 - File Handling in Node.js (fs module)]]
[[12 - express.urlencoded, express.json & req.body]] — where `req.body` comes from
[[13 - API Testing with Postman]] — how you'll actually test this endpoint
[[REST APIs]] — status codes (`201`, `400`) and verb semantics for the PATCH/DELETE homework
