# C++ Fundamentals

## What is it?

C++ is a **compiled, statically-typed** programming language. "Compiled" means your source code is fully translated into machine code *before* the program runs (unlike Python or JavaScript, which are interpreted line-by-line as they run). "Statically-typed" means every variable's type is fixed at compile time and checked before the program ever runs.

This combination is *why* C++ is fast and widely used for performance-critical software (game engines, operating systems, trading systems): the compiler does all the type-checking and translation work up front, so at runtime the CPU just executes raw machine instructions with no interpreter overhead.

## The compilation model

Unlike Python, you can't just "run" a `.cpp` file directly — it has to be **built** first, through four stages:

1. **Preprocessor** — handles anything starting with `#`, like `#include` (pastes in header file contents) and `#define` (macro substitution). Produces expanded source code.
2. **Compiler** — translates the expanded C++ code into assembly/machine instructions specific to your CPU architecture. Catches type errors here.
3. **Assembler** — converts assembly into an object file (`.o` / `.obj`) — actual binary machine code, but not yet a runnable program.
4. **Linker** — combines your object file(s) with any libraries you use (like the standard library) into a single executable (`a.out` on Linux/Mac, `.exe` on Windows).

`source.cpp -> preprocessor -> compiler -> assembler (.o file) -> linker -> executable`

**Why this matters practically:** compile errors (syntax, type mismatches) happen at step 2. Linker errors (e.g. "undefined reference to `foo()`") happen at step 4 — meaning your code is syntactically fine, but the compiler can't find the actual implementation of something you declared. Beginners often confuse these two error categories.

## Variables and data types

A variable is a named, typed slot of memory. C++ requires you to declare the type upfront so the compiler knows exactly how many bytes to reserve and how to interpret the bits stored there.

| Type | Typical size | Holds | Example |
|---|---|---|---|
| `int` | 4 bytes | Whole numbers | `int age = 25;` |
| `float` | 4 bytes | Decimal numbers (~7 digit precision) | `float pi = 3.14f;` |
| `double` | 8 bytes | Decimal numbers (~15 digit precision) | `double pi = 3.14159265;` |
| `char` | 1 byte | A single character | `char grade = 'A';` |
| `bool` | 1 byte | `true` / `false` | `bool isValid = true;` |
| `std::string` | variable | Text — **not a built-in type**, requires `#include <string>` | `std::string name = "Alex";` |

Sizes are platform-dependent in principle, but the values above hold on virtually all modern systems. Use `sizeof(type)` to check on your machine.

### Integer overflow (important edge case)

A 4-byte `int` can only represent roughly -2.1 billion to +2.1 billion. Exceeding that range doesn't throw an error — it silently **wraps around**:

```cpp
int x = 2147483647;   // INT_MAX
x = x + 1;             // becomes -2147483648, no warning, no crash
```

This is a classic source of silent bugs, especially in loops with large bounds or when multiplying sizes. When you need larger ranges, use `long long` (typically 8 bytes).

### `const`

Declaring a variable `const` prevents it from being reassigned after initialization. Use it for any value that shouldn't change — it documents intent to readers and lets the compiler catch accidental modification as a compile error rather than a runtime bug.

```cpp
const double PI = 3.14159;
```

## Operators

- **Arithmetic:** `+ - * / %`
  - `%` (modulo, remainder) only works on integer types.
  - **Integer division truncates**: `7 / 2` evaluates to `3`, not `3.5`, because both operands are `int`. To get a decimal result, at least one operand must be a floating-point type: `7.0 / 2` gives `3.5`. This is one of the most common beginner mistakes in C++.
- **Relational:** `== != < > <= >=` — return `bool`.
- **Logical:** `&& || !` — combine boolean expressions, with short-circuit evaluation (if the left side of `&&` is false, the right side isn't even evaluated).
- **Assignment:** `= += -= *= /=` — shorthand update-and-assign operators.

**Common mistake:** writing `if (x = 5)` instead of `if (x == 5)`. This *compiles* — it assigns `5` to `x`, and the assignment expression itself evaluates to `5`, which is truthy — but it silently does the wrong thing (always enters the `if` branch and overwrites `x`). Many compilers warn about this; don't ignore that warning.

## Control flow

Controls **which** statements execute and **how many times**, based on conditions.

```cpp
if (score >= 90) {
    grade = 'A';
} else if (score >= 80) {
    grade = 'B';
} else {
    grade = 'F';
}
```

**Loops** — three forms, each suited to a different situation:

```cpp
for (int i = 0; i < 5; i++) {
    // Use when you know the number of iterations in advance
    // (e.g. iterating over an array/vector by index)
}

while (condition) {
    // condition checked BEFORE each iteration
    // use when the number of iterations is unknown ahead of time
    // (e.g. reading input until a sentinel value or EOF)
}

do {
    // condition checked AFTER each iteration - body always runs at least once
} while (condition);
```

## Functions

A function is a named, reusable block of code. Functions exist to avoid repeating logic and to break a program into understandable, testable pieces.

```cpp
int add(int a, int b) {   // return type | name | parameters
    return a + b;
}
```

### Pass by value vs pass by reference (critical concept)

By default, C++ **copies** arguments into a function — this is called *pass by value*. Modifying the parameter inside the function does **not** affect the caller's original variable.

```cpp
void tryToDouble(int x) {
    x = x * 2;   // only changes the local copy
}

void doubleIt(int &x) {   // & marks this as a reference parameter
    x = x * 2;   // modifies the CALLER's actual variable
}

int n = 5;
tryToDouble(n);   // n is still 5
doubleIt(n);      // n is now 10
```

**Why this matters for performance, not just correctness:** passing a large object like `std::string` or `std::vector` by value copies the *entire* object on every single call. Passing by `const &` (a "const reference") avoids that copy while still preventing the function from modifying the original — this is the standard idiom for function parameters that are read but not changed:

```cpp
void printName(const std::string &name) {   // no copy made, and name can't be modified
    std::cout << name;
}
```

## Arrays and strings

```cpp
int nums[5] = {1, 2, 3, 4, 5};   // fixed-size C-style array - size is baked in, can't grow/shrink
std::string name = "Alex";        // dynamic, resizable, has built-in methods (+=, .length(), etc.)
name += " Smith";                 // concatenation works naturally, unlike C-style char arrays
```

**Edge case - no bounds checking:** a C-style array doesn't know its own size at runtime, and accessing an out-of-range index doesn't throw an error:

```cpp
int nums[5] = {1,2,3,4,5};
std::cout << nums[10];   // compiles and runs - reads garbage/undefined memory, not a crash you can rely on
```

This is a major source of real-world bugs and security vulnerabilities (buffer overflows). Modern C++ generally prefers `std::vector` (dynamic array, part of the STL) and `std::array` (fixed-size but bounds-checkable via `.at()`) over raw C-style arrays for this reason.

## Common mistakes summary

- Confusing `=` and `==` in conditions.
- Expecting `7 / 2` to give `3.5` (integer division truncates).
- Forgetting that C-style arrays have no bounds checking.
- Passing large objects by value unnecessarily (performance cost) instead of `const &`.
- Ignoring integer overflow in loops or size calculations.

## Related concepts

- [[C++ Core Language Features]]
- [[OOP in CPP]]
- [[CPP Complete Revision]]
