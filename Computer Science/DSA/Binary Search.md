# Binary Search

## Concept
Repeatedly halve the search space by comparing the middle element to a target/condition. Eliminates half the possibilities each step.

**When to apply (the big signal):** Search space is **sorted**, OR the answer has a **monotonic property** (a condition that's false...false...true...true, or true...true...false...false across the range) — even if the array itself isn't sorted. If you can define "is this value good enough?" as a yes/no that flips exactly once, binary search applies.

## How it works
Maintain `low`, `high`. Check `mid`. Discard the half that can't contain the answer. Repeat until `low > high`.

---

## Pattern 1: Standard Binary Search (exact value in sorted array)
**When to apply:** Array is sorted, looking for exact element or its position.
```cpp
int binarySearch(vector<int>& a, int target) {
    int low = 0, high = a.size() - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;   // avoids overflow
        if (a[mid] == target) return mid;
        else if (a[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```
- Time: O(log n), Space: O(1)
- **Remember:** `low + (high-low)/2`, not `(low+high)/2` — avoids integer overflow on large indices.

## Pattern 2: Lower Bound / Upper Bound
**When to apply:** Need first index ≥ target (lower bound) or first index > target (upper bound) — e.g. insertion point, counting occurrences.
```cpp
int lowerBound(vector<int>& a, int target) {   // first index with a[i] >= target
    int low = 0, high = a.size();
    while (low < high) {
        int mid = low + (high - low) / 2;
        if (a[mid] < target) low = mid + 1;
        else high = mid;
    }
    return low;
}
// upperBound: same but condition is a[mid] <= target
```
- Time: O(log n)
- **Remember:** `std::lower_bound`/`upper_bound` in C++ STL do exactly this — use them directly instead of hand-rolling in practice. Count of an element = `upperBound(x) - lowerBound(x)`.

## Pattern 3: Search in Rotated Sorted Array
**When to apply:** Array was sorted then rotated at an unknown pivot — one half of any `[low, high]` window is always still sorted; use that to decide direction.
```cpp
int searchRotated(vector<int>& a, int target) {
    int low = 0, high = a.size() - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (a[mid] == target) return mid;
        if (a[low] <= a[mid]) {                 // left half sorted
            if (a[low] <= target && target < a[mid]) high = mid - 1;
            else low = mid + 1;
        } else {                                 // right half sorted
            if (a[mid] < target && target <= a[high]) low = mid + 1;
            else high = mid - 1;
        }
    }
    return -1;
}
```
- Time: O(log n)
- **Remember:** The key check is always "which half is sorted" (`a[low] <= a[mid]`), then check if target lies in that sorted half's range.

## Pattern 4: Binary Search on Answer (search space, not array)
**When to apply:** You're asked to minimize/maximize some value (capacity, speed, days, distance) subject to a feasibility check that's monotonic — "can this value achieve the goal?" flips from false to true (or true to false) exactly once as the value increases. Classic phrasing: "minimum X such that condition holds" or "maximum X such that condition holds."

Examples: Koko eating bananas (min speed to finish in h hours), ship packages within D days (min capacity), aggressive cows / book allocation (max-min or min-max distance).
```cpp
// Template: find minimum X such that isFeasible(X) is true
int binarySearchOnAnswer(int lo, int hi, function<bool(int)> isFeasible) {
    int ans = hi;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (isFeasible(mid)) { ans = mid; hi = mid - 1; }  // try smaller
        else lo = mid + 1;
    }
    return ans;
}
```
- Time: O(log(range) × cost of feasibility check)
- **Remember:** This is the single most important binary search pattern for interviews beyond basic lookup. The trigger phrase is "minimum/maximum value such that [condition]" — if you can write a `bool isFeasible(x)` that's monotonic, binary search applies even though there's no "array" in sight.

## Pattern 5: Binary Search on 2D Matrix
**When to apply:** Matrix is row-wise and column-wise sorted (or fully sorted if flattened) — treat it as a 1D sorted array via index mapping, or use a staircase-search from a corner.
```cpp
// Case: each row sorted, first element of row > last element of previous row (fully sorted flattened)
bool searchMatrix(vector<vector<int>>& mat, int target) {
    int m = mat.size(), n = mat[0].size();
    int low = 0, high = m * n - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        int val = mat[mid / n][mid % n];
        if (val == target) return true;
        val < target ? low = mid + 1 : high = mid - 1;
    }
    return false;
}
```
- Time: O(log(m·n))
- **Remember:** Row-major index mapping: `row = mid / n, col = mid % n`. If rows/cols are sorted independently (not globally), use the O(m+n) staircase search from top-right instead — binary search per row only works with the global-sort guarantee.

## Pattern 6: Finding Peak Element
**When to apply:** Array where you need an index i such that `a[i] > a[i-1]` and `a[i] > a[i+1]` (local max) — comparing `a[mid]` to `a[mid+1]` tells you which direction has a peak, even without global sort.
```cpp
int findPeak(vector<int>& a) {
    int low = 0, high = a.size() - 1;
    while (low < high) {
        int mid = low + (high - low) / 2;
        if (a[mid] < a[mid + 1]) low = mid + 1;   // peak is to the right
        else high = mid;                           // peak is at mid or left
    }
    return low;
}
```
- Time: O(log n)
- **Remember:** Works because if `a[mid] < a[mid+1]`, the array is "rising" at mid, guaranteeing a peak exists somewhere to the right (array boundaries count as -infinity). No global sortedness needed — just local monotonic comparison.

---

## When to apply — quick reference
- Sorted array, exact value → **Standard binary search**
- Sorted array, insertion point / first-or-last occurrence / count → **Lower/upper bound**
- Sorted-then-rotated array → **Search in rotated array**
- "Minimize/maximize X such that condition(X) holds" phrasing, no array needed → **Binary search on answer**
- Row/column sorted matrix → **2D matrix binary search**
- Need a local max with rising/falling comparison → **Peak element**
- General rule of thumb: if brute force is "try every value and check," and checking is monotonic, binary search turns O(n) or O(n²) into O(log n) or O(n log n).

## Common mistakes
- `(low+high)/2` overflow on large arrays — always use `low + (high-low)/2`.
- Off-by-one in lower/upper bound: using `<=` vs `<` in the while condition inconsistently.
- Applying binary search to unsorted, non-monotonic data (no valid "which half to discard" logic exists).
- Binary search on answer: forgetting to also test whether the range boundaries (`lo`/`hi`) themselves are valid feasibility bounds before starting.
- Rotated array search: checking `a[low] < a[mid]` incorrectly when `low == mid` (single-element range) — use `<=`.
- Infinite loop risk in peak-finding/answer-search templates if `high = mid` and `low = mid` are both used incorrectly for the same comparison (should always shrink the range each iteration).

## Related concepts
[[Arrays]]
[[Sorting Techniques]]
