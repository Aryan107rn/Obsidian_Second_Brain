# 10 - Data Structure & Graph Templates

Code you should be able to write from memory for DSA and placement rounds: structures the STL does **not** provide (DSU, Fenwick tree, segment tree) and the standard graph routines built on STL containers. Full pattern theory for graphs will live in the upcoming Graphs notes; this note is the C++ implementation reference.

## Graph Representation

```cpp
int n, m;                                       // nodes 0..n-1, m edges
vector<vector<int>> adj(n);                     // unweighted
for (int i = 0; i < m; i++) {
    int u, v; cin >> u >> v;
    adj[u].push_back(v);
    adj[v].push_back(u);                         // omit for a directed graph
}

vector<vector<pair<int,int>>> g(n);             // weighted: {neighbour, weight}
g[u].push_back({v, w});
```
| Representation | Space | Edge check | Best for |
|---|---|---|---|
| Adjacency list | O(V + E) | O(degree) | almost everything (sparse graphs) |
| Adjacency matrix | O(V²) | O(1) | dense graphs, Floyd-Warshall, V ≤ ~1000 |

## BFS (shortest path, unweighted)

```cpp
vector<int> bfs(int src, int n, vector<vector<int>>& adj) {
    vector<int> dist(n, -1);
    queue<int> q;
    dist[src] = 0; q.push(src);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : adj[u])
            if (dist[v] == -1) { dist[v] = dist[u] + 1; q.push(v); }   // mark when pushing
    }
    return dist;
}
```
- Time: O(V + E), Space: O(V). Same loop as [[Matrix]] Pattern 8, with a grid replaced by a graph.

## DFS and Connected Components

```cpp
void dfs(int u, vector<vector<int>>& adj, vector<bool>& vis) {
    vis[u] = true;
    for (int v : adj[u]) if (!vis[v]) dfs(v, adj, vis);
}
int components(int n, vector<vector<int>>& adj) {
    vector<bool> vis(n, false);
    int c = 0;
    for (int i = 0; i < n; i++) if (!vis[i]) { c++; dfs(i, adj, vis); }
    return c;
}
```
- Time: O(V + E). For very deep graphs (path of 10⁵ nodes) use an explicit stack to avoid stack overflow ([[09 - Competitive Programming Template & Math Utilities]]).

## Topological Sort (Kahn's algorithm, BFS)

```cpp
vector<int> topoSort(int n, vector<vector<int>>& adj) {      // directed graph
    vector<int> indeg(n, 0), order;
    for (int u = 0; u < n; u++) for (int v : adj[u]) indeg[v]++;
    queue<int> q;
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        order.push_back(u);
        for (int v : adj[u]) if (--indeg[v] == 0) q.push(v);
    }
    return order;                       // order.size() < n  ⇒ the graph has a cycle
}
```
- Time: O(V + E). Doubles as directed-cycle detection.

## Dijkstra (non-negative weights)

```cpp
vector<ll> dijkstra(int src, int n, vector<vector<pair<int,int>>>& g) {
    vector<ll> dist(n, LLONG_MAX);
    priority_queue<pair<ll,int>, vector<pair<ll,int>>, greater<>> pq;   // min-heap on distance
    dist[src] = 0; pq.push({0, src});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;                   // stale entry, skip
        for (auto [v, w] : g[u])
            if (dist[u] + w < dist[v]) { dist[v] = dist[u] + w; pq.push({dist[v], v}); }
    }
    return dist;
}
```
- Time: O((V + E) log V), Space: O(V + E).
- **Remember:** fails with negative edges (use Bellman-Ford). The `d > dist[u]` check is what stops outdated heap entries from being reprocessed.

## Floyd-Warshall (all pairs, small V)

```cpp
for (int k = 0; k < n; k++)
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            if (d[i][k] < INF && d[k][j] < INF)               // guard INF + INF overflow
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
```
- Time: O(V³), Space: O(V²). The loop order **k outermost** is essential.

## Union-Find / DSU (with size)

The rank version is in [[08 - DSA Patterns with STL]]; this one tracks component sizes and counts.

```cpp
struct DSU {
    vector<int> parent, sz;
    int comps;
    DSU(int n) : parent(n), sz(n, 1), comps(n) { iota(parent.begin(), parent.end(), 0); }
    int find(int x) { return parent[x] == x ? x : parent[x] = find(parent[x]); }   // path compression
    bool unite(int a, int b) {
        a = find(a); b = find(b);
        if (a == b) return false;                     // already connected: this edge forms a cycle
        if (sz[a] < sz[b]) swap(a, b);
        parent[b] = a; sz[a] += sz[b]; comps--;
        return true;
    }
};
```
- Time: almost O(1) per operation (inverse Ackermann), Space: O(n).

