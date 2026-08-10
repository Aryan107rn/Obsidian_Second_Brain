# OOP in C++

# OOP in C++

## Why OOP exists

Procedural code (functions and data floating separately) gets hard to reason about as programs grow — data can be modified from anywhere with no guarantees about its validity. OOP's answer: bundle data and the functions that operate on it into a single unit (a **class**), and control who can touch what. This also models real-world entities more naturally: a `Car` has both data (speed, fuel) and behavior (accelerate, brake) bound together.

## Classes vs objects

A **class** is a blueprint; an **object** is a concrete instance built from it.

```cpp
class Rectangle {
private:
    double width, height;              // data (state)
public:
    Rectangle(double w, double h) : width(w), height(h) {}  // constructor, initializer list preferred
    double area() const { return width * height; }           // behavior
};

Rectangle r(3, 4);   // r is an object - an actual instance in memory
```
- `private` (default) hides members; `public` exposes interface.
- `struct` = `class` but default access is `public` - used by convention for simple data holders.
- `const` after a method signature means the method doesn't modify the object's state.

## Constructors and destructors

A constructor runs automatically when an object is created; a destructor runs automatically when it's destroyed (scope exit or `delete`).

```cpp
Rectangle(double w, double h) : width(w), height(h) {}   // initializer list - PREFERRED
// vs: Rectangle(double w, double h) { width = w; height = h; }  // works, but default-inits then reassigns - less efficient
```
Prefer the initializer list over assigning in the constructor body - it directly initializes members instead of default-constructing then reassigning, and is required for `const`/reference members.

## The Four Pillars — why each exists, not just what it's called

1. **Encapsulation** - Problem: if any code can directly modify an object's internals, the object can't guarantee it stays valid. Solution: hide data behind `private`, expose a controlled `public` interface.

2. **Abstraction** - Problem: callers shouldn't need to know *how* something works, only *what* it does. Solution: pure virtual functions (`= 0`) define a contract with no implementation - a class containing one becomes abstract and can't be instantiated directly.

3. **Inheritance** - Problem: avoid rewriting shared behavior for related types. Solution: `class Derived : public Base` inherits the base's public members and can add/override its own.

4. **Polymorphism** - Problem: you want to call `shape->area()` on a `Shape*` and have the CORRECT version run (Circle's or Square's) without knowing the concrete type at compile time.

### How polymorphism actually works (vtable mechanism)
When a base class has a `virtual` function, the compiler gives every object of that class a hidden pointer (**vptr**) to a **vtable** - a lookup table of function addresses specific to its actual (derived) type. Calling a virtual function through a base pointer follows the vptr into the vtable and jumps to the derived class's version - this is **dynamic dispatch**, resolved at runtime rather than compile time.

- Overloading = same name, different parameters, resolved at COMPILE time (compile-time/static polymorphism).
- Overriding = same signature, base vs derived, requires `virtual`, resolved at RUN time (runtime/dynamic polymorphism).

**Why this mechanism means the base destructor must be `virtual`**: `delete shapePtr` looks up the vtable to decide which destructor to call. Without `virtual`, deleting through a base pointer only calls the base destructor - the derived part of the object never gets cleaned up. This is one of the most common real-world C++ bugs.

## Example (all four pillars together)
```cpp
class Shape {
private:
    string name;
public:
    Shape(string n) : name(n) {}
    virtual double area() const = 0;   // pure virtual -> abstraction
    virtual ~Shape() {}                // virtual destructor -> safe polymorphic deletion
    string getName() const { return name; }  // encapsulation
};

class Circle : public Shape {           // inheritance
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
        cout << s->getName() << ": " << s->area() << endl;  // polymorphism
    for (Shape* s : shapes) delete s;
}
```

## Common Mistakes
- Forgetting `virtual` on base class destructor when using inheritance -> undefined behavior on `delete` via base pointer (derived part never cleaned up).
- **Object slicing**: assigning a derived object to a base-type variable (not pointer/reference) copies only the base part, silently discarding derived data:
```cpp
Circle c(5);
Shape s = c;   // SLICED - s is just a Shape, radius is gone
```
- Calling virtual functions from a constructor - the vtable isn't fully set up for derived classes yet during construction, so this calls the base version even if an override was intended. Avoid it.

## Related Concepts
- [[ES6 Classes and OOP]] — JS equivalent of class/OOP concepts
- [[C++ Core Language Features]] — pointers/references, memory management used alongside OOP
- [[C++ Fundamentals]]
