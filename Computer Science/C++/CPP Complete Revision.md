# C++ Complete Revision (STL + Core Concepts for DSA)

## 1. Data Type Ranges
Critical for avoiding overflow bugs in DSA — always check constraints against these.

| Type | Size | Range |
|---|---|---|
| `char` | 1 byte | -128 to 127 (signed) |
| `short` | 2 bytes | -32,768 to 32,767 |
| `int` | 4 bytes | -2,147,483,648 to 2,147,483,647 (~2.1 × 10⁹) |
| `unsigned int` | 4 bytes | 0 to 4,294,967,295 |
| `long` | 4/8 bytes (platform-dependent) | at least int range |
| `long long` | 8 bytes | -9.2×10¹⁸ to 9.2×10¹⁸ |
| `unsigned long long` | 8 bytes | 0 to 1.8×10¹⁹ |
| `float` | 4 bytes | ~7 decimal digits precision |
| `double` | 8 bytes | ~15-16 decimal digits precision |

**Remember:**
- `int` overflows around 2×10⁹ — if a problem has n up to 10⁵ and you're summing/multiplying, sums can exceed `int` range (e.g. 10⁵ × 10⁵ = 10¹⁰) → use `long long`.
- `INT_MAX` = 2147483647, `INT_MIN` = -2147483648 (`<climits>`) — common sentinel values.
- Mid-point formula `(low+high)/2` can overflow `int` if both are near `INT_MAX` — use `low + (high-low)/2`.

---

## 2. Essential Headers
```cpp
#include <bits/stdc++.h>   // competitive programming: includes almost everything
// Individually: <vector> <deque> <list> <set> <map> <unordered_set> <unordered_map>
// <stack> <queue> <algorithm> <numeric> <climits> <cmath> <string> <utility>
```

---

## 3. Pass by Value vs Reference vs Pointer
- **Pass by value**: copies the argument — safe but costly for large objects (vectors, strings).
- **Pass by reference (`&`)**: no copy, function can modify caller's variable. Use `const T&` when you don't want to modify but want to avoid copying.
- **Pass by pointer (`*`)**: similar to reference but allows null and explicit re-pointing.
- **Remember:** Always pass large containers (`vector`, `string`, `map`) by `const&` unless you intend to modify them in place — passing by value silently causes O(n) copy overhead on every call, a common cause of TLE.

---

## 4. Object-Oriented Concepts (quick DSA-relevant recap)
- **Class vs Struct**: identical except default access — `class` members are `private` by default, `struct` members `public`. DSA convention: use `struct` for simple data holders (e.g. `struct Node { int val; Node* next; };`).
- **Constructor**: special function to initialize objects; called automatically on creation.
- **Operator overloading**: needed when custom structs go into `set`/`priority_queue`/`sort` — must define `<` (or supply a comparator).
```cpp
struct Point {
    int x, y;
    bool operator<(const Point& other) const {   // required for set/sort/priority_queue
        return x < other.x;
    }
};
```
- **Templates**: generic functions/classes — STL containers are all templates (`vector<T>`).

---

## 5. STL Containers

### 5.1 `vector<T>` — dynamic array
- Contiguous memory, O(1) random access, O(1) amortized `push_back`, O(n) insert/erase at arbitrary position.
- **When to use:** default choice for a resizable array — most common container in DSA.
```cpp
vector<int> v = {1, 2, 3};
v.push_back(4);          // O(1) amortized
v.pop_back();             // O(1)
v.size();                 // O(1)
v.insert(v.begin()+1, 99); // O(n)
v.erase(v.begin());       // O(n)
sort(v.begin(), v.end()); // O(n log n)
v.begin(), v.end();       // iterators
vector<vector<int>> mat(m, vector<int>(n, 0)); // 2D vector
```

### 5.2 `deque<T>` — double-ended queue
- O(1) push/pop at BOTH ends, O(1) random access (not fully contiguous internally).
- **When to use:** need push/pop from both front and back (e.g. sliding window maximum).
```cpp
deque<int> dq;
dq.push_front(1); dq.push_back(2);
dq.pop_front(); dq.pop_back();
dq.front(); dq.back();
```

### 5.3 `list<T>` — doubly linked list
- O(1) insert/erase anywhere (given an iterator), O(n) random access (no `[]`).
- **When to use:** rarely needed in interviews — `vector`/`deque` usually suffice. Use when frequent mid-list insertion/deletion outweighs random access needs.

### 5.4 `pair<T1,T2>` and `tuple<...>`
- **When to use:** grouping 2 (pair) or 3+ (tuple) values together — e.g. storing (value, index) for sorting.
```cpp
pair<int,int> p = {1, 2};
p.first; p.second;
tuple<int,int,int> t = {1,2,3};
get<0>(t);
```