### Kruskal's MST using DSU
```cpp
long long kruskal(int n, vector<array<int,3>>& edges) {      // {w, u, v}
    sort(edges.begin(), edges.end());
    DSU d(n);
    long long cost = 0;
    for (auto& [w, u, v] : edges) if (d.unite(u, v)) cost += w;
    return cost;                                              // d.comps == 1 ⇒ spanning tree exists
}
```
- Time: O(E log E).

## Fenwick Tree / Binary Indexed Tree (prefix sums with updates)

```cpp
struct BIT {
    int n; vector<ll> t;
    BIT(int n) : n(n), t(n + 1, 0) {}                // 1-indexed
    void update(int i, ll delta) { for (; i <= n; i += i & -i) t[i] += delta; }
    ll query(int i) { ll s = 0; for (; i > 0; i -= i & -i) s += t[i]; return s; }   // sum of [1, i]
    ll range(int l, int r) { return query(r) - query(l - 1); }
};
```
- Time: O(log n) per update/query, Space: O(n). `i & -i` is the lowest set bit ([[Bit Manipulation]]). Use it instead of a prefix array when the array **changes**.

## Segment Tree (iterative, point update, range query)

```cpp
struct SegTree {
    int n; vector<ll> t;
    SegTree(vector<int>& a) : n(a.size()), t(2 * a.size()) {
        for (int i = 0; i < n; i++) t[n + i] = a[i];
        for (int i = n - 1; i > 0; i--) t[i] = t[2 * i] + t[2 * i + 1];
    }
    void update(int p, ll v) {                       // set a[p] = v
        for (t[p += n] = v; p > 1; p >>= 1) t[p >> 1] = t[p] + t[p ^ 1];
    }
    ll query(int l, int r) {                         // sum over the half-open range [l, r)
        ll res = 0;
        for (l += n, r += n; l < r; l >>= 1, r >>= 1) {
            if (l & 1) res += t[l++];
            if (r & 1) res += t[--r];
        }
        return res;
    }
};
```
- Time: O(log n) per operation, Space: O(2n).
- **Remember:** swap `+` for `min`/`max`/`gcd` (with the right identity element) to get other range queries. This simple form needs a commutative operation and handles point updates only; range updates need lazy propagation.

## Structure Chooser

| Need | Use |
|---|---|
| Prefix/range sum, array never changes | prefix sum ([[Arrays]] Pattern 4) |
| Prefix/range sum with point updates | Fenwick tree |
| Range min/max/gcd with updates | segment tree |
| Are two nodes connected? Merge groups | DSU |
| Shortest path, unit edges | BFS |
| Shortest path, non-negative weights | Dijkstra |
| Order tasks with dependencies | topological sort |
| All-pairs shortest path, V ≤ ~400 | Floyd-Warshall |
| Cheapest connected subgraph | Kruskal / Prim |

## Complexity Summary

| Routine | Time | Space |
|---|---|---|
| BFS / DFS | O(V + E) | O(V) |
| Kahn topological sort | O(V + E) | O(V) |
| Dijkstra | O((V + E) log V) | O(V + E) |
| Floyd-Warshall | O(V³) | O(V²) |
| DSU operation | ~O(1) amortized | O(n) |
| Kruskal | O(E log E) | O(V) |
| Fenwick tree | O(log n) per op | O(n) |
| Segment tree | O(log n) per op | O(n) |

## Common Mistakes
- Marking BFS nodes visited when popping instead of pushing (duplicate queue entries).
- Dijkstra with negative edges, or without the stale-entry check.
- Using `INT_MAX` as infinity in Floyd-Warshall, then overflowing on `INF + INF`.
- Fenwick tree with 0-based indices: `i & -i` loops forever at 0, so index from 1.
- Segment tree `query` treating `r` as inclusive when the loop assumes `[l, r)`.
- DSU `find` without path compression, giving O(n) worst-case chains.
- Forgetting directed graphs add only one adjacency entry per edge.

## Related Concepts
[[05 - STL Containers]]
[[08 - DSA Patterns with STL]]
[[09 - Competitive Programming Template & Math Utilities]]
[[Merge K Sorted Lists]]
[[Stack & Queues]]
[[DSA A2Z Sheet MOC]]
