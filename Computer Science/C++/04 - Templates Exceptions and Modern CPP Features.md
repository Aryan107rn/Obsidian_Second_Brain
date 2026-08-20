# 04 - Templates Exceptions and Modern CPP Features

## Templates (Generic Programming)
Lets a function or class work with any data type without rewriting it per type — the compiler generates the right version at compile time.
```cpp
template <typename T>
T maxVal(T a, T b) { return (a > b) ? a : b; }

template <class T>
class Box {
    T value;
public:
    Box(T v) : value(v) {}
    T get() { return value; }
};
Box<int> b(5);
```
All STL containers (`vector<T>`, `map<K,V>`, ...) are templates — this is why they work with any type.

## Exception Handling
Lets you signal and handle errors without crashing the program or littering code with error-code checks.
```cpp
try {
    throw runtime_error("error message");
} catch (const exception &e) {
    cout << e.what();
} catch (...) {                 // catches anything not matched above
    cout << "unknown exception";
}
```

## Lambda Functions
An anonymous, inline function — very useful for one-off logic passed to STL algorithms (comparators, predicates).
```cpp
auto add = [](int a, int b) { return a + b; };
auto sq  = [](int x) -> int { return x * x; };   // explicit return type

int y = 10;
auto capByVal = [y]() { return y; };    // captures a snapshot of y (value)
auto capByRef = [&y]() { y++; };        // captures y itself, can modify it
auto capAllRef = [&]() { y++; };        // capture everything used, by reference
auto capAllVal = [=]() { return y; };   // capture everything used, by value

// Most common DSA use: custom sort comparator
sort(v.begin(), v.end(), [](int a, int b) { return a > b; }); // descending order
```

## Structured Bindings (C++17)
Unpacks a `pair`/`tuple`/struct into named variables in one line — very useful with `map` and `pair`.
```cpp
map<string,int> m;
for (auto &[key, val] : m) cout << key << " " << val;

pair<int,int> p = {1, 2};
auto [a, b] = p;   // a = 1, b = 2
```

## auto Keyword
Lets the compiler deduce the variable's type from its initializer — reduces verbosity, especially with long STL types like iterators.
```cpp
auto x = 5;                 // deduced as int
auto v = vector<int>{};     // deduced as vector<int>
for (auto it = v.begin(); it != v.end(); it++) { }  // deduces the iterator type
```

## Common Mistakes
- Forgetting explicit capture in a lambda (`[]` captures nothing — using an outer variable inside will fail to compile).
- Relying on `auto` when you actually need a specific type (e.g. `auto x = 5` gives `int`, not `long long` — can silently reintroduce overflow bugs).
- Catching exceptions too broadly (`catch (...)`) without re-throwing or logging, hiding real bugs.

## Related Concepts
[[03 - OOP in CPP]]
[[05 - STL Containers]]
