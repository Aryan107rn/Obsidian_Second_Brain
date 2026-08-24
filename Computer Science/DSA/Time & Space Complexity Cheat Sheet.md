---
tags: [dsa, complexity, revision, cheat-sheet]
---

# Time & Space Complexity Cheat Sheet

A single-page summary of every pattern's complexity across the vault — for fast pre-interview scanning. Full explanations, code, and diagrams live in the linked topic/algorithm notes; this note is intentionally just the numbers.

**Reading the notation:** `n` = input size (array/string length) unless noted. `m` = second input's size, or matrix rows. `k` = range/distinct-value count. `V, E` = graph vertices/edges.

---

## [[Arrays]]
| Pattern | Time | Space |
|---|---|---|
| Two Pointer | O(n) | O(1) |
| Sliding Window | O(n) | O(1) |
| Kadane's Algorithm | O(n) | O(1) |
| Prefix Sum | Build O(n), Query O(1) | O(n) |
| Dutch National Flag | O(n) | O(1) |
| Moore's Voting | O(n) | O(1) |
| Quickselect (Kth largest) | O(n) avg, O(n²) worst | O(1) |
| Next Permutation | O(n) | O(1) |
| Floyd's Cycle (Find Duplicate) | O(n) | O(1) |
| Merge Overlapping Intervals | O(n log n) | O(n) |
| Rotate Array (3-reversal) | O(n) | O(1) |
| Set Matrix Zeroes | O(m·n) | O(1) |

## [[Strings]]
| Pattern | Time | Space |
|---|---|---|
| Two Pointer | O(n) | O(1) |
| Sliding Window | O(n) | O(min(n, charset)) |
| Frequency Counting (Anagram) | O(n) | O(1) — fixed 26-alphabet |
| KMP | O(n+m) | O(m) |
| Z-Algorithm | O(n+m) | O(n+m) |
| Rabin-Karp | O(n+m) avg, O(n·m) worst | O(1) |
| Trie (insert/search) | O(L) per op | O(total characters stored) |
| LCS / Longest Common Substring | O(n·m) | O(n·m) |
| String Compression (RLE) | O(n) | O(n) |
| Anagram Grouping | O(n·k log k) | O(n·k) |

## [[Linked List]]
| Pattern | Time | Space |
|---|---|---|
| Traversal / Insert / Delete | O(n) traverse, O(1) head ops | O(1) |
| Reverse (iterative / recursive) | O(n) | O(1) / O(n) |
| Slow-Fast Pointers (find middle) | O(n) | O(1) |
| Floyd's Cycle Detection | O(n) | O(1) |
| Merge Two Sorted Lists | O(n+m) | O(1) |
| Merge Sort on Linked List | O(n log n) | O(log n) |
| Remove Nth From End | O(n) | O(1) |
| Palindrome Check | O(n) | O(1) |
| Reverse in Groups of K | O(n) | O(n/k) |
| Intersection Point | O(m+n) | O(1) |
| Add Two Numbers | O(max(m,n)) | O(max(m,n)) |
| Clone with Random Pointer | O(n) | O(1) extra |
| Flatten Multilevel List | O(n) | O(d) — d = nesting depth |
| LRU Cache (DLL + hashmap) | O(1) get/put | O(capacity) |
| Rotate List by K | O(n) | O(1) |

## [[Binary Search]]
| Pattern | Time | Space |
|---|---|---|
| Standard Binary Search | O(log n) | O(1) |
| Lower / Upper Bound | O(log n) | O(1) |
| Search in Rotated Sorted Array | O(log n) | O(1) |
| Binary Search on Answer | O(log(range) × feasibility check) | O(1) |
| Binary Search on 2D Matrix | O(log(m·n)) | O(1) |
| Finding Peak Element | O(log n) | O(1) |

## [[Sorting Techniques]]
| Algorithm | Best | Avg | Worst | Space | Stable |
|---|---|---|---|---|---|
| Selection Sort | n² | n² | n² | O(1) | No |
| Bubble Sort | n | n² | n² | O(1) | Yes |
| Insertion Sort | n | n² | n² | O(1) | Yes |
| Merge Sort | n log n | n log n | n log n | O(n) | Yes |
| Quick Sort | n log n | n log n | n² | O(log n) | No |
| Heap Sort | n log n | n log n | n log n | O(1) | No |
| Counting Sort | n+k | n+k | n+k | O(k) | Yes* |
| Radix Sort | d(n+k) | d(n+k) | d(n+k) | O(n+k) | Yes |
| Bucket Sort | n+k | n+k | n² | O(n+k) | Depends |

