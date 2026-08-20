# 07 - C++ Built-in Functions for DSA

## Bit Manipulation
Extremely common in DSA — subset generation, bitmask DP, XOR tricks.
```cpp
x << n, x >> n           // left/right shift (× or ÷ by 2^n)
x & y, x | y, x ^ y, ~x   // AND, OR, XOR, NOT

__builtin_popcount(x)     // count set bits (int); use popcountll for long long
__builtin_clz(x)          // count leading zeros
__builtin_ctz(x)          // count trailing zeros

x & (x-1)    // remove the lowest set bit
x & (-x)     // isolate the lowest set bit
x | (1<<i)   // set bit i
x & ~(1<<i)  // clear bit i
x ^ (1<<i)   // toggle bit i
(x>>i) & 1   // check bit i
```

## `<cmath>` — Math
```cpp
pow(x, y); sqrt(x); cbrt(x);
abs(x); fabs(x);              // abs() for int, fabs() for float/double
ceil(x); floor(x); round(x);
log(x); log2(x); log10(x);
gcd(a, b); lcm(a, b);          // <numeric>, C++17
```

## `<cctype>` — Character Checks
```cpp
isdigit(c); isalpha(c); isalnum(c); isspace(c); isupper(c); islower(c);
toupper(c); tolower(c);
c - '0';    // char digit -> int
'0' + d;    // int digit -> char
```

## String Conversions
```cpp
to_string(123);            // number -> string
stoi("123"); stol(); stoll();
stod("1.23"); stof("1.23");
```

## Common Mistakes
- Using the `int` popcount builtin on a `long long` value — use `__builtin_popcountll` instead.
- `stoi`/`stod` throw an exception on invalid input — wrap in try/catch if the input isn't guaranteed valid ([[04 - Templates Exceptions and Modern CPP Features]]).
- Forgetting `abs()` is for `int` and `fabs()` is for floating point — mixing them can silently truncate.

## Related Concepts
[[01 - Data Types and Fundamentals]]
[[06 - STL Algorithms and Iterators]]
[[08 - DSA Patterns with STL]]
