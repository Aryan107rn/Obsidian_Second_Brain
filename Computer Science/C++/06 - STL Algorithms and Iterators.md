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

> [!warning] Trap
> `lower_bound`/`upper_bound`/`binary_search` require the range to be sorted first — using them on unsorted data gives silently wrong results (no error thrown).

## `<numeric>` Functions

```cpp
accumulate(v.begin(), v.end(), 0);                          // sum
accumulate(v.begin(), v.end(), 1, multiplies<int>());       // product
partial_sum(v.begin(), v.end(), result.begin());             // prefix sums
gcd(a, b); lcm(a, b);                                         // C++17+
iota(v.begin(), v.end(), 0);                                  // fill with 0,1,2,3...
```

## More `<algorithm>` Functions

### Predicates and counting
```cpp
count_if(v.begin(), v.end(), [](int x) { return x % 2 == 0; });   // how many satisfy a condition
all_of (v.begin(), v.end(), [](int x) { return x > 0; });          // every element
any_of (v.begin(), v.end(), [](int x) { return x < 0; });          // at least one
none_of(v.begin(), v.end(), [](int x) { return x < 0; });          // zero elements
```

### Erase-remove idiom (the standard way to delete from a vector)
`remove` / `remove_if` / `unique` do **not** shrink the container. They shuffle the kept elements to the front and return an iterator to the new logical end; you must `erase` the leftover tail.
```cpp
v.erase(remove(v.begin(), v.end(), 5), v.end());                 // delete every 5
v.erase(remove_if(v.begin(), v.end(), pred), v.end());           // delete by condition
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());                    // sort, then dedupe
// C++20 shortcuts: erase(v, 5);  erase_if(v, pred);
```

### Partial ordering (when you don't need a full sort)
```cpp
nth_element(v.begin(), v.begin() + k, v.end());   // v[k] becomes the k-th smallest; O(n) average
partial_sort(v.begin(), v.begin() + k, v.end());  // smallest k sorted at the front; O(n log k)
partition(v.begin(), v.end(), pred);              // elements satisfying pred first (not stable)
stable_partition(v.begin(), v.end(), pred);       // same, keeps relative order
is_sorted(v.begin(), v.end());                    // bool
```
`nth_element` is C++'s built-in Quickselect (see [[Arrays]] Pattern 7).

### Merging and set operations (inputs must be sorted)
```cpp
vector<int> c;
merge(a.begin(), a.end(), b.begin(), b.end(), back_inserter(c));
set_union       (a.begin(), a.end(), b.begin(), b.end(), back_inserter(c));
set_intersection(a.begin(), a.end(), b.begin(), b.end(), back_inserter(c));
set_difference  (a.begin(), a.end(), b.begin(), b.end(), back_inserter(c));
inplace_merge(v.begin(), v.begin() + mid, v.end());   // merge two sorted halves in place
```

### Ranges, min/max, copying, filling
```cpp
auto [lo, hi] = equal_range(v.begin(), v.end(), x);   // iterators to [lower_bound, upper_bound)
auto [mn, mx] = minmax_element(v.begin(), v.end());   // both iterators in one pass
int m = max({a, b, c});  int n = min({a, b, c});    // initializer-list form for 3+ values
copy(a.begin(), a.end(), b.begin());
transform(v.begin(), v.end(), v.begin(), [](int x) { return x * 2; });
fill(v.begin(), v.end(), -1);
iter_swap(it1, it2);
lexicographical_compare(a.begin(), a.end(), b.begin(), b.end());
```

### memset (C-style arrays only)
```cpp
int a[100];
memset(a, 0, sizeof a);      // all zeros
memset(a, -1, sizeof a);     // all -1
memset(a, 0x3f, sizeof a);   // ~1.06e9, a common "infinity" for int arrays
```
`memset` writes **bytes**, so only 0, -1 and byte-repeating patterns like `0x3f` give a meaningful `int`. For any other value use `fill`. `sizeof a` is only the array size when `a` is a real array, not a pointer parameter.

### Random shuffle (useful to defeat worst-case quicksort inputs)
```cpp
mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());
shuffle(v.begin(), v.end(), rng);
```

### accumulate's type is decided by the third argument
```cpp
vector<int> v = {2000000000, 2000000000};
accumulate(v.begin(), v.end(), 0);      // sums as int: overflow, wrong
accumulate(v.begin(), v.end(), 0LL);    // sums as long long: correct
```

## Common Mistakes

- Calling `lower_bound`/`upper_bound` on an unsorted vector.
- Calling `unique()` without sorting first — it only collapses **adjacent** duplicates.
- Forgetting `next_permutation` needs the range sorted ascending first to generate *all* permutations starting from the smallest.

- Calling `remove`/`unique` and expecting the vector to shrink; you must `erase` the tail (erase-remove idiom).
- `accumulate(..., 0)` on large values: the `0` makes the sum `int`; use `0LL`.

## Related Concepts

[[05 - STL Containers]]
[[07 - C++ Built-in Functions for DSA]]
