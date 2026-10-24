# 05 - STL Containers

The Standard Template Library (STL) provides ready-made, well-optimized data structures. Picking the right one is one of the highest-leverage skills for DSA interviews.

## vector — dynamic array

Contiguous memory, O(1) random access, O(1) amortized `push_back`, O(n) insert/erase in the middle.

> [!tip] When to use
> default choice for a resizable array — the most common container in DSA.

```cpp
vector<int> v = {1, 2, 3};
v.push_back(4);              // O(1) amortized
v.pop_back();                 // O(1)
v.insert(v.begin()+1, 99);    // O(n)
v.erase(v.begin());           // O(n)
v.size(); v.empty(); v.front(); v.back();
v[i]; v.at(i);                 // at() is bounds-checked (throws)
vector<vector<int>> grid(n, vector<int>(m, 0)); // 2D vector
```

## pair / tuple

Groups 2 (`pair`) or 3+ (`tuple`) values together — e.g. storing (value, index) for sorting.

```cpp
pair<int,int> p = {1, 2};
p.first; p.second;
vector<pair<int,int>> vp;
sort(vp.begin(), vp.end());   // sorts by first, then second

tuple<int,int,int> t = {1,2,3};
get<0>(t);
```

## string

```cpp
string s = "hello";
s.length(); s.size();
s.substr(pos, len);
s.append("world"); s += "world";
s.find("lo");            // returns index or string::npos
s.push_back('c'); s.pop_back();
reverse(s.begin(), s.end());
sort(s.begin(), s.end());
s == s2;                   // direct comparison works
stringstream ss(s);        // for tokenizing
```

## stack — LIFO

O(1) push/pop/top.

> [!tip] When to use
> matching brackets, monotonic stack problems, undo operations, iterative DFS, expression evaluation.

```cpp
stack<int> st;
st.push(x); st.pop(); /* no return value */ st.top(); st.empty();
```

## queue — FIFO

O(1) push/pop/front.

> [!tip] When to use
> BFS, level-order traversal.

```cpp
queue<int> q;
q.push(x); q.pop(); q.front(); q.back(); q.empty();
```

## deque — double-ended queue

O(1) push/pop at **both** ends, O(1) random access.

> [!tip] When to use
> push/pop needed from both front and back (e.g. sliding window maximum).

```cpp
deque<int> dq;
dq.push_back(x); dq.push_front(x);
dq.pop_back(); dq.pop_front();
dq.front(); dq.back(); dq[i];
```

## priority_queue — heap (max-heap by default)

O(log n) push/pop, O(1) top.

> [!tip] When to use
> repeatedly need the max/min (kth largest, Dijkstra, merge k sorted lists, top-K problems).

```cpp
priority_queue<int> maxHeap;                                // max-heap
priority_queue<int, vector<int>, greater<int>> minHeap;      // min-heap
maxHeap.push(5); maxHeap.top(); maxHeap.pop();

// custom comparator for pairs (min-heap by second element)
auto cmp = [](pair<int,int>&a, pair<int,int>&b){ return a.second > b.second; };
priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq2(cmp);
```

## set — sorted, unique elements (Red-Black tree)

O(log n) insert/erase/find, always sorted.

> [!tip] When to use
> need unique elements + sorted order + range queries.

```cpp
set<int> s;
s.insert(x); s.erase(x);       // erase by value
s.count(x);                     // 0 or 1 — existence check
s.find(x);                      // iterator or s.end()
s.lower_bound(x);               // first element >= x
s.upper_bound(x);               // first element > x
*s.begin(); *s.rbegin();        // smallest, largest
```
- `multiset<T>`: same but allows duplicates. **Trap:** `ms.erase(x)` removes **all** copies of `x` — to remove just one, use `ms.erase(ms.find(x))`.
- `unordered_set<T>`: O(1) avg insert/find, no ordering — faster on average but O(n) worst case (hash collisions).

## map — sorted key-value pairs (Red-Black tree)

