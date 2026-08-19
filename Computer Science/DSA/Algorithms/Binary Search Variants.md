# Binary Search Variants (Ternary, Exponential, Interpolation, Fibonacci Search)

These are less common alternatives to standard [[Binary Search]], each suited to a specific situation. Grouped here since each is a small variation on the same halving/narrowing idea rather than a fundamentally different technique.

## Ternary Search

**What it's for:** Finding the maximum/minimum of a **unimodal function** (strictly increases then strictly decreases, or vice versa) — not for searching a sorted array (binary search already does that better).

**How it works:** Split the range into three parts using two midpoints `m1`, `m2`. Compare `f(m1)` and `f(m2)` to determine which third of the range **cannot** contain the peak, and discard it.

```cpp
double ternarySearch(double lo, double hi, function<double(double)> f) {
    for (int i = 0; i < 100; i++) {   // enough iterations for precision
        double m1 = lo + (hi - lo) / 3;
        double m2 = hi - (hi - lo) / 3;
        if (f(m1) < f(m2)) lo = m1;   // peak is not in [lo, m1]
        else hi = m2;                  // peak is not in [m2, hi]
    }
    return (lo + hi) / 2;
}
```
- Time: O(log₃/₂ n) — slightly more comparisons per step than binary search, same asymptotic order.
- **Common mistake:** using ternary search on a sorted array to find a *value* (not a peak) — that's just binary search with extra steps; ternary search is for unimodal optimization, not membership search.

## Exponential Search

**What it's for:** Searching a sorted array (or unbounded/infinite sorted stream) when you **don't know the size** upfront, or the target is likely near the start.

**How it works:** Start with a bound of 1, double it repeatedly until the bound exceeds/contains the target's range, then run standard binary search within that bound.

```cpp
int exponentialSearch(vector<int>& a, int target) {
    if (a[0] == target) return 0;
    int i = 1;
    while (i < a.size() && a[i] <= target) i *= 2;
    return binarySearch(a, target, i / 2, min(i, (int)a.size() - 1));   // standard binary search on the found range
}
```
- Time: O(log p) to find the range (p = position of target), then O(log p) for the binary search itself — still O(log n) overall.
- **When it beats plain binary search:** unbounded/streaming input, or when the target is expected to be near the front (search cost scales with the target's position, not the full array size).

## Interpolation Search

**What it's for:** Sorted array with **uniformly distributed** values (like a sorted list of roughly evenly-spaced numbers) — can outperform binary search's O(log n).

**How it works:** Instead of always checking the middle, estimate the target's likely position proportionally — like flipping to the right page in a dictionary based on the letter, not always opening to the middle.

```cpp
int interpolationSearch(vector<int>& a, int target) {
    int low = 0, high = a.size() - 1;
    while (low <= high && target >= a[low] && target <= a[high]) {
        if (low == high) return (a[low] == target) ? low : -1;
        int pos = low + (double)(target - a[low]) * (high - low) / (a[high] - a[low]);
        if (a[pos] == target) return pos;
        if (a[pos] < target) low = pos + 1;
        else high = pos - 1;
    }
    return -1;
}
```
- Time: O(log log n) average on uniformly distributed data, **O(n) worst case** on skewed distributions (e.g. exponentially spaced values).
- **Common mistake:** using this blindly on non-uniform data — the worst case is genuinely bad, unlike binary search's guaranteed O(log n).

## Fibonacci Search

**What it's for:** Similar use case to binary search, but splits the range using **Fibonacci numbers** instead of exact halves — historically useful when division is expensive (division isn't needed, only addition/subtraction) or for searching data with non-uniform access costs.

**How it works:** Use the smallest Fibonacci number ≥ n to define the initial range, and narrow using Fibonacci offsets instead of `mid = (low+high)/2`.

- Time: O(log n), same order as binary search.
- **When it's actually chosen today:** rare in modern practice (division is cheap on modern CPUs) — mostly asked to test theoretical understanding, or used in specific embedded/legacy contexts.

---

## Quick reference — which variant to reach for
| Situation | Use |
|---|---|
| Sorted array, find exact value | Standard [[Binary Search]] |
| Find peak of unimodal function | Ternary Search |
| Unknown/unbounded array size | Exponential Search |
| Uniformly distributed sorted values | Interpolation Search |
| Division-expensive environment (rare today) | Fibonacci Search |

## Related concepts
- [[Binary Search]] — the base technique all four of these adapt for a specific constraint.
