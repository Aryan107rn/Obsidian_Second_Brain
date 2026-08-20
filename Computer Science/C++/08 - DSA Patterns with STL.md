# 08 - DSA Patterns with STL

A map from problem pattern → the STL tool that solves it efficiently, with reference implementations for the ones worth memorizing.

## Pattern → Tool Mapping
| Pattern | Tool |
|---|---|
| Two pointers / sliding window | `vector`, indices |
| Frequency counting | `unordered_map<int,int>` |
| Top-K problems | `priority_queue` |
| Graph BFS | `queue` + adjacency list + `vector<bool> visited` |
| Graph DFS | recursion / `stack` |
| Dijkstra | `priority_queue<pair<int,int>, vector<...>, greater<...>>` |
| Subsets / bitmask DP | `bitset`, bit tricks ([[07 - C++ Built-in Functions for DSA]]) |
| LRU Cache | `unordered_map` + `list` |
| Interval problems | `sort` + custom comparator on `vector<pair<int,int>>` |
| Union-Find (DSU) | plain arrays — write from scratch (below) |

## Frequency Counting
```cpp
unordered_map<int,int> freq;
for (int x : arr) freq[x]++;
```
**When to use:** counting occurrences, anagram checks, majority element.

## Custom Sort Comparator
```cpp
sort(v.begin(), v.end(), [](int a, int b) { return a > b; }); // descending
sort(v.begin(), v.end(), [](pair<int,int>& a, pair<int,int>& b) {
    return a.second < b.second;  // sort pairs by second element
});
```

## Monotonic Stack — Next Greater Element
```cpp
vector<int> nextGreater(vector<int>& a) {
    int n = a.size();
    vector<int> res(n, -1);
    stack<int> st;                  // stores indices
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
**When to use:** "next greater/smaller element" problems — O(n) instead of the naive O(n²).

## Sliding Window Maximum (deque)
```cpp
vector<int> maxSlidingWindow(vector<int>& arr, int k) {
    deque<int> dq;   // stores indices, values decreasing front-to-back
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
**When to use:** sliding window max/min in O(n) — the deque keeps candidates in monotonic order so the front is always the current window's max.

## Union-Find (DSU)
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
**When to use:** connectivity queries, cycle detection in undirected graphs, Kruskal's MST.

## Fast Hashing with Pair Keys (graph/coordinate problems)
```cpp
unordered_map<int, vector<int>> adj;   // adjacency list
map<pair<int,int>, int> visited;        // pair has a built-in operator< — works directly in map
// unordered_map<pair<int,int>, int> needs a custom hash function — map is simpler here
```

## Common Mistakes
- Passing large containers by value instead of `const&` into pattern functions — silent O(n) copy per call.
- Using `unordered_map` for adjacency/visited keys made of `pair` without a custom hash (won't compile) — use `map` instead, or write a hash functor.
- Forgetting the sliding-window deque stores **indices**, not values — needed to detect when the front has slid out of the window.

## Related Concepts
[[05 - STL Containers]]
[[06 - STL Algorithms and Iterators]]
