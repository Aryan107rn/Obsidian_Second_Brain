# Maximum Product Subarray

## What is it?
Find the contiguous subarray with the **largest product** (not sum) of its elements. Looks like a Kadane's variant, but negative numbers break the simple "extend or restart" logic that works for sums.

## Why is this harder than Kadane's (max sum)?
With sums, a negative running total is always bad, so you'd restart. With **products**, a negative number flips the sign — a very negative running product can become the **largest** product if multiplied by another negative number. You can't just track one running value; you need to track both the running **max** and running **min** product ending at each index, because the min (a large negative number) might become the max on the next multiplication.

## How it works
At each index, the new max product ending here is the largest of: the element alone, (previous max × element), or (previous min × element) — since a negative element flips previous min into a candidate max.

```cpp
int maxProduct(vector<int>& nums) {
    int maxProd = nums[0], minProd = nums[0], result = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        int x = nums[i];
        if (x < 0) swap(maxProd, minProd);   // negative flips which was bigger
        maxProd = max(x, maxProd * x);
        minProd = min(x, minProd * x);
        result = max(result, maxProd);
    }
    return result;
}
```

**Why the swap trick works:** if `x` is negative, multiplying flips the ordering — what was the min (very negative) times a negative `x` could become the new max, and vice versa. Swapping `maxProd`/`minProd` before the multiplication lets the same two lines (`max(x, maxProd*x)`, `min(x, minProd*x)`) handle both cases correctly.

## Complexity
- Time: O(n) single pass. Space: O(1).

## When to use it
Any "maximum product of contiguous subarray" phrasing — the product analog of [[Kadane's Algorithm]].

## Common mistakes
- Applying plain Kadane's logic (tracking only max) — fails silently on inputs with negative numbers, since the true answer might come from multiplying two negatives.
- Forgetting to also update `result` from `minProd` when the **entire product needs to be minimized instead** (a related but different problem).
- Zero handling: a zero resets both `maxProd` and `minProd` to 0 naturally (since `max(0, prev*0)` and `min(0, prev*0)` both evaluate to 0) — no special-casing needed, but worth verifying by hand once.

## Related concepts
- [[Kadane's Algorithm]] — the sum version of this exact "extend or restart" idea; comparing the two clarifies why products need dual tracking.
