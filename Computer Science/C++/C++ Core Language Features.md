# C++ Core Language Features

## Pointers & References

### Why they exist
Every variable lives at a memory address. Normally you just use the variable's name and never think about the address — but sometimes you need the address itself: to avoid copying large data into functions, to build data structures where one node needs to "point to" another (linked lists, trees), or to work with memory that must outlive the function that created it.

### Pointers
A pointer is a variable whose value IS a memory address — like a sticky note with another variable's "house address" written on it.

```cpp
int x = 5;
int *p = &x;   // & = "address-of": p now holds the address of x
*p = 10;       // * here = "dereference": follow the address, modify what's there -> x becomes 10
int **pp = &p; // pointer to a pointer (holds the address of p itself)
nullptr        // modern null pointer literal - prefer over NULL/0, it's type-safe
```

### References
A reference is a second NAME for an existing variable, not a separate value holding an address:

```cpp
int x = 5;
int &r = x;   // r IS x, just under another name - no dereference syntax needed
r = 10;       // x is now 10
```

### Pointer vs reference - when to use which

| Pointer | Reference |
|---|---|
| Can be reassigned to point elsewhere | Bound permanently to one variable at creation |
| Can be `nullptr` (point to nothing) | Must always refer to something valid - can't be null |
| Needs `*` to access the value | Used exactly like a normal variable |
| Good for: optional values, dynamic structures (linked lists), reassignable ownership | Good for: function parameters (simpler, safer, avoids copies) |

**Rule of thumb**: prefer references for function parameters. Use pointers only when you need "can be null" or "can be reassigned" behavior.

Pass large objects (`vector`, `string`, `struct`) by `const&` to avoid copies unless the function needs to mutate the original:
```cpp
void printName(const std::string &name);  // no copy made, and can't modify name
```

### Common mistakes
- **Dangling reference/pointer to a local variable** - returning a reference to a function's local variable is undefined behavior, since that local is destroyed when the function returns:
```cpp
int& bad() {
    int x = 5;
    return x;   // x dies when bad() returns - caller gets garbage
}
```
- Forgetting that a pointer can be `nullptr` and dereferencing it without checking - crashes immediately.
- Confusing `int* const p` (const pointer, can't repoint, can modify target) with `const int* p` (pointer to const, can repoint, can't modify target) - see const Correctness below.

## const Correctness

### Why it matters
`const` lets you make a *promise*, checked by the compiler, that something won't change. This documents intent and catches accidental mutation as a compile error instead of a runtime bug.

```cpp
const int x = 5;        // x cannot change
int* const p = &y;      // pointer itself is const (can't repoint), *p CAN vary
const int* p2 = &y;     // pointer to const int - p2 CAN repoint, but can't modify *p2
const int* const p3 = &y; // neither the pointer nor the target can change
void f() const;         // member function promises not to modify the object's members
```

**How to read `const` placement**: read right-to-left from the variable name. `const int* p2` = "p2 is a pointer to a const int." `int* const p` = "p is a const pointer to an int."

## Function Overloading vs Overriding
| Overloading | Overriding |
|---|---|
| Same name, different params, same scope | Same signature, base vs derived class |
| Compile-time (static) polymorphism | Run-time (dynamic) polymorphism |
| No `virtual` needed | Requires `virtual` in base |

## Templates (Generic Programming)

### Why they exist
Without templates, you'd need a separate `maxVal` function for `int`, `double`, `string`, etc. Templates let you write the logic ONCE, generically over a type parameter `T`, and the compiler generates a specific version for each concrete type actually used in your code (this happens at **compile time** — called template instantiation — unlike Java/Python generics which are resolved at runtime).

```cpp
template <typename T>
T maxVal(T a, T b) { return (a > b) ? a : b; }
maxVal(3, 5);       // compiler generates a version for int
maxVal(3.1, 5.2);   // compiler generates a SEPARATE version for double

template <class T>
class Box {
    T value;
public:
    Box(T v) : value(v) {}
    T get() { return value; }
};
Box<int> b(5);
```

**Trade-off to know**: because a separate version is compiled per type used, heavy template use can increase binary size ("code bloat") and slow compilation — the STL itself is entirely templates, which is why C++ compile times are notoriously long on large projects.

## Memory Management

