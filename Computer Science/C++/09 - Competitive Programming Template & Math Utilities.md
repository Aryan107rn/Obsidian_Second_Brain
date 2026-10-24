# 09 - Competitive Programming Template & Math Utilities

Everything you need around the algorithm itself: a ready template, how to read constraints, modular arithmetic, and the C++ traps that silently cost marks.

## Standard Template

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll  = long long;
using pii = pair<int,int>;
const int  MOD  = 1e9 + 7;
const int  INF  = 1e9;
const ll   LINF = 1e18;

void solve() {
    // read input, compute, print
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int t = 1;
    // cin >> t;
    while (t--) solve();
    return 0;
}
```
- Use `"\n"` instead of `endl` (which flushes every time).
- On LeetCode you only write the function body, but the same habits apply.

## Reading Constraints: What Complexity Can I Afford?

A typical judge does roughly **10⁸ simple operations per second**. Match the constraint to the algorithm before you code.

| Constraint on n | Target complexity | Typical technique |
|---|---|---|
| n ≤ 10 | O(n!) | permutations, brute-force backtracking |
| n ≤ 20 | O(2ⁿ · n) | bitmask, subsets, [[Bit Manipulation]] DP |
| n ≤ 40 | O(2^(n/2)) | meet in the middle |
| n ≤ 500 | O(n³) | Floyd-Warshall, 3-nested DP |
| n ≤ 5,000 | O(n²) | 2D DP, LCS/edit distance |
| n ≤ 10⁵ | O(n log n) | sorting, heap, binary search, segment tree |
| n ≤ 10⁶ | O(n) or tight O(n log n) | two pointers, hashing, prefix sums |
| n ≤ 10⁸ | O(n) with tiny constant | sieve, linear scans |
| n ≤ 10¹⁸ | O(log n) or O(1) | binary search on answer, fast power, math formula |

If your first idea is O(n²) and n is 10⁵, you need a pattern change (hashing, two pointers, sorting, heap), not micro-optimization.

## Overflow Defence

```cpp
int a = 100000, b = 100000;
long long wrong = a * b;            // multiplies as int FIRST: overflow
long long right = 1LL * a * b;      // promote before multiplying
```
- Sums and products of `n ≤ 10⁵` values can exceed `int`: default to `long long` when unsure.
- `accumulate(v.begin(), v.end(), 0LL)`, not `0` ([[06 - STL Algorithms and Iterators]]).
- Use `__int128` for a product of two `long long` values before taking a modulus (GCC only).
- Check an overflow before it happens: `if (a > LLONG_MAX / b)` instead of computing and testing afterwards.
- `abs(INT_MIN)` and `-INT_MIN` overflow.

## `size()` Is Unsigned

```cpp
vector<int> v;
for (int i = 0; i < v.size() - 1; i++) { }   // v.size()-1 wraps to ~4e9 when empty: runs forever / crashes
for (int i = 0; i + 1 < (int)v.size(); i++) { }   // safe
```
Cast `(int)v.size()` whenever you subtract from it or compare with a signed value.

## Modular Arithmetic

Answers "modulo 10⁹+7" exist to keep numbers inside 64 bits. The rules:

```cpp
(a + b) % MOD
(a - b % MOD + MOD) % MOD        // subtraction: add MOD to stay non-negative
1LL * a % MOD * b % MOD          // multiplication: 64-bit before the %
```
**Division does not work directly** (`(a / b) % MOD` is wrong). Multiply by the **modular inverse**; for a prime modulus, Fermat's little theorem gives `inv(b) = b^(MOD-2) mod MOD`.

```cpp
ll power(ll b, ll e, ll m = MOD) {          // binary exponentiation, O(log e)
    ll r = 1; b %= m;
    while (e > 0) {
        if (e & 1) r = r * b % m;
        b = b * b % m;
        e >>= 1;
    }
    return r;
}
ll inv(ll a) { return power(a, MOD - 2); }   // MOD must be prime and gcd(a, MOD) = 1
```
Related note: [[Euclidean Algorithm (GCD)]] covers extended Euclid, the inverse method when the modulus is not prime.

## nCr Modulo a Prime

Precompute factorials and inverse factorials once, then every query is O(1).

```cpp
const int N = 1e6 + 5;
ll fact[N], inv_fact[N];

void init() {
    fact[0] = 1;
    for (int i = 1; i < N; i++) fact[i] = fact[i - 1] * i % MOD;
    inv_fact[N - 1] = power(fact[N - 1], MOD - 2);
    for (int i = N - 1; i > 0; i--) inv_fact[i - 1] = inv_fact[i] * i % MOD;
}
ll nCr(int n, int r) {
    if (r < 0 || r > n) return 0;
    return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD;
}
```
For small n (≤ ~2000) with no modulus issues, Pascal's triangle is simpler: `C[i][j] = C[i-1][j-1] + C[i-1][j]`.

## Number-Theory Helpers

```cpp
__gcd(a, b);  gcd(a, b);  lcm(a, b);          // gcd/lcm need C++17 <numeric>

bool isPrime(ll n) {                           // O(sqrt n)
    if (n < 2) return false;
    for (ll i = 2; i * i <= n; i++) if (n % i == 0) return false;
    return true;
}
```
For all primes up to N use [[Sieve of Eratosthenes]]. For prime factorization, divide out `i` from 2 up to `sqrt(n)`, and whatever remains above 1 is the last prime factor.

## Floating Point

```cpp
fabs(a - b) < 1e-9;                  // never compare doubles with ==
cout << fixed << setprecision(6) << x;   // fixed number of decimals
```

## Recursion Depth

Each recursive call uses stack memory. Depths around 10⁴ to 10⁵ can overflow the stack (the exact limit depends on the judge). If recursion depth is `O(n)` with `n ≥ 10⁵`, convert to an iterative loop or an explicit `stack` (see [[Stack & Queues]]). Large arrays also belong **outside** `main` (global) or in a `vector`, not as big local arrays.

## Debug Helper

```cpp
#define debug(x) cerr << #x << " = " << (x) << "\n"
```
`cerr` output does not mix with the program's `cout` answer.

## Common Mistakes
- `int` overflow in `a * b` before assigning to `long long`.
- `v.size() - 1` on an empty vector.
- Subtracting under a modulus without adding `MOD`.
- Dividing under a modulus instead of multiplying by the inverse.
- Using the modular inverse formula when the modulus is not prime.
- Comparing floating-point numbers with `==`.
- `memset` with a value other than 0, -1 or a byte pattern.
- Using `endl` in a loop that prints 10⁵ lines.
- Large local arrays causing a stack overflow.

## Related Concepts
[[01 - Data Types and Fundamentals]]
[[07 - C++ Built-in Functions for DSA]]
[[10 - Data Structure & Graph Templates]]
[[Euclidean Algorithm (GCD)]]
[[Sieve of Eratosthenes]]
