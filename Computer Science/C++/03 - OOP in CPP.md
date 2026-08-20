# 03 - OOP in CPP

## What is it?
Object-Oriented Programming organizes code around **objects** — bundles of data (attributes) and behavior (methods) — instead of a sequence of standalone functions. A `class` is the blueprint; an object is an instance of it.

## Class Basics
```cpp
class Rectangle {
private:
    double width, height;
public:
    Rectangle(double w, double h) : width(w), height(h) {}  // constructor, initializer list preferred (more efficient than assigning in the body)
    double area() const { return width * height; }           // const = doesn't modify object state
};
```
- `private` (default for `class`) hides members from outside code; `public` exposes the interface.
- `struct` is identical to `class` except its default access is `public` — DSA convention: use `struct` for simple data holders (`struct Node { int val; Node* next; };`).

## The Four Pillars

1. **Encapsulation** — bundle data + methods, hide internals via `private`/`public`, expose controlled access through getters/setters.
2. **Abstraction** — expose only essential behavior, hide implementation details (abstract classes, pure virtual functions).
3. **Inheritance** — derive a new class from an existing one to reuse code: `class Dog : public Animal { ... }`.
4. **Polymorphism** — same interface, different behavior:
   - **Compile-time** (static): function/operator overloading.
   - **Run-time** (dynamic): virtual functions / overriding.

## Full Example (all four pillars together)
```cpp
#include <iostream>
using namespace std;

class Shape {                                  // Abstraction + Encapsulation
private:
    string name;
public:
    Shape(string n) : name(n) {}
    virtual double area() const = 0;            // pure virtual -> makes Shape abstract, cannot instantiate
    virtual ~Shape() {}                         // virtual destructor — required when deleting via base pointer
    string getName() const { return name; }     // encapsulated access
};

class Circle : public Shape {                    // Inheritance
    double radius;
public:
    Circle(double r) : Shape("Circle"), radius(r) {}
    double area() const override { return 3.14159 * radius * radius; }
};

class Square : public Shape {
    double side;
public:
    Square(double s) : Shape("Square"), side(s) {}
    double area() const override { return side * side; }
};

int main() {
    Shape* shapes[] = { new Circle(5), new Square(4) };
    for (Shape* s : shapes)
        cout << s->getName() << ": " << s->area() << endl;  // Polymorphism — correct override called at runtime
    for (Shape* s : shapes) delete s;
}
```

## Function Overloading vs Overriding
| Overloading | Overriding |
|---|---|
| Same name, different parameters, same scope | Same name/signature, base class vs derived class |
| Resolved at compile-time (static polymorphism) | Resolved at run-time (dynamic polymorphism) |
| No `virtual` needed | Requires `virtual` in the base class |

## Static & Friend
```cpp
class Counter {
    static int count;                        // one copy shared across all objects, not per-instance
public:
    static int getCount() { return count; }  // static method — no 'this' pointer, callable without an object
    friend void reset(Counter&);              // friend function — external function allowed to access private members
};
int Counter::count = 0;  // static members must be defined outside the class
```

## Multiple Inheritance & the Diamond Problem
```cpp
class A { public: int x; };
class B : virtual public A {};
class C : virtual public A {};
class D : public B, public C {};  // without 'virtual', D would contain two separate copies of A (ambiguous x)
```
`virtual` inheritance ensures `D` has exactly one shared `A` subobject instead of two conflicting copies.

## Common Mistakes
- Forgetting `virtual` on a base class destructor when objects are deleted through a base pointer → the derived class's destructor never runs → memory leak. This is one of the most common interview traps.
- Confusing `class` (private by default) with `struct` (public by default).
- No `.equals()` method exists in C++ — equality is done via an overloaded `operator==`.

## Related Concepts
[[02 - Pointers References and Memory Management]]
[[04 - Templates Exceptions and Modern CPP Features]]