### Stack vs heap - why two kinds of memory exist
- **Stack**: memory automatically managed by the compiler. Entering a function pushes its local variables onto the stack; returning pops (frees) them automatically. Fast, but limited size, and everything on it dies when its scope ends.
- **Heap**: memory you control manually. It survives beyond the function that created it - essential when data must outlive the function, or when size isn't known until runtime.

```cpp
int* p = new int(5);   // allocate an int on the heap; p (on the stack) holds its address
delete p;               // must free manually - the compiler will NOT do this for you

int* arr = new int[10];
delete[] arr;           // array form needs delete[], not delete
```

### The three classic heap bugs
- **Memory leak** - forgetting `delete`: the memory is never freed, and stays reserved for the life of the program.
- **Dangling pointer** - using a pointer after its target was `delete`d. Dereferencing it is undefined behavior: might crash, might silently corrupt unrelated data - unpredictable and hard to debug.
- **Double free** - calling `delete` on the same pointer twice, also undefined behavior.

### Smart pointers - the modern fix
`#include <memory>` gives you pointer-like objects that automatically call `delete` when they go out of scope, applying the stack's automatic-cleanup idea to heap memory:

```cpp
unique_ptr<int> up = make_unique<int>(5);
// sole ownership - cannot be copied, only moved. Freed automatically when up goes out of scope.

shared_ptr<int> sp = make_shared<int>(5);
// reference-counted - multiple shared_ptrs can own the same object;
// freed only when the LAST owner goes out of scope.

weak_ptr<int> wp = sp;
// non-owning observer of a shared_ptr - doesn't keep the object alive,
// used to break reference cycles (two shared_ptrs pointing at each other would otherwise leak forever)
```

**Default rule**: use `unique_ptr` unless you specifically need shared ownership - it has zero overhead over a raw pointer and makes ownership unambiguous. Prefer smart pointers over raw `new`/`delete` in new code - they avoid leaks and dangling pointers by construction.

## Exception Handling

### Why it exists
Exceptions separate *error-handling* code from *normal-logic* code, instead of every function returning an error code that callers must remember to check. When you `throw`, C++ immediately begins **stack unwinding**: it walks back up the call stack, running the destructor of every local object it passes, until it finds a matching `catch`. This is why exceptions pair naturally with RAII (Resource Acquisition Is Initialization) — objects that own resources clean themselves up automatically even when an error interrupts normal flow.

```cpp
try {
    throw runtime_error("error message");
} catch (const exception &e) {   // catch by const& - avoids slicing derived exception types
    cout << e.what();
} catch (...) {                  // catches literally anything - use as a last resort
    cout << "unknown exception";
}
```

**When to use exceptions vs error codes**: exceptions for genuinely *exceptional*, rare failure conditions (file not found, invalid input) where handling it inline everywhere would clutter the logic. Avoid using exceptions for expected, frequent control flow (e.g. "end of loop") — they carry performance overhead when actually thrown.

## Lambda Functions

### Why they exist
STL algorithms like `sort` and `find_if` take a function as an argument. Writing a separate named function for every one-off comparator is verbose. A lambda is an anonymous, inline function you can define exactly where you need it.

```cpp
auto add = [](int a, int b) { return a + b; };
auto sq = [](int x) -> int { return x * x; };   // -> int is an explicit return type (optional if deducible)

int y = 10;
auto capByVal = [y]() { return y; };   // captures a SNAPSHOT of y at creation time - safe, but won't see later changes
auto capByRef = [&y]() { y++; };       // captures a LIVE reference to y - can modify it, but dangerous if lambda outlives y
auto capAllRef = [&]() { y++; };       // capture everything used, by reference
auto capAllVal = [=]() { return y; };  // capture everything used, by value

sort(v.begin(), v.end(), [](int a, int b) { return a > b; }); // descending, common in DSA
```

**Common mistake**: capturing by reference (`[&]`) and then using the lambda after the captured variable has gone out of scope — this produces a dangling reference, same danger as returning a reference to a local variable.

## Structured Bindings (C++17)

### Why they exist
Before C++17, unpacking a `pair` or `tuple` meant repeatedly writing `.first`/`.second` or `get<0>()`/`get<1>()`, which is noisy and not self-documenting. Structured bindings let you destructure directly into named variables.

```cpp
map<string,int> m;
for (auto &[key, val] : m) cout << key << " " << val;   // reads far better than it->first, it->second
pair<int,int> p = {1, 2};
auto [a, b] = p;
```

