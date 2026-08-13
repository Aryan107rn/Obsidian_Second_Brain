# Dynamic Programming (DP)

## What is it?

Dynamic Programming is a technique for solving problems by breaking them into smaller subproblems, solving each subproblem **only once**, and reusing (storing) that result instead of recomputing it. It's not a separate algorithm — it's an optimization layered on top of recursion.

Think of it as **"smart recursion that remembers its own answers."**

## Why does it exist?

Plain recursion can be extremely wasteful. Some recursive solutions branch out and solve the *exact same smaller subproblem* many times over. DP eliminates this repeated work, turning exponential-time solutions into polynomial-time ones.

### The Fibonacci example

```
fib(5)
├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2)
│   │   │   ├── fib(1)
│   │   │   └── fib(0)
│   │   └── fib(1)
│   └── fib(2)          ← already computed above!
│       ├── fib(1)
│       └── fib(0)
└── fib(3)               ← already computed above!
    ├── fib(2)
    │   ├── fib(1)
    │   └── fib(0)
    └── fib(1)
```

`fib(2)` gets computed 3 times, `fib(3)` gets computed 2 times, and the blowup gets exponentially worse as `n` grows. Plain recursive Fibonacci is **O(2ⁿ)** time. If we cache the answer for each `n` the first time it's computed, every later call becomes an O(1) lookup, and total work drops to **O(n)**.

## When does DP apply? (the two required properties)

A problem is a DP candidate **only if both** hold:

1. **Optimal substructure** — the optimal solution to the full problem can be built from optimal solutions to its subproblems. (e.g. the shortest path A→C through B is the shortest path A→B plus the shortest path B→C.)
2. **Overlapping subproblems** — a naive recursive solution ends up solving the *same* subproblem multiple times, as seen in the Fibonacci tree above.

If a problem has optimal substructure but **no** overlapping subproblems, DP gives no benefit — that's plain divide-and-conquer (e.g. merge sort recursively splits, but the left half and right half are never the same subproblem, so there's nothing to cache).

## How does it work? Two implementation styles

### 1. Memoization (Top-Down)

Write the natural recursive solution as you'd normally think of it, but before doing the recursive work for an input, check a cache (map/array). If already solved, return the cached value; otherwise compute, store, and return.

```cpp
unordered_map<int, long long> memo;

long long fib(int n) {
    if (n <= 1) return n;                     // base case
    if (memo.count(n)) return memo[n];        // already solved? reuse it
    return memo[n] = fib(n - 1) + fib(n - 2);  // solve once, store, return
}
```

- Feels natural — matches how you'd think about the recursive definition.
- Only computes subproblems actually needed (lazy).
- Has recursion call-stack overhead; risks stack overflow for very deep recursion (e.g. n = 10^5).

### 2. Tabulation (Bottom-Up)

Start from the smallest/base subproblems and iteratively build up to the answer, filling a table as you go. No recursion at all.

```cpp
long long fib(int n) {
    if (n <= 1) return n;
    vector<long long> dp(n + 1);
    dp[0] = 0;
    dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];   // build up from smaller answers
    }
    return dp[n];
}
```

- No recursion overhead, no stack overflow risk — generally faster in practice.
- Computes every subproblem up to `n`, even ones that might not strictly be needed.
- Requires figuring out the correct *iteration order* — trickier to get right initially than memoization.

**Rule of thumb:** think through the recursive (memoized) solution first — easier to reason about correctness. Convert to tabulation afterward for the more efficient final version if stack depth or performance matters.

### Space optimization

Often `dp[i]` only depends on the last one or two previous states (Fibonacci: `dp[i]` needs only `dp[i-1]` and `dp[i-2]`). In such cases the whole array isn't needed — just keep the last few values in variables, reducing O(n) space to O(1). [[Kadane's Algorithm]] is a concrete worked example of exactly this optimization.

```cpp
long long fib(int n) {
    if (n <= 1) return n;
    long long prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        long long curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

## Key concepts / vocabulary

- **State**: the set of parameters that uniquely identifies a subproblem. For Fibonacci, the state is just `n`. For more complex problems (e.g. 0/1 Knapsack), the state might be `(index, remaining capacity)` — meaning `dp` needs 2 dimensions.
- **Transition (recurrence relation)**: the formula expressing how to compute the current state's answer from smaller states' answers (e.g. `dp[i] = dp[i-1] + dp[i-2]`).
- **Base case**: the smallest subproblem(s) whose answer is known directly, without further recursion (e.g. `dp[0] = 0`, `dp[1] = 1`).

**Finding the correct state is the hard part of DP.** Most of the difficulty in a new DP problem is figuring out exactly what to put in `dp[...]` — get the state definition right, and the recurrence often follows naturally.

## When to use DP

- The problem asks for an **optimal value** — maximum, minimum, or count of ways (e.g. "minimum cost to reach the end", "number of distinct ways to climb stairs", "longest common subsequence").
- Making a decision at each step depends on results of solving smaller versions of the same problem.
- A greedy (locally-best-choice) approach provably fails on this problem.

## When NOT to use DP

- Subproblems don't overlap → plain recursion or divide-and-conquer is simpler and just as fast.
- A greedy approach is provably correct for the problem → greedy is simpler and faster, skip DP.
- The state space is too large to store → DP may not be feasible without further optimization.

## Common mistakes

- **Wrong state definition** — a `dp[i]` definition that doesn't capture enough information to correctly compute transitions leads to wrong answers, not a crash — the most dangerous kind of bug.
- **Missing/incorrect base cases** — wrong answers or infinite recursion.
- **Assuming a problem needs DP when it doesn't (or vice versa)** — always check for overlapping subproblems and optimal substructure first.
- **Wrong iteration order in tabulation** — filling `dp[i]` before the states it depends on have been computed yields garbage. Always fill dependencies before dependents.
- **Forgetting to memoize inside recursion** — writing the recursive solution but forgetting to check/store in the cache turns it back into plain (slow) recursion.

## Related concepts
- [[Recursion and Backtracking]] — DP with memoization is recursion + caching; understanding plain recursion is a prerequisite.
- [[Kadane's Algorithm]] — a concrete, space-optimized 1-D DP example; good first worked problem for seeing the DP mental model in action.
