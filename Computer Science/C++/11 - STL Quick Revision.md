# 11 - STL Quick Revision (LeetCode Cheat Sheet)

A one-page revision layer over [[05 - STL Containers]], [[06 - STL Algorithms and Iterators]] and [[07 - C++ Built-in Functions for DSA]]. Read those for explanations; use this note to **decide what to learn first** and to **recall fast** before solving.

## What to Learn First (priority tiers)

Don't try to memorize the whole STL before solving problems. Learn Tier 1, start practising, and add the rest when a problem needs them.

**Tier 1: essential**
- Containers: `vector`, `string`, `unordered_map`, `unordered_set`, `pair`, `stack`, `queue`, `priority_queue`
- Member functions: `push_back`, `pop_back`, `size`, `empty`, `front`, `back`
- Algorithms: `sort`, `reverse`, `max`, `min`, `max_element`, `min_element`, `find`, `count`

**Tier 2: very important**
- `lower_bound`, `upper_bound`, `binary_search`, `accumulate`, `swap`
- `erase`, `insert`, `set`, `map`, `deque`

**Tier 3: learn when needed**
- `iota`, `next_permutation`, `prev_permutation`, `count_if`, `all_of`, `any_of`, `none_of`, `remove`, `unique`

## The Mental Model: three questions

For any STL function, ask:

1. **Does it work on values?** `max(a, b)`, `min(a, b)`, `swap(a, b)`
2. **Does it work on a range?** `sort(v.begin(), v.end())`, `reverse`, `count(begin, end, x)`
3. **Does it return an iterator?** Then dereference it for the value, subtract `begin()` for the index.

```cpp
auto it = find(v.begin(), v.end(), x);
if (it != v.end()) {
    int value = *it;
    int index = it - v.begin();
}
```
`end()` points **one past** the last element, and "not found" means "returned `end()`".

## What Does It Return? (the table that prevents most bugs)

| Function | Returns | Typical use |
|---|---|---|
| `max(a, b)` / `min(a, b)` | the value | two values |
| `max_element` / `min_element` | **iterator** | `*max_element(v.begin(), v.end())` |
| `find` | iterator (`end()` if absent) | `it != v.end()` |
| `lower_bound(x)` | iterator to first `>= x` | `it - v.begin()` for an index |
| `upper_bound(x)` | iterator to first `> x` | count of `x` = `upper - lower` |
| `binary_search` | `bool` | membership in a sorted range |
| `count` / `count_if` | integer | occurrences / matches |
| `accumulate` | type of the 3rd argument | `accumulate(b, e, 0LL)` |
| `sort`, `reverse`, `swap`, `iota`, `fill` | `void` | modify in place |
| `unique`, `remove`, `remove_if` | iterator to new logical end | wrap in `erase` |
| `next_permutation` | `bool` (false after the last permutation) | `do { } while (next_permutation(...))` |
| `all_of`, `any_of`, `none_of` | `bool` | predicate checks |
| `stack::pop`, `queue::pop`, `priority_queue::pop` | **`void`** | read `top()`/`front()` first |
| `set::insert` / `map::insert` | `pair<iterator, bool>` | `.second` is "newly inserted" |
| `vector::erase` | iterator to the next element | use it when erasing in a loop |
| `map::operator[]` | reference; **inserts a default** if missing | use `count`/`find` to test |

## LeetCode One-Liners

```cpp
int maxi = *max_element(v.begin(), v.end());
int mini = *min_element(v.begin(), v.end());
sort(v.begin(), v.end());                                  // ascending
sort(v.begin(), v.end(), greater<int>());                  // descending
reverse(v.begin(), v.end());                               // also works on strings
long long sum = accumulate(v.begin(), v.end(), 0LL);
int cnt   = count(v.begin(), v.end(), x);
int evens = count_if(v.begin(), v.end(), [](int a) { return a % 2 == 0; });

if (binary_search(v.begin(), v.end(), x)) { }              // sorted input only
int firstGE = lower_bound(v.begin(), v.end(), x) - v.begin();
int firstGT = upper_bound(v.begin(), v.end(), x) - v.begin();

unordered_map<int,int> freq;  for (int a : v) freq[a]++;    // frequency table
unordered_set<int> seen;      if (seen.count(x)) { }        // existence check

stack<int> st;                // int top = st.top(); st.pop();   (two steps)
queue<int> q;                 // int f = q.front(); q.pop();
priority_queue<int, vector<int>, greater<int>> minHeap;       // min-heap

v.erase(unique(v.begin(), v.end()), v.end());               // after sorting: remove duplicates
iota(v.begin(), v.end(), 0);                                 // 0, 1, 2, ... n-1
do { /* use v */ } while (next_permutation(v.begin(), v.end()));   // start from sorted order
```

## Range-based `for`

```cpp
for (int x : v) { }              // x is a copy: modifying it does not change v
for (int& x : v) { x *= 2; }     // reference: modifies the element
for (const auto& s : strs) { }   // avoid copying large objects
for (auto& [k, val] : mp) { }    // structured binding over a map
```

## `set` vs `unordered_set`, `map` vs `unordered_map`

| Property | ordered (`set`/`map`) | unordered |
|---|---|---|
| Order | sorted by key | none |
| Operations | O(log n) | O(1) average, O(n) worst |
| Extras | `lower_bound`, `upper_bound`, min/max via `begin()`/`rbegin()` | none |
| Pick when | you need order or range queries | you need the fastest lookup/frequency table |

## Common Mistakes
- `int x = st.pop();` does not compile: `pop()` returns `void`.
- Forgetting the `*` in `*max_element(...)` and getting an iterator instead of a value.
- Using `lower_bound`/`binary_search` on an unsorted range (silently wrong).
- `accumulate(..., 0)` on large data: use `0LL`.
- `map[x]` to check existence: it inserts `x`.
- Expecting `remove`/`unique` to shrink a vector without `erase`.
- `for (int x : v)` when you meant to modify elements (needs `int&`).

## Related Concepts
[[05 - STL Containers]]
[[06 - STL Algorithms and Iterators]]
[[07 - C++ Built-in Functions for DSA]]
[[08 - DSA Patterns with STL]]
[[00 - C++ MOC]]