## [[Hashing]]
| Pattern | Time | Space |
|---|---|---|
| Frequency Counting | O(n) | O(n) |
| Existence Check (hash set) | O(n) | O(n) |
| Complement / Two Sum | O(n) | O(n) |
| Prefix Sum + Hash Map | O(n) | O(n) |
| Grouping by Computed Key | O(n·k log k) | O(n·k) |
| Sliding Window + Hash Map | O(n) | O(k) — k = charset/window distinct values |
| Hashing in Graphs (visited/clone) | O(V+E) | O(V) |
| Custom Hashing | O(1) avg per op | O(n) |

## [[Recursion & Backtracking]]
| Pattern | Time | Space |
|---|---|---|
| Factorial / basic recursion | O(n) | O(n) — call stack |
| Divide & Conquer (Pow(x,n)) | O(log n) | O(log n) |
| Memoized Fibonacci | O(n) | O(n) |
| Subsets I / II | O(2ⁿ · n) | O(n) |
| Combination Sum (reuse allowed) | O(2^target) | O(target) |
| Combination Sum II (no reuse) | O(2ⁿ) | O(n) |
| Permutations | O(n! · n) | O(n) |
| Generate Parentheses | O(4ⁿ / √n) | O(n) |
| Letter Combinations of Phone Number | O(4ⁿ · n) | O(n) |
| Palindrome Partitioning | O(2ⁿ · n) | O(n) |
| Word Search (grid) | O(m·n·4ᴸ) | O(L) — L = word length |
| Word Break (memoized) | O(n²) | O(n) |
| N-Queens | ~O(n!), pruned | O(n²) |
| M-Coloring | O(mᵛ), pruned | O(V) |
| Sudoku Solver | O(9^81), heavily pruned | O(81) |
| Expression Add Operators | O(4ⁿ) | O(n) |

## [[Matrix]]
| Pattern | Time | Space |
|---|---|---|
| Traversal Basics | O(m·n) | O(1) |
| Spiral Traversal | O(m·n) | O(1) extra |
| Rotate In-Place (transpose+reverse) | O(n²) | O(1) |
| Set Matrix Zeroes | O(m·n) | O(1) |
| Binary Search on Matrix | O(log(m·n)) | O(1) |
| Staircase Search | O(m+n) | O(1) |
| Flood Fill / Number of Islands | O(m·n) | O(m·n) worst-case stack |
| BFS Shortest Path | O(m·n) | O(m·n) |
| DP on Grid | O(m·n) | O(m·n), optimizable to O(n) |
| Diagonal Traversal | O(m·n) | O(m·n) |

## Algorithm/ (standalone named algorithms)
| Algorithm | Time | Space |
|---|---|---|
| [[Kadane's Algorithm]] | O(n) | O(1) |
| [[Dynamic Programming]] (general) | Varies — O(states × transition cost) | O(states), optimizable |
| [[Sieve of Eratosthenes]] | O(N log log N) | O(N) |
| [[Difference Array]] | O(q+n) for q range-updates | O(n) |
| [[Maximum Product Subarray]] | O(n) | O(1) |
| [[Euclidean Algorithm (GCD)]] | O(log(min(a,b))) | O(log(min(a,b))) recursive, O(1) iterative |
| [[Rat in a Maze]] | O(4^(n²)), pruned | O(n²) |
| [[Merge K Sorted Lists]] (min-heap) | O(n log k) | O(k) |
| [[Binary Search Variants]] — Ternary | O(log₃/₂ n) | O(1) |
| [[Binary Search Variants]] — Exponential | O(log p) — p = target position | O(1) |
| [[Binary Search Variants]] — Interpolation | O(log log n) avg, O(n) worst | O(1) |
| [[Binary Search Variants]] — Fibonacci | O(log n) | O(1) |
| [[Hash Collision Resolution]] | O(1) avg, O(n) worst (long chain/probe sequence) | O(n) |
| [[Manacher's Algorithm]] | O(n) | O(n) |

---

## How to read this for interview prep
- If you can only remember one thing: **O(n) single-pass beats O(n²) nested loops** — most "optimize this" interview questions are asking you to find the hashing/two-pointer/sliding-window trick that drops a nested loop.
- **O(log n)** almost always signals binary search (or a divide-and-conquer halving trick) — see it in the target complexity? Ask "what am I halving each step?"
- **O(2ⁿ) / O(n!)** is the signature of brute-force backtracking/recursion without pruning or memoization — if you see this as your *first* solution, ask whether overlapping subproblems exist (→ DP) or heavy pruning is possible (→ better backtracking).
- Worst-case ≠ average-case: Quicksort and hashing both look great on paper (O(n log n), O(1)) but have real worst cases (O(n²), O(n)) — know when an interviewer might push on this (adversarial/sorted input, hash collisions).

## Related concepts
- [[DSA A2Z Sheet MOC]] — topic coverage tracker
- [[Important Questions to Revise]] — problem-level revision tracker (now individual notes in `imp question/`)