## auto Keyword

### Why it exists
Reduces verbosity, especially for long iterator or template types (`vector<pair<int,int>>::iterator` vs just `auto`), and keeps the declared type in sync automatically if the initializer's type changes.

```cpp
auto x = 5;                 // int
auto v = vector<int>{};     // vector<int>
for (auto it = v.begin(); it != v.end(); it++) // deduces iterator type
```

**Common mistake**: `auto` deduces the VALUE type by default, so `auto x = bigObject;` still makes a full copy. Use `auto&` (or `const auto&`) to bind by reference and avoid the copy — the same pass-by-value vs pass-by-reference trade-off that applies to function parameters applies here too.

## Rule of 3 / 5 / 0

### Why it exists
The compiler auto-generates a copy constructor, copy assignment operator, and destructor for you — but its auto-generated versions do a **shallow copy** (copy the raw member values, including pointers, without copying what they point to). That's wrong for any class that owns a resource via a raw pointer:

```cpp
class Buffer {
    int* data;
public:
    Buffer(int size) { data = new int[size]; }
    ~Buffer() { delete[] data; }
    // no custom copy constructor/assignment defined!
};

Buffer a(10);
Buffer b = a;   // shallow copy - b.data and a.data point to the SAME memory
// when a and b are both destroyed, delete[] runs TWICE on the same pointer -> double free, undefined behavior
```

- **Rule of 3**: if you define a destructor, copy constructor, or copy assignment operator, you almost certainly need to define all three - because needing custom cleanup logic (destructor) usually means the default shallow-copy versions are also wrong.
- **Rule of 5** (C++11): adds move constructor and move assignment operator - lets resources be *transferred* instead of copied when the source is a temporary, which is far cheaper.
- **Rule of 0**: the best fix is often to avoid the problem entirely - use `unique_ptr`/`vector`/other RAII-managing members instead of raw pointers, so the compiler-generated special member functions are already correct and you don't need to write any of the above by hand.

## Static & Friend

### Why `static` members exist
A normal member variable exists separately per object. Sometimes you want data shared across ALL instances of a class (e.g. a count of how many objects exist) - that's what `static` members are for.

```cpp
class Counter {
    static int count;                      // shared across all objects - one copy total, not per-instance
public:
    static int getCount() { return count; } // static methods have no 'this' pointer - can't access non-static members
    friend void reset(Counter&);            // friend can access private members despite being outside the class
};
int Counter::count = 0; // must be defined outside the class (declaration in class, definition outside)
```

**`friend`** intentionally breaks encapsulation for a specific external function/class - use sparingly, since it couples the friend function tightly to the class's internals and undermines the whole point of `private`.

## Multiple Inheritance & Diamond Problem

### The problem
If `B` and `C` both inherit from `A`, and `D` inherits from both `B` and `C`, then without precaution `D` ends up containing TWO separate copies of `A`'s data - one via `B`, one via `C`. Accessing `d.x` (a member of `A`) becomes ambiguous: the compiler can't tell which copy you mean.

```cpp
class A { public: int x; };
class B : virtual public A {};   // 'virtual' here means: share ONE A subobject across the whole hierarchy
class C : virtual public A {};
class D : public B, public C {}; // virtual inheritance avoids duplicate A subobject - d.x is now unambiguous
```

## Common Mistakes
- Confusing `struct`/`class` default access (struct = public, class = private).
- No `.equals()` in C++ - use overloaded `operator==`.
- Default copy constructor does a **shallow copy** - pointer members share memory; write a custom copy constructor for deep copy (see Rule of 3/5/0 above for why this causes a double free).
- `malloc`/`free` don't call constructors/destructors - use `new`/`delete` (type-safe, calls ctor/dtor).
- Forgetting `virtual` destructor in a base class -> deleting via base pointer skips derived destructor -> memory/resource leak.
- Mid-point overflow: `(l+r)/2` can overflow near `INT_MAX` - use `l + (r-l)/2`.
- Capturing a lambda by reference (`[&]`) and using it after the captured variable goes out of scope - dangling reference.
- `auto x = obj;` silently copies even large objects - use `auto&`/`const auto&` when you don't want a copy.

## Related Concepts
[[OOP in CPP]]
[[C++ Fundamentals]]
[[CPP Complete Revision]]
[[C++ Built-in Functions for DSA]]
