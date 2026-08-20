# 01 - Data Types and Fundamentals

## Compilation & Basic Structure
```cpp
#include <bits/stdc++.h>   // GCC-only — pulls in almost all STL headers; fine for contests/interviews,
using namespace std;        // avoid if asked for "portable"/production code — include specific headers instead

int main() {
    ios_base::sync_with_stdio(false); // speeds up cin/cout
    cin.tie(NULL);                    // untie cin from cout
    return 0;
}
```

## Data Type Sizes & Ranges (CRITICAL for DSA)
| Type | Size (64-bit) | Range | Header for limits |
|---|---|---|---|
| `bool` | 1 byte | true/false | - |
| `char` | 1 byte | -128 to 127 (signed) | `<climits>` |
| `short` | 2 bytes | -32,768 to 32,767 | `<climits>` |
| `int` | 4 bytes | -2,147,483,648 to 2,147,483,647 (~2.1×10⁹) | `<climits>` |
| `unsigned int` | 4 bytes | 0 to 4,294,967,295 | `<climits>` |
| `long` | 8 bytes (Linux 64-bit) | ~±9.2×10¹⁸ | `<climits>` |
| `long long` | 8 bytes | ~±9.2×10¹⁸ | `<climits>` |
| `unsigned long long` | 8 bytes | 0 to ~1.8×10¹⁹ | `<climits>` |
| `float` | 4 bytes | ~±3.4×10³⁸, 6-7 sig digits | `<cfloat>` |
| `double` | 8 bytes | ~±1.7×10³⁰⁸, 15-16 sig digits | `<cfloat>` |

**Getting limits programmatically:**
```cpp
#include <climits>
INT_MAX, INT_MIN, LLONG_MAX, LLONG_MIN, UINT_MAX, ULLONG_MAX

#include <limits>
numeric_limits<int>::max();
numeric_limits<long long>::max();

sizeof(int);                     // 4
sizeof(arr)/sizeof(arr[0]);      // element count of a static C-array
```

## Integer Overflow & Common Traps
- `int a = 1e9, b = 1e9; int c = a*b;` → **overflows**, undefined behavior. Fix: `long long c = (long long)a * b;`
- **Rule of thumb:** if `n` up to 10⁵ and you're multiplying/summing (e.g. `n*n`), the result can hit 10¹⁰ — exceeds `int`'s ~2.1×10⁹ ceiling. Default to `long long` (`typedef long long ll;`) whenever unsure.
- Binary search midpoint: `mid = (l+r)/2` can overflow if `l, r` are near `INT_MAX` → use `mid = l + (r-l)/2`.
- Division/modulo with negatives truncates **toward zero** (not floor): `-7/2 == -3`, `-7%2 == -1`.

## Fast I/O
```cpp
ios_base::sync_with_stdio(false);
cin.tie(NULL);
// avoid endl in loops (flushes buffer each time) — use "\n" instead
```

## Common Mistakes
- Forgetting `long long` when constraints imply sums/products can exceed 2×10⁹ — silent wrong answer, no crash.
- Using `bits/stdc++.h` in code explicitly asked to be "portable" (it's GCC-only).
- Assuming `%` behaves like floor-mod with negative numbers (it doesn't in C++).

## Related Concepts
[[02 - Pointers References and Memory Management]]
[[07 - C++ Built-in Functions for DSA]]
