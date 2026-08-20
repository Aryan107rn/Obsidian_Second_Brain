# 02 - Pointers References and Memory Management

## Pass by Value / Reference / Pointer
```cpp
void byValue(int x) { x++; }              // copy — no effect outside
void byRef(int &x) { x++; }               // modifies original
void byPtr(int *x) { (*x)++; }            // modifies original via address
void byConstRef(const vector<int> &v);    // avoid copy + prevent modification — use for large objects
```
**Interview rule:** always pass large objects (`vector`, `string`, `struct`, `map`) by `const&` unless you need to mutate them. Passing by value on every call silently causes O(n) copy overhead — a common cause of TLE (time limit exceeded).

## Pointers & References Quick Facts
```cpp
int x = 5;
int *p = &x;      // p holds the address of x
int &r = x;       // r is an alias of x — must be initialized, cannot be reseated to refer elsewhere
int **pp = &p;     // pointer to pointer
nullptr            // modern null pointer literal — prefer over NULL/0
```
Key difference: a pointer can be reassigned or set to null; a reference is permanently bound to one variable at creation.

## const Correctness
```cpp
const int x = 5;         // x cannot change
int* const p = &y;       // pointer itself is const; *p can still vary
const int* p2 = &y;      // pointer to const int — can't modify *p2 through p2
void f() const;          // const member function — can't modify the object's members
```

## Stack vs Heap
- **Stack**: automatic allocation, very fast, limited size, freed automatically when scope ends.
- **Heap**: allocated with `new`, freed manually with `delete` (or managed by a smart pointer) — larger capacity, slower, persists until explicitly freed.

```cpp
int* p = new int(5);      // heap allocation
delete p;                 // must free manually or it leaks
int* arr = new int[10];
delete[] arr;              // array delete — must match new[] with delete[]
```

## Smart Pointers (prefer over raw new/delete)
```cpp
#include <memory>
unique_ptr<int> up = make_unique<int>(5);  // sole ownership — cannot be copied, only moved
shared_ptr<int> sp = make_shared<int>(5);  // reference-counted — freed when last owner goes out of scope
weak_ptr<int> wp = sp;                     // non-owning reference to a shared_ptr — avoids reference cycles
```
**When to use:** default to `unique_ptr` unless multiple owners genuinely need to share the resource (`shared_ptr`). `weak_ptr` breaks cycles that would otherwise leak memory (e.g. two objects holding `shared_ptr`s to each other).

## Rule of 3 / 5 / 0
- **Rule of 3:** if a class defines a destructor, copy constructor, or copy assignment operator, it likely needs all three (the compiler-generated shallow versions are usually wrong once one is customized).
- **Rule of 5** (C++11): adds move constructor and move assignment operator for efficient transfer of resources.
- **Rule of 0:** best practice — avoid needing any of the above by using smart pointers / STL containers as members instead of raw owning pointers; let the compiler generate everything.

## Common Mistakes
- **Dangling pointer**: pointer still refers to memory that has been freed — using it is undefined behavior.
- **Memory leak**: heap memory allocated with `new` but never `delete`d.
- **Shallow vs deep copy**: the compiler's default copy constructor copies pointer *values*, not what they point to — two objects end up sharing the same underlying memory. Write a custom copy constructor when a class owns a raw pointer.
- `malloc`/`free` vs `new`/`delete`: `malloc` doesn't call constructors, `new` does (and is type-safe) — never mix them.
- Forgetting to match `new[]` with `delete[]` (using plain `delete` on an array is undefined behavior).

## Related Concepts
[[01 - Data Types and Fundamentals]]
[[03 - OOP in CPP]]
