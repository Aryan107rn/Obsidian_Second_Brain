# Sieve of Eratosthenes

## What is it?
An algorithm to find **all prime numbers up to N** in one pass, far faster than checking each number individually for primality.

## Why does it exist?
Checking if a single number is prime by trial division takes O(√n). Doing that for every number up to N costs O(N√N) — too slow when N is large (e.g. 10⁷). The Sieve instead **eliminates multiples**, giving all primes up to N in O(N log log N).

## How it works
Start with a boolean array marking every number as "possibly prime." Starting from 2, for each number still marked prime, cross out all its multiples (they can't be prime — they're divisible by that number). The next unmarked number is the next prime; repeat.

```cpp
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; (long long)i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i)   // start at i*i — smaller multiples already crossed out
                isPrime[j] = false;
        }
    }
    return isPrime;
}
```

## Why start crossing out at `i*i`?
Any smaller multiple of `i` (like `2i`, `3i`, ... `(i-1)i`) already got crossed out by a smaller prime factor earlier in the loop. `i*i` is the first multiple of `i` that hasn't been eliminated yet.

## Complexity
- Time: O(N log log N) — near-linear in practice.
- Space: O(N) for the boolean array.

## When to use it
Any problem needing **all primes up to N**, or repeated primality checks over a fixed range — precompute once, O(1) lookup after.

## When NOT to use it
- Checking primality of a **single large number** (e.g. up to 10¹⁸) — trial division up to √n or Miller-Rabin is better; sieving that range is infeasible.
- N is small and used once — plain trial division per number is simpler and fast enough.

## Common mistakes
- Starting the inner loop at `2*i` instead of `i*i` — not wrong, just wastes work re-crossing already-eliminated multiples.
- Integer overflow: use `(long long)i * i` when N is large enough that `i*i` could overflow `int`.
- Off-by-one: array must be sized `n+1` to include index n itself.

## Related concepts
- [[Hashing]] — precomputed lookup tables follow the same "pay once, query O(1) forever" trade-off.
