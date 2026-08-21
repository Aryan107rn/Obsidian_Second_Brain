# 01 - Data Types and Fundamentals

## What is C++?
C++ is a **compiled, statically-typed** language. *Compiled* means source code is fully translated into machine code before the program runs (unlike Python/JS, which are interpreted line-by-line). *Statically-typed* means every variable's type is fixed and checked at compile time, not runtime. This combination is why C++ is fast and used for performance-critical software — the compiler does all type-checking and translation up front, so at runtime the CPU just executes raw instructions with no interpreter overhead.

## The Compilation Model
A `.cpp` file can't just be "run" — it goes through four stages:
```
source.cpp → preprocessor → compiler → assembler (.o file) → linker → executable
```
1. **Preprocessor** — handles `#` directives: `#include` (pastes in header contents), `#define` (macro substitution).
2. **Compiler** — translates expanded code into assembly for your CPU. Type errors are caught here.
3. **Assembler** — converts assembly into an object file (`.o`/`.obj`) — real machine code, but not yet runnable.
4. **Linker** — combines object file(s) with libraries into one executable.

**Why this matters:** compile errors (syntax, type mismatches) happen at step 2. Linker errors (e.g. "undefined reference to `foo()`") happen at step 4 — your code is syntactically fine, but the implementation of something you declared can't be found. Beginners often confuse these two error categories.

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
A variable is a named, typed slot of memory — C++ requires the type upfront so the compiler knows how many bytes to reserve and how to interpret the bits stored there.

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
A 4-byte `int` can only hold roughly ±2.1 billion. Exceeding that range doesn't throw an error — it silently **wraps around**:
```cpp
int x = 2147483647;   // INT_MAX
x = x + 1;             // becomes -2147483648 — no warning, no crash
```
- **Rule of thumb:** if `n` up to 10⁵ and you're multiplying/summing (e.g. `n*n`), the result can hit 10¹⁰ — exceeds `int`'s ceiling. Default to `long long` (`typedef long long ll;`) whenever unsure.
- Binary search midpoint: `mid = (l+r)/2` can overflow if `l, r` are near `INT_MAX` → use `mid = l + (r-l)/2`.
- Division/modulo with negatives truncates **toward zero** (not floor): `-7/2 == -3`, `-7%2 == -1`.

## const
Declaring a variable `const` prevents reassignment after initialization — documents intent and lets the compiler catch accidental modification as a compile error instead of a runtime bug.
```cpp
const double PI = 3.14159;
```

## Operators
- **Arithmetic:** `+ - * / %` — `%` only works on integers. **Integer division truncates**: `7 / 2` gives `3`, not `3.5`, because both operands are `int`. At least one operand must be floating-point to get a decimal result: `7.0 / 2` → `3.5`. One of the most common beginner mistakes in C++.
- **Relational:** `== != < > <= >=` — return `bool`.
- **Logical:** `&& || !` — short-circuit evaluation (if the left side of `&&` is false, the right side isn't evaluated at all).
- **Assignment:** `= += -= *= /=`.

**Common mistake:** `if (x = 5)` instead of `if (x == 5)` — this *compiles* (assigns 5 to x; the assignment expression evaluates to 5, which is truthy) but silently does the wrong thing every time. Don't ignore compiler warnings about this.

## Control Flow
```cpp
if (score >= 90) grade = 'A';
else if (score >= 80) grade = 'B';
else grade = 'F';

for (int i = 0; i < 5; i++) { }     // use when the iteration count is known in advance
while (condition) { }                // condition checked BEFORE each iteration — count unknown ahead of time
do { } while (condition);            // condition checked AFTER — body always runs at least once
```

## Functions
```cpp
int add(int a, int b) {   // return type | name | parameters
    return a + b;
}
```
See [[02 - Pointers References and Memory Management]] for pass-by-value vs pass-by-reference — critical for both correctness and performance with large objects.

## Arrays and Strings (C-style basics)
```cpp
int nums[5] = {1, 2, 3, 4, 5};   // fixed-size C-style array — size baked in, can't grow/shrink
string name = "Alex";              // dynamic, resizable, has built-in methods
name += " Smith";                  // concatenation works naturally, unlike C-style char arrays
```
**Edge case — no bounds checking:** a C-style array doesn't know its own size at runtime, and an out-of-range index doesn't throw:
```cpp
int nums[5] = {1,2,3,4,5};
cout << nums[10];   // compiles and runs — reads garbage/undefined memory, not a reliable crash
```
This is a major source of real-world bugs (buffer overflows). Modern C++ prefers `vector` (dynamic, part of STL) and `array` (fixed-size but bounds-checkable via `.at()`) over raw C-style arrays — see [[05 - STL Containers]].

## Fast I/O
```cpp
ios_base::sync_with_stdio(false);
cin.tie(NULL);
// avoid endl in loops (flushes buffer each time) — use "\n" instead
```

## Common Mistakes
- Confusing `=` and `==` in conditions.
- Expecting `7 / 2` to give `3.5` (integer division truncates).
- Forgetting C-style arrays have no bounds checking.
- Forgetting `long long` when constraints imply sums/products can exceed 2×10⁹.
- Using `bits/stdc++.h` in code explicitly asked to be "portable" (it's GCC-only).
- Assuming `%` behaves like floor-mod with negative numbers (it doesn't in C++).

## Related Concepts
[[02 - Pointers References and Memory Management]]
[[07 - C++ Built-in Functions for DSA]]
