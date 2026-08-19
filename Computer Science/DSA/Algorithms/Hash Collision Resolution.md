# Hash Collision Resolution (Chaining, Open Addressing, Probing, Double Hashing)

## What is it?
When two different keys hash to the same array index (a **collision**), the hash table needs a strategy to store both. [[Hashing]] covers *using* hash maps; this note covers what happens **inside** them when collisions occur — relevant for understanding worst-case behavior and for occasionally being asked to implement one from scratch.

## Why does it exist?
No hash function is perfectly collision-free for arbitrary input (pigeonhole principle: more possible keys than table slots). Collision resolution determines whether a hash table degrades gracefully (still fast) or badly (O(n) lookups) under heavy load.

---

## Separate Chaining
**Idea:** Each array slot holds a **list** (or tree) of all entries that hashed to that index. On collision, just append to the list.

```
index 3 → [("apple", 5)] → [("grape", 9)]   // both hashed to index 3
```

- Lookup: O(1) average, O(chain length) worst case.
- Simple to implement; handles arbitrarily many collisions (list just grows).
- Extra memory overhead for list pointers; poor cache locality (list nodes scattered in memory).
- **This is what C++'s `unordered_map` uses internally.**

## Open Addressing (the family)
**Idea:** All entries live directly in the array itself (no separate lists) — on collision, **probe** for the next open slot using a defined sequence.

### Linear Probing
Try `index+1, index+2, index+3, ...` until an empty slot is found.
- Excellent cache locality (checking adjacent memory).
- **Clustering problem:** consecutive collisions create long runs of filled slots ("primary clustering"), degrading performance as the table fills up.

### Quadratic Probing
Try `index+1², index+2², index+3², ...` (i.e. `index + i²` for i = 1, 2, 3...) instead of linear steps.
- Reduces primary clustering compared to linear probing.
- Can still fail to find an empty slot even when one exists, unless table size and probing are chosen carefully (e.g. table size is prime).

### Double Hashing
Use a **second hash function** to determine the probe step size: `index + i × hash2(key)` for i = 1, 2, 3...
- Best collision-spreading of the open-addressing family — different keys colliding at the same index will usually have different step sizes, avoiding clustering almost entirely.
- Requires a second, independent hash function — more computation per probe.

## Comparison

| | Separate Chaining | Linear Probing | Quadratic Probing | Double Hashing |
|---|---|---|---|---|
| Extra memory | Yes (list nodes) | No | No | No |
| Cache locality | Poor | Best | Good | Good |
| Clustering | None (n/a) | High | Moderate | Low |
| Deletion | Easy | Tricky (needs tombstones) | Tricky | Tricky |

**Deletion in open addressing is notably trickier:** simply clearing a slot can break the probe chain for later lookups (a search might stop at the now-empty slot, missing an entry that was placed further along). Open-addressing tables typically use a special "deleted" marker (tombstone) instead of truly clearing the slot.

## When to use which
- **Separate chaining:** default choice when simplicity and easy deletion matter more than raw cache performance (what most language standard libraries use).
- **Open addressing (linear/quadratic/double hashing):** memory-constrained or cache-performance-critical systems; double hashing specifically when collision-heavy workloads are expected.

## Common mistakes
- Forgetting that open addressing requires a **load factor** below 1 (table can never be more than 100% full) — chaining has no such hard limit.
- Not using tombstones for deletion in open addressing — breaks subsequent lookups silently.
- Assuming `unordered_map`'s worst-case O(n) can't happen — it can, under adversarial input designed to cause maximal collisions (rarely relevant outside of deliberately crafted test cases or security contexts).

## Related concepts
- [[Hashing]] — the practical patterns for *using* hash maps; this note covers the internal mechanics.
