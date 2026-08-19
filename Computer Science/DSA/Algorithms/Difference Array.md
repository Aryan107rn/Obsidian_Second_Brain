# Difference Array

## What is it?
A technique for applying **many range-update operations** efficiently — "add value v to every element from index l to r" — without touching each element in the range directly.

## Why does it exist?
Naively applying `q` range updates to an array of size `n` costs O(q·n) (looping through each range every time). If you only need the **final** array after all updates (not intermediate states), the difference array reduces this to O(q + n).

## How it works
Build an auxiliary array `diff` where `diff[i] = a[i] - a[i-1]`. To add `v` to range `[l, r]`, you only need two O(1) edits:
```
diff[l] += v
diff[r+1] -= v   (if r+1 is in bounds)
```
After all updates, take the **prefix sum** of `diff` to recover the final array — the `+v` at `l` "starts" the addition, and `-v` at `r+1` "cancels" it out beyond the range.

```cpp
vector<int> diff(n + 1, 0);

void rangeAdd(int l, int r, int v) {
    diff[l] += v;
    diff[r + 1] -= v;
}

vector<int> buildFinalArray() {
    vector<int> result(n);
    int running = 0;
    for (int i = 0; i < n; i++) {
        running += diff[i];
        result[i] = running;   // prefix sum of diff = actual value at i
    }
    return result;
}
```

## Complexity
- Each range update: O(1).
- Final reconstruction: O(n) once, after all updates.
- Total for q updates: O(q + n), vs O(q·n) naive.

## When to use it
Multiple **range-add** operations applied to an array, where you only need the final result (not queries in between updates). Classic phrasing: "apply these q operations, each adding v to range [l,r], then print the final array."

## When NOT to use it
- Need the array's state **between** updates (difference array only gives correct values after *all* updates are applied and prefix-summed).
- Updates aren't uniform range-adds (e.g. range multiply, range assign) — needs a different technique (segment tree with lazy propagation).

## Common mistakes
- Forgetting to bounds-check `r+1` before subtracting — if `r == n-1`, `r+1 == n` is still valid (diff array is sized `n+1` exactly for this).
- Trying to read the "current" array value mid-updates without reconstructing via prefix sum first — the diff array itself isn't the answer, its prefix sum is.

## Related concepts
- [[Arrays]] — Pattern 4 (Prefix Sum) is the inverse operation this technique relies on to reconstruct the final array.
