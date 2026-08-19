# Euclidean Algorithm (GCD)

## What is it?
An efficient algorithm to find the **Greatest Common Divisor** (GCD) of two numbers — the largest number that divides both without a remainder.

## Why does it exist?
Naively finding GCD by checking every number up to `min(a,b)` takes O(min(a,b)) time. The Euclidean algorithm does it in O(log(min(a,b))) using a simple mathematical identity.

## The key identity
`gcd(a, b) = gcd(b, a % b)`, with base case `gcd(a, 0) = a`.

**Why this is true:** any number that divides both `a` and `b` must also divide `a - b`, `a - 2b`, ... and therefore `a % b` (the remainder after removing as many `b`s as possible). So the set of common divisors of `(a, b)` is exactly the same as the set of common divisors of `(b, a % b)` — the GCD doesn't change, but the numbers shrink fast.

## Implementation
```cpp
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}
// Iterative (avoids recursion overhead):
int gcdIter(int a, int b) {
    while (b) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

## LCM, built from GCD
```cpp
long long lcm(int a, int b) {
    return (long long)a / gcd(a, b) * b;   // divide before multiplying to avoid overflow
}
```

## Complexity
- Time: O(log(min(a,b))) — each step roughly halves the smaller number (provably, via Fibonacci-worst-case analysis).
- Space: O(log(min(a,b))) recursive / O(1) iterative.

## When to use it
Any problem needing GCD/LCM — simplifying fractions, finding a common period, "smallest array that can be formed by repeating a pattern," and as a subroutine in Extended Euclidean (modular inverse) or number-theory problems.

## Common mistakes
- Computing `a * b / gcd(a,b)` for LCM instead of `a / gcd(a,b) * b` — the former can overflow `int`/`long long` before the division happens, even when the final LCM would fit.
- Assuming `gcd(0, 0)` is defined — conventionally treated as 0, but worth checking problem constraints.
- Off-by-one: `gcd(a, 0) = a`, not `gcd(a,0) = 0` — a common transcription error.

## Related concepts
- [[Recursion]] — this is a clean textbook example of the "shrink toward base case" recursive structure (Pattern 1).