### 5.5 `stack<T>` (adapter, default backed by `deque`)
- LIFO. O(1) push/pop/top.
- **When to use:** matching brackets, monotonic stack problems, undo operations, DFS iterative, expression evaluation.
```cpp
stack<int> st;
st.push(1); st.pop(); st.top(); st.empty();
```

### 5.6 `queue<T>` (adapter, default backed by `deque`)
- FIFO. O(1) push/pop/front.
- **When to use:** BFS, level-order traversal.
```cpp
queue<int> q;
q.push(1); q.pop(); q.front(); q.back(); q.empty();
```

### 5.7 `priority_queue<T>` (max-heap by default)
- O(log n) push/pop, O(1) top.
- **When to use:** need max/min repeatedly (kth largest, Dijkstra, merge k sorted lists, top-k problems).
```cpp
priority_queue<int> maxHeap;                              // max-heap
priority_queue<int, vector<int>, greater<int>> minHeap;    // min-heap
maxHeap.push(5); maxHeap.top(); maxHeap.pop();
// Custom comparator for pairs (min-heap by second element):
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
```

### 5.8 `set<T>` — sorted, unique elements (Red-Black Tree)
- O(log n) insert/erase/find. Always sorted (in-order traversal = sorted order).
- **When to use:** need unique elements AND sorted order AND range queries (`lower_bound`/`upper_bound`).
```cpp
set<int> s;
s.insert(5); s.erase(5); s.find(5) != s.end(); // membership check
s.count(5);                 // 0 or 1
s.lower_bound(5);           // iterator to first element >= 5
s.upper_bound(5);           // iterator to first element > 5
*s.begin();                 // smallest
*s.rbegin();                // largest
```
- `multiset<T>`: same but allows duplicates. `unordered_set<T>`: O(1) avg insert/find but no order — use when order doesn't matter and you just need fast membership.

### 5.9 `map<K,V>` — sorted key-value pairs (Red-Black Tree)
- O(log n) insert/erase/find. Keys always sorted.
- **When to use:** need key-value storage AND sorted keys AND range queries.
```cpp
map<string,int> m;
m["apple"] = 5;              // insert or update
m.find("apple") != m.end();  // check existence (avoid m["x"] for checks — it inserts!)
m.erase("apple");
for (auto& [k, v] : m) { }   // structured bindings, iterates in sorted key order
```
- `unordered_map<K,V>`: O(1) average insert/find/erase, no order — **default choice for frequency counting / hashing** unless sorted order is needed.
- **Remember:** `unordered_map` worst case is O(n) (hash collisions) — `map` guarantees O(log n) always. For adversarial/competitive judges, `map` can be safer.

---

## 6. Iterators
- `begin()/end()`: forward, `rbegin()/rend()`: reverse.
- `auto it = v.begin(); *it` dereferences; `it++` advances.
- Range-based for loop `for (auto& x : v)` is the idiomatic modern equivalent for most cases.

---

## 7. Key `<algorithm>` / `<numeric>` Functions for DSA

| Function | Purpose | Complexity |
|---|---|---|
| `sort(begin, end)` | sort ascending | O(n log n) |
| `sort(begin, end, cmp)` | custom comparator | O(n log n) |
| `stable_sort(begin, end)` | preserves equal-element order | O(n log n) |
| `reverse(begin, end)` | reverse range | O(n) |
| `binary_search(begin, end, val)` | true/false if present (needs sorted) | O(log n) |
| `lower_bound(begin, end, val)` | iterator to first ≥ val | O(log n) |
| `upper_bound(begin, end, val)` | iterator to first > val | O(log n) |
| `accumulate(begin, end, init)` | sum (or custom fold) | O(n) |
| `min_element`/`max_element(begin, end)` | iterator to min/max | O(n) |
| `count(begin, end, val)` | count occurrences | O(n) |
| `unique(begin, end)` | removes consecutive duplicates (needs sorted first) | O(n) |
| `next_permutation(begin, end)` | generates next lexicographic permutation | O(n) |
| `__builtin_popcount(x)` | count set bits (int) | O(1)-ish |
| `__gcd(a, b)` | GCD | O(log min(a,b)) |
| `swap(a, b)` | swap two values | O(1) |
| `clamp(val, lo, hi)` | bound value in range | O(1) |
| `fill(begin, end, val)` | fill range with value | O(n) |

**Remember:** `lower_bound`/`upper_bound`/`binary_search` require the range to be sorted first — using them on unsorted data gives undefined/wrong results silently.

---

## 8. Common DSA Patterns with STL

### 8.1 Frequency counting
```cpp
unordered_map<int,int> freq;
for (int x : arr) freq[x]++;
```
- **When to use:** counting occurrences, anagram checks, majority checks.

### 8.2 Custom sort comparator
```cpp
sort(v.begin(), v.end(), [](int a, int b) { return a > b; }); // descending
sort(v.begin(), v.end(), [](pair<int,int>& a, pair<int,int>& b) {
    return a.second < b.second;  // sort pairs by second element
});
```

