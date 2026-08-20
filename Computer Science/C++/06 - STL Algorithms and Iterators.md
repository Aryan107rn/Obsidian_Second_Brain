# 06 - STL Algorithms and Iterators

## Iterators — Quick Reference
An iterator is a generalized pointer that lets algorithms traverse any container the same way, regardless of its internal structure.
```cpp
v.begin(), v.end();        // forward range
v.rbegin(), v.rend();      // reverse range
*it;                        // dereference — access the element
it++;                       // move forward
next(it), prev(it);         // <iterator> header, non-mutating
distance(it1, it2);         // number of elements between two iterators
```
A range-based for loop (`for (auto& x : v)`) is the idiomatic modern equivalent for most simple traversals.

## Key `<algorithm>` Functions
| Function | Purpose | Complexity |
|---|---|---|
| `sort(begin, end)` | sort ascending | O(n log n) |
| `sort(begin, end, cmp)` | custom comparator | O(n log n) |
| `stable_sort(begin, end)` | preserves relative order of equal elements | O(n log n) |
| `reverse(begin, end)` | reverse a range | O(n) |
| `binary_search(begin, end, val)` | true/false if present (needs sorted range) | O(log n) |
| `lower_bound(begin, end, val)` | iterator to first element ≥ val | O(log n) |
| `upper_bound(begin, end, val)` | iterator to first element > val | O(log n) |
| `min_element` / `max_element(begin, end)` | iterator to min/max | O(n) |
| `count(begin, end, val)` | count occurrences | O(n) |
| `find(begin, end, val)` | iterator to first match or `end()` | O(n) |
| `unique(begin, end)` | removes **consecutive** duplicates (sort first!) | O(n) |
| `next_permutation` / `prev_permutation(begin, end)` | rearranges to next/prev lexicographic permutation | O(n) |
| `rotate(begin, mid, end)` | left-rotate range so `mid` becomes the new start | O(n) |
| `fill(begin, end, val)` | fill range with a value | O(n) |
| `swap(a, b)` | swap two values | O(1) |
| `clamp(val, lo, hi)` | bound value within [lo, hi] | O(1) |

**Trap:** `lower_bound`/`upper_bound`/`binary_search` require the range to be sorted first — using them on unsorted data gives silently wrong results (no error thrown).

## `<numeric>` Functions
```cpp
accumulate(v.begin(), v.end(), 0);                          // sum
accumulate(v.begin(), v.end(), 1, multiplies<int>());       // product
partial_sum(v.begin(), v.end(), result.begin());             // prefix sums
gcd(a, b); lcm(a, b);                                         // C++17+
iota(v.begin(), v.end(), 0);                                  // fill with 0,1,2,3...
```

## Common Mistakes
- Calling `lower_bound`/`upper_bound` on an unsorted vector.
- Calling `unique()` without sorting first — it only collapses **adjacent** duplicates.
- Forgetting `next_permutation` needs the range sorted ascending first to generate *all* permutations starting from the smallest.

## Related Concepts
[[05 - STL Containers]]
[[07 - C++ Built-in Functions for DSA]]
