# C++ Built-in Functions for DSA

## What is it?

A reference of C++ standard library functions that come up constantly while solving DSA problems but don't fit neatly under "STL containers" — bit manipulation, string handling, character classification, and numeric conversion. [[CPP Complete Revision]] and [[C++ Core Language Features]] both link here; this note fills the gap.

---

## 1. Bit Manipulation

### Why it matters for DSA
Bitwise operations run in O(1) and are the backbone of subset generation, bitmask DP, XOR tricks, and space-efficient state representation.

| Operator | Meaning | Example |
|---|---|---|
| `&` | AND — 1 only if both bits are 1 | `5 & 3` → `1` (`101 & 011 = 001`) |
| `\|` | OR — 1 if either bit is 1 | `5 \| 3` → `7` |
| `^` | XOR — 1 if bits differ | `5 ^ 3` → `6` |
| `~` | NOT — flips every bit | `~5` → `-6` (two's complement) |
| `<<` | left shift — multiply by 2 per shift | `1 << 3` → `8` |
| `>>` | right shift — divide by 2 per shift | `8 >> 2` → `2` |

### Common bit tricks
```cpp
bool isSet   = (n >> i) & 1;      // check if bit i is set
int  setBit  = n | (1 << i);      // set bit i
int  clrBit  = n & ~(1 << i);     // clear bit i
int  togBit  = n ^ (1 << i);      // toggle bit i
bool isEven  = !(n & 1);          // faster than n % 2 == 0
int  lastSetBit = n & (-n);       // isolates the lowest set bit (e.g. 12 (1100) -> 4 (0100))
n &= (n - 1);                     // clears the lowest set bit — used to count set bits in a loop
```
**Why `n & (-n)` isolates the lowest set bit**: in two's complement, `-n` is `~n + 1`. Flipping all bits and adding 1 makes every bit above the lowest set bit of `n` cancel out under AND, leaving only that one bit.

### Built-in bit-counting functions (GCC/Clang)
```cpp
__builtin_popcount(x);     // number of set bits in a 32-bit int — O(1)-ish, hardware-backed
__builtin_popcountll(x);   // same, for long long
__builtin_clz(x);          // count leading zeros (undefined if x == 0)
__builtin_ctz(x);          // count trailing zeros (undefined if x == 0)
```
**When to use:** subset enumeration (`for (int mask = 0; mask < (1<<n); mask++)`), bitmask DP (Travelling Salesman-style states), checking power of 2 (`n && !(n & (n-1))`), XOR-based problems (single number, missing number).

### `bitset<N>`
```cpp
bitset<32> b(13);          // binary representation of 13, fixed width 32
b.count();                 // number of set bits
b.set(2); b.reset(2);      // set/clear bit 2
b.to_string();             // "00000000000000000000000000001101"
```
**When to use:** fixed-size bit arrays (sieve of Eratosthenes flags, visited sets over a bounded range) — more memory-efficient than `vector<bool>` and gives O(1) popcount via `.count()`.

---

## 2. String Functions (`<string>`)

Strings in C++ are mutable, dynamically-sized, and index like arrays (`s[i]`).

```cpp
string s = "hello world";
s.length(); s.size();          // both give length, interchangeable
s.substr(6);                   // "world" — from index 6 to end
s.substr(0, 5);                // "hello" — 5 chars starting at index 0
s.find("world");                // 6 (index of first match) or string::npos if not found
s.find("xyz") == string::npos;  // correct way to check "not found"
s += "!";                       // append (also s.push_back('!') for a single char)
s.insert(5, "XYZ");             // insert "XYZ" at index 5
s.erase(0, 3);                  // remove 3 chars starting at index 0
reverse(s.begin(), s.end());    // in-place reverse (from <algorithm>)
sort(s.begin(), s.end());       // sort characters (e.g. for anagram checks)
s.compare(other);               // 0 if equal, <0 if s < other, >0 if s > other (rarely needed — use ==, <)
```

### String ↔ number conversion
```cpp
int n = stoi("123");            // string to int
long long n2 = stoll("123456789012"); // string to long long
double d = stod("3.14");        // string to double
string s = to_string(123);      // int (or any number) to string
```
**Common mistake:** `stoi` throws `std::invalid_argument` if the string has no valid leading number, and `std::out_of_range` if the number doesn't fit in an `int` — wrap in `try/catch` or validate input first when parsing untrusted strings.

### Building strings efficiently
```cpp
stringstream ss;
ss << "value: " << 42;
string result = ss.str();       // "value: 42"

// splitting a string by delimiter using stringstream:
stringstream ss2("a,b,c");
string token;
vector<string> parts;
while (getline(ss2, token, ',')) parts.push_back(token);
```
**When to use:** `stringstream` is the standard idiom for splitting/parsing since C++ has no built-in `.split()` like Python.

---

## 3. Character Functions (`<cctype>`)

Operate on a single `char`; essential for string/parsing problems.

```cpp
isalpha(c);   isdigit(c);   isalnum(c);   isspace(c);
isupper(c);   islower(c);
toupper(c);   tolower(c);   // return the converted char, don't modify in place
```
**Common mistake:** these functions expect an `int` and technically have undefined behavior for negative `char` values (possible on some platforms with extended/non-ASCII chars) — cast to `unsigned char` first if working with arbitrary byte data: `isalpha((unsigned char)c)`.

### Char ↔ int arithmetic
```cpp
char c = 'c';
int digit = c - '0';        // '7' - '0' == 7 — converts a digit char to its numeric value
char next = 'a' + 2;        // 'c' — offset within the alphabet, common in anagram/hashing tricks
int idx = c - 'a';          // 0-25 index for lowercase letters — classic array-based frequency counting
```
**Why this works:** characters are stored as their ASCII integer codes, and C++ lets you do arithmetic directly on `char` types since they're just small integers under the hood.

---

## 4. Math Functions (`<cmath>`)

```cpp
pow(base, exp);      // returns double — for integer power, prefer manual fast exponentiation to avoid float errors
sqrt(x);             // square root, returns double
abs(x);              // works for int (also in <cstdlib>); use fabs(x) for double precision explicitly
ceil(x);  floor(x);  // round up / down, return double
```
**Common mistake:** `pow(2, 10)` returns a `double` (`1024.0`), which can have floating-point rounding error for large integer powers — for competitive programming, write an integer fast-power function instead:
```cpp
long long power(long long base, long long exp, long long mod) {
    long long res = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) res = res * base % mod;
        base = base * base % mod;
        exp >>= 1;
    }
    return res;
}
```

---

## Related concepts
[[CPP Complete Revision]]
[[C++ Core Language Features]]
[[C++ Fundamentals]]