### 8.3 Monotonic stack (next greater element)
```cpp
vector<int> nextGreater(vector<int>& a) {
    int n = a.size();
    vector<int> res(n, -1);
    stack<int> st;   // stores indices
    for (int i = 0; i < n; i++) {
        while (!st.empty() && a[st.top()] < a[i]) {
            res[st.top()] = a[i];
            st.pop();
        }
        st.push(i);
    }
    return res;
}
```
- **When to use:** "next greater/smaller element" style problems — O(n) instead of naive O(n²).

### 8.4 Sliding window maximum with deque
```cpp
vector<int> maxSlidingWindow(vector<int>& arr, int k) {
    deque<int> dq;  // stores indices, values decreasing front-to-back
    vector<int> result;
    for (int i = 0; i < arr.size(); i++) {
        if (!dq.empty() && dq.front() <= i - k) dq.pop_front();
        while (!dq.empty() && arr[dq.back()] < arr[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) result.push_back(arr[dq.front()]);
    }
    return result;
}
```
- **When to use:** sliding window max/min in O(n) — deque keeps candidates in monotonic order.

### 8.5 Fast hashing with pairs (for graph/coordinate keys)
```cpp
unordered_map<int, vector<int>> adj; // adjacency list
map<pair<int,int>, int> visited;      // pair keys work directly in map (has operator<)
// unordered_map<pair<int,int>, int> needs a custom hash — map is simpler for this
```

---

## 9. Common mistakes / Interview points
- Using `int` for sums/products that can exceed 2×10⁹ — silent overflow, wrong answer, no crash.
- Checking membership with `map[x]` instead of `map.find(x) != map.end()` — `operator[]` **inserts** a default value as a side effect.
- Forgetting `lower_bound`/`upper_bound`/`binary_search` need sorted input.
- Passing large containers by value instead of `const&` — silent O(n) copy per call, causes TLE.
- Using `unordered_map` when worst-case O(n) lookup (hash collision / adversarial input) is unacceptable — prefer `map` in that case.
- Forgetting custom structs need `operator<` (or a comparator) to go into `set`/`sort`/`priority_queue`.
- `priority_queue` is max-heap by default — forgetting `greater<int>` for min-heap.
- `unique()` only removes **consecutive** duplicates — must `sort()` first for full dedup.

## 10. Study checklist
- [ ] Know when to use `vector` vs `deque` vs `list`
- [ ] Know when `set`/`map` (sorted, O(log n)) vs `unordered_set`/`unordered_map` (O(1) avg, no order)
- [ ] Comfortable writing custom comparators (lambdas) for `sort` and `priority_queue`
- [ ] Know `lower_bound`/`upper_bound` semantics cold
- [ ] Know int vs long long overflow thresholds for typical constraint sizes (n ≤ 10⁵, 10⁶, etc.)
- [ ] Comfortable with monotonic stack and deque-based sliding window patterns

## Related concepts
[[Arrays]]
[[Sorting Techniques]]
[[Binary Search]]

---

## 11. Container Complexity Cheat Sheet (Access / Search / Insert / Delete)

| Container | Access | Search | Insert | Delete |
|---|---|---|---|---|
| `vector` | O(1) | O(n) | O(1) amortized at end, O(n) middle | O(n) |
| `list` | O(n) | O(n) | O(1) with iterator | O(1) with iterator |
| `deque` | O(1) | O(n) | O(1) at ends | O(1) at ends |
| `set` / `map` | O(log n) | O(log n) | O(log n) | O(log n) |
| `unordered_set` / `unordered_map` | O(1) avg | O(1) avg | O(1) avg | O(1) avg |
| `stack` / `queue` | O(1) top/front | - | O(1) | O(1) |
| `priority_queue` | O(1) top | - | O(log n) | O(log n) |

## 12. Pattern → Tool Mapping
- Two pointers / sliding window → `vector`, indices
- Frequency counting → `unordered_map<int,int>`
- Top-K → `priority_queue`
- Graph BFS → `queue` + adjacency list + `vector<bool> visited`
- Dijkstra → `priority_queue<pair<int,int>, vector<...>, greater<...>>`
- Subsets / bitmask DP → `bitset`, bit tricks (see [[C++ Built-in Functions for DSA]])
- LRU Cache → `unordered_map` + `list`
- Intervals → `sort` + custom comparator on `vector<pair<int,int>>`
- Union-Find (DSU) → plain arrays, write from scratch:
```cpp
vector<int> parent, rank_;
int find(int x) { return parent[x]==x ? x : parent[x]=find(parent[x]); } // path compression
void unite(int a, int b) {
    a = find(a); b = find(b);
    if (a == b) return;
    if (rank_[a] < rank_[b]) swap(a, b);
    parent[b] = a;
    if (rank_[a] == rank_[b]) rank_[a]++;
}
```

## Related concepts
[[C++ Core Language Features]]
[[C++ Built-in Functions for DSA]]