O(log n) insert/erase/find, keys always sorted.

> [!tip] When to use
> key-value storage + sorted keys + range queries.

```cpp
map<string,int> m;
m["key"] = 5;                        // insert or update
m.count("key");                       // 0 or 1
m.find("key");                        // iterator or m.end()
m.erase("key");
for (auto &[k, v] : m) { }            // iterates in sorted key order
```

> [!warning] Trap
> `m["missingKey"]` **auto-inserts** a default-value entry as a side effect — use `.count()` or `.find()` to check existence without inserting.

- `unordered_map<K,V>`: O(1) average insert/find/erase, no order — **default choice for frequency counting / hashing** unless order matters. Worst case O(n) (hash collision) vs `map`'s guaranteed O(log n) — for adversarial/competitive judges, `map` can be the safer pick.
- `multimap<K,V>`: sorted, allows duplicate keys.

```cpp
multimap<int,int> mm;
mm.insert({1, 100}); mm.insert({1, 200});     // both kept
auto range = mm.equal_range(1);                // all values for key 1
for (auto it = range.first; it != range.second; it++) cout << it->second;
```

## list — doubly linked list

O(1) insert/erase given an iterator, O(n) random access (no `[]`).

> [!tip] When to use
> rarely needed in interviews — `vector`/`deque` usually suffice. Use when frequent mid-list insertion/deletion outweighs random access needs.

```cpp
list<int> l;
l.push_back(x); l.push_front(x);
l.insert(it, x);      // O(1) given an iterator — vector can't do this
l.erase(it);
l.sort(); l.reverse(); // list has its own sort, not <algorithm>'s
```

## array — fixed-size (C++11)

Safer alternative to a raw C array — knows its own size, works with STL algorithms.

```cpp
array<int, 5> a = {1,2,3,4,5};
a.size(); a.fill(0); a[i]; a.at(i);
```

## bitset — fixed-size bit array

> [!tip] When to use
> bitmask DP, compact boolean flags, fast set operations on small fixed universes.

```cpp
bitset<8> b(5);        // 00000101
b.set(i); b.reset(i); b.flip(i);
b.count();              // number of set bits
b.any(); b.all(); b.none();
b[i];
```

## Extra Container Functions & Idioms

### vector extras
```cpp
v.reserve(n);                  // pre-allocate capacity, avoids repeated reallocation
v.resize(n, 0);                // change size; new slots get 0
v.assign(n, x);                // replace contents with n copies of x
v.clear(); v.capacity(); v.swap(other);
v.emplace_back(a, b);          // construct in place (pairs/structs): avoids a temporary copy
v.insert(v.end(), w.begin(), w.end());     // append a whole range
v.erase(v.begin() + l, v.begin() + r);     // erase the half-open range [l, r)
vector<int> sub(v.begin() + l, v.begin() + r);   // copy of [l, r)
```
> [!warning] Trap
> `vector<bool>` is a bit-packed special case: you cannot take a reference to an element and it is slower than a byte array. Use `vector<char>`, `vector<int>`, or `bitset` when you need plain flags.

### Iterator invalidation (why "erase inside a loop" crashes)

| Container | What invalidates iterators |
|---|---|
| `vector` | `push_back` that reallocates invalidates **all**; `insert`/`erase` invalidate everything at and after that point |
| `deque` | insert/erase in the middle invalidates all; push at either end invalidates iterators (not references) |
| `list`, `set`, `map` | only the erased element's iterator |
| `unordered_*` | a rehash (triggered by insert) invalidates all iterators |

Safe erase-while-iterating (works for `set`, `map`, `list`, `vector`):
```cpp
for (auto it = s.begin(); it != s.end(); ) {
    if (shouldRemove(*it)) it = s.erase(it);   // erase returns the next valid iterator
    else ++it;
}
```

