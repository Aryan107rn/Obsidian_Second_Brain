# Manacher's Algorithm (Longest Palindromic Substring in O(n))

## What is it?
Finds the **longest palindromic substring** of a string in O(n) time — a dramatic improvement over the O(n²) "expand around every center" approach.

## Why does it exist?
The brute-force approach checks every possible center (2n-1 of them, counting between-character centers for even-length palindromes) and expands outward — O(n) per center, O(n²) total. Manacher's algorithm reuses information from previously computed palindromes to skip redundant expansion work, achieving O(n).

## The core trick
1. **Transform the string** to handle even and odd-length palindromes uniformly: insert a separator (e.g. `#`) between every character, including the ends. `"aba"` → `"#a#b#a#"`. Now every palindrome (odd or even in the original) becomes odd-length in the transformed string, centered on some index.
2. **Maintain a "rightmost palindrome boundary"** found so far: `center C` and its right edge `R`.
3. For each new position `i`, if it's within the current rightmost palindrome's boundary, use the **mirror position** (`2C - i`) to initialize a lower bound on `i`'s palindrome radius **without re-expanding from scratch** — palindromic symmetry means much of the work is already known.
4. Expand further only if needed, then update `C`/`R` if this palindrome extends past the previous rightmost boundary.

```cpp
string longestPalindrome(string s) {
    string t = "#";
    for (char c : s) { t += c; t += '#'; }   // transformed string
    int n = t.size();
    vector<int> p(n, 0);   // p[i] = radius of palindrome centered at i (in transformed string)
    int center = 0, right = 0;

    for (int i = 0; i < n; i++) {
        if (i < right) p[i] = min(right - i, p[2 * center - i]);  // mirror trick
        while (i - p[i] - 1 >= 0 && i + p[i] + 1 < n && t[i - p[i] - 1] == t[i + p[i] + 1])
            p[i]++;
        if (i + p[i] > right) { center = i; right = i + p[i]; }   // update boundary
    }

    int maxLen = 0, centerIdx = 0;
    for (int i = 0; i < n; i++)
        if (p[i] > maxLen) { maxLen = p[i]; centerIdx = i; }

    int start = (centerIdx - maxLen) / 2;   // map back to original string indices
    return s.substr(start, maxLen);
}
```

## Complexity
- Time: O(n) — each index is visited a bounded number of times due to the mirror trick avoiding redundant re-expansion (amortized analysis).
- Space: O(n) for the transformed string and `p` array.

## When to use it
"Longest palindromic substring" specifically when O(n²) brute force / DP is too slow (large input, e.g. n > 10⁴). For smaller inputs, the simpler "expand around center" O(n²) approach is easier to write correctly under time pressure and often sufficient.

## Common mistakes
- Forgetting the string transformation step — without inserting separators, odd- and even-length palindromes need separate handling, complicating the mirror logic significantly.
- Off-by-one when mapping the palindrome center/radius back to the **original** string's indices after finding the answer in the transformed string.
- Assuming this is always necessary — for most interview settings, the O(n²) expand-around-center solution is acceptable and much easier to derive under pressure; reach for Manacher's only when explicitly required.

## Related concepts
- [[Strings]] — Pattern 1 (Two Pointer) is the building block for the simpler O(n²) expand-around-center approach that Manacher's improves upon.
