# DSA A2Z Sheet MOC

Tracks vault coverage against Striver's A2Z DSA sheet. ✅ = note exists and is solid · 🟨 = partially covered / needs depth · ⬜ = not yet created.

## Step 1 — Basics
- ⬜ Patterns, Math for DSA (GCD/LCM, primes, sieve)
- 🟨 C++ STL basics → see [[C++ Built-in Functions for DSA]]

## Step 2 — Sorting
- ✅ [[Sorting Techniques]]

## Step 3 — Arrays
- ✅ [[Arrays]]

## Step 4 — Binary Search
- ✅ [[Binary Search]]

## Step 5 — Strings
- ✅ [[Strings]]

## Step 6 — Linked List
- ✅ [[Linked List]]

## Step 7 — Recursion & Backtracking
- ✅ [[Recursion & Backtracking]]

## Step 8 — Bit Manipulation
- ⬜ Bit Manipulation (AND/OR/XOR tricks, bitmasking, subsets via bitmask)

## Step 9 — Stack and Queues
- ⬜ Stack (monotonic stack, next greater/smaller element)
- ⬜ Queues (circular queue, deque, monotonic deque)

## Step 10 — Sliding Window & Two Pointer
- ⬜ Sliding Window & Two Pointer (fixed/variable window, two-pointer on sorted arrays)
  - *Note: Pattern 6 in [[Hashing]] covers sliding window + hash map specifically — this note should cover the general technique beyond hashing use cases.*

## Step 11 — Heaps
- ⬜ Heaps / Priority Queue (min-heap, max-heap, k-th largest, merge k sorted lists)

## Step 12 — Greedy Algorithms
- 🟨 Greedy Algorithms — taught in conversation, not yet saved to vault

## Step 13 — Binary Trees
- ⬜ Binary Trees (traversals, views, diameter, LCA, construction from traversals)

## Step 14 — Binary Search Trees
- ⬜ Binary Search Trees (BST-specific operations, validate BST, floor/ceil)

## Step 15 — Graphs
- ⬜ Graph Basics & Representation
- ⬜ BFS / DFS
- ⬜ Topological Sort
- ⬜ Shortest Path Algorithms (Dijkstra, Bellman-Ford, Floyd-Warshall)
- ⬜ Minimum Spanning Tree (Prim's, Kruskal's, Union-Find/DSU)

## Step 16 — Dynamic Programming
- ✅ [[Dynamic Programming]] (concept + memoization/tabulation fundamentals)
- ⬜ DP on Subsequences (LCS, LIS family)
- ⬜ DP on Strings (edit distance, wildcard matching)
- ⬜ DP on Stocks (buy/sell variants)
- ⬜ DP on Trees / DP on Grids
- ✅ [[Kadane's Algorithm]] (1-D DP special case)

## Step 17 — Tries
- ⬜ Tries (insert/search/prefix, word break, XOR tries)

## Step 18 — Advanced String Algorithms
- ⬜ KMP / Z-Function (pattern matching)

## Cross-cutting (not strictly A2Z but interview-relevant)
- ✅ [[Hashing]] (all patterns)

---



## Named-Algorithm Coverage (cross-referenced against your algorithm list)

Most named algorithms you listed already live as **patterns inside topic notes** (e.g. Kadane's, Two Pointer, Sliding Window in [[Arrays]]; KMP, Rabin-Karp, Trie in [[Strings]]; N-Queens, Subsets in [[Recursion & Backtracking|Backtracking]]). The following got **dedicated notes** in `Algorithms/` since they weren't covered anywhere:

- ✅ [[Sieve of Eratosthenes]]
- ✅ [[Difference Array]]
- ✅ [[Maximum Product Subarray]]
- ✅ [[Euclidean Algorithm (GCD)]]
- ✅ [[Rat in a Maze]]
- ✅ [[Merge K Sorted Lists]]
- ✅ [[Binary Search Variants]] (Ternary, Exponential, Interpolation, Fibonacci Search)
- ✅ [[Hash Collision Resolution]] (Separate Chaining, Linear/Quadratic Probing, Double Hashing)
- ✅ [[Manacher's Algorithm]]

### Still queued (lower interview-frequency — add on request)
- ⬜ Boyer-Moore String Search (distinct from Boyer-Moore Majority Vote / Moore's Voting, which is already covered)
- ⬜ Aho-Corasick Algorithm (multi-pattern matching)
- ⬜ Suffix Array / Suffix Tree
- ⬜ Shell Sort, Tim Sort, Cycle Sort, Comb Sort
- ⬜ Branch and Bound
- ⬜ Backtracking with Bitmasking (state-compression technique)
- ⬜ Reservoir Sampling
- ⬜ Hoare Partition (Lomuto is implicitly used in [[Sorting Techniques]]'s Quick Sort code but not named — could add a short comparison note)
- ⬜ Brent's Cycle Detection (improvement over Floyd's Tortoise and Hare — already covered in [[Linked List]] Pattern 4)

## Suggested next order
1. **Greedy Algorithms** (already taught once — quick to formalize into a note)
2. **Trees** (per your stated queue — Binary Trees then BST)
3. **Stack and Queues**
4. **Sliding Window & Two Pointer**
5. **Bit Manipulation**
6. **Heaps**
7. **Graphs** (biggest step — will likely need its own folder like Algorithms/)
8. **DP subtypes** (subsequences, strings, stocks — expand off the existing DP note)
9. **Tries**
10. **Advanced Strings (KMP/Z-function)**

Update this tracker's checkboxes as each topic is covered — same pattern as [[System Design MOC]].