### Custom comparators for ordered containers and heaps
```cpp
set<int, greater<int>> descSet;                   // sorted descending
map<int, int, greater<int>> descMap;

struct ByIdx {                                     // functor comparator
    bool operator()(const pair<int,int>& a, const pair<int,int>& b) const { return a.second < b.second; }
};
set<pair<int,int>, ByIdx> s2;

struct Task {
    int pri; string name;
    bool operator<(const Task& o) const { return pri < o.pri; }   // priority_queue<Task> = max-heap on pri
};
priority_queue<Task> pq;
```
> [!warning] Trap
> A comparator must be a **strict weak ordering**: `a < b`, never `a <= b`. Using `<=` is undefined behaviour (can crash `sort`/`set`). Also, `priority_queue`'s comparator is inverted relative to `sort`: `operator<` returning true means "lower priority", so the **largest** element ends up on top.

### Insert / emplace return values
```cpp
auto [it, inserted] = s.insert(x);       // set/map insert returns pair<iterator, bool>
mp.emplace(k, v);                         // construct in place
mp.try_emplace(k, v);                     // does nothing if k already exists
```

### Member `lower_bound` vs `std::lower_bound`
> [!warning] Trap
> On `set`/`map` use the **member** `s.lower_bound(x)` (O(log n)). Calling `std::lower_bound(s.begin(), s.end(), x)` compiles but walks the tree linearly, O(n), because those iterators are not random access.

### Rarely needed but good to know
- `unordered_multiset<T>`: hash set that allows duplicates. `forward_list<T>`: singly linked list (no `size()`, no `push_back`).

### More string functions
```cpp
s.rfind("ab");                  // last occurrence, or string::npos
s.find_first_of("aeiou");       // first index of any listed character
s.replace(pos, len, "xyz");     // replace len characters starting at pos
s.resize(n, 'x');  string t(5, 'a');   // "aaaaa"
s.substr(pos);                    // from pos to the end; substr is an O(len) copy
// C++20: s.starts_with("ab"), s.ends_with("ab")
```

## Which Container to Pick

| Need | Container |
|---|---|
| Dynamic array, random access | `vector` |
| Frequency count, fast lookup | `unordered_map` |
| Sorted key-value, need order | `map` |
| Unique sorted elements | `set` |
| Unique elements, order irrelevant | `unordered_set` |
| LIFO | `stack` |
| FIFO | `queue` |
| Push/pop both ends | `deque` |
| Repeatedly get min/max | `priority_queue` |
| Frequent middle insert/delete | `list` |
| Fixed-size bit manipulation | `bitset` |
| Duplicate keys, sorted | `multimap` / `multiset` |

## Complexity Cheat Sheet (Access / Search / Insert / Delete)

| Container | Access | Search | Insert | Delete |
|---|---|---|---|---|
| `vector` | O(1) | O(n) | O(1) amortized at end, O(n) middle | O(n) |
| `list` | O(n) | O(n) | O(1) with iterator | O(1) with iterator |
| `deque` | O(1) | O(n) | O(1) at ends | O(1) at ends |
| `set` / `map` | O(log n) | O(log n) | O(log n) | O(log n) |
| `unordered_set` / `unordered_map` | O(1) avg | O(1) avg | O(1) avg | O(1) avg |
| `stack` / `queue` | O(1) top/front | - | O(1) | O(1) |
| `priority_queue` | O(1) top | - | O(log n) | O(log n) |

## Common Mistakes

- Checking membership with `map[x]` instead of `map.count(x)`/`find(x)` — `operator[]` **inserts** a default entry as a side effect.
- `priority_queue` is a max-heap by default — forgetting `greater<int>` when a min-heap is needed.
- Forgetting custom structs need `operator<` (or a comparator) to go into `set`/`sort`/`priority_queue`.
- Using `unordered_map`/`unordered_set` when worst-case O(n) lookup is unacceptable (adversarial input / strict judges) — prefer `map`/`set`.

## Related Concepts

[[06 - STL Algorithms and Iterators]]
[[08 - DSA Patterns with STL]]
