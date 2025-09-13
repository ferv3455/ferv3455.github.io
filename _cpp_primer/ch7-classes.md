---
layout: post
title: Chapter 7 Classes
date: 2024/9/23
chapter: 7
toc: true
---

## Defining Abstract Data Types

- Member functions access the object through an extra implicit `const` parameter `this`. When the function is invoked, the address of the object will be passed as `this`.
- `this` is a `const` pointer, not a pointer to a `const` object. **As a result, `this` in ordinary member functions cannot be bound to a `const` object. In order to declare `this` as low-level `const`, we can declare the function as a `const` member function.**
- `*self` can be returned as a reference to the object itself.
- Auxiliary nonmember functions are declared in the same header as the class itself.
- The default constructor takes no arguments. **It can be implicitly defined by the compiler (synthesized default constructor) only if no other constructors for the class are defined**: in-class initializer (curly braces or a `=` sign) or default initialization. For members with a custom class type, they may not be default-initialized, and thus need initialized manually.
- **`= default` can ask the compiler to generate the constructor (inline if it is used inside the class body).**
- The constructor initializer is a list of member names, each of which is followed by that member’s initial value in parentheses or inside curly braces.
- When a member is omitted from the initializer list, it is implicitly initialized using the same process as the synthesized default constructor.
- **Copy, assignment, and destruction can be controlled in class. Default operations for them will be synthesized by the compiler (copying/assigning/destroying each member).**

## Access Control and Encapsulation

- A class can allow another class or function to access its non-public members by making that class or function a friend. **A declaration for that function preceded by `friend` should be included in the class (no `friend` keyword at definition).**
- **A friend declaration only specifies access.** It needs to be declared separately (usually in the same file).

## Additional Class Features

- **A class can define its own local names for types (via `typedef` or `using`). They are subject to the same access controls as any other member.** They must appear before usage.
- **Member functions defined inside the class are automatically inline.** Otherwise `inline` need to be used at either declaration or definition (preferred). Inline member functions should be defined in the same header.
- A mutable data member is never `const`, even when it is a member of a `const` object. It can be changed in `const` member functions.
- Member functions can be overloaded based on whether they are `const` are not. This is because the implicit `this` parameters have different types.
- Two classes are different even if they have the same members.
- A class must be defined before creating the objects of that type. Data members of a class can be specified to be of a class type only if the class has been defined. **Data members can only be pointers or references without the definition (their behavior is limited though).**
- `friend class ...` can be used to declare friend classes. Class friendship is not transitive.
- We can specify only member functions to be friend functions. However, that requires careful structuring of the program.
- **Classes and nonmember functions need not have been declared before they are used in a friend declaration. When a name first appears in a friend declaration, that name is implicitly assumed to be part of the surrounding scope.** Even if we define the function inside the class, we must still provide a declaration outside of the class itself to make that function visible.

```cpp
struct X {
    friend void f() { /* friend function can be defined in the class body */ }
    X() { f(); } // error: no declaration for f
    void g();
    void h();
};

void X::g() { return f(); } // error: f hasn't been declared
void f();                   // declares the function defined inside X
void X::h() { return f(); } // ok: declaration for f is now in scope
```

## Class Scope

- A class is a scope. Once the class name is seen in the member function definition, the remainder (parameter list and body) is in the scope of the class. However, the return type appears before the class name, so the class needs to be specified.
- Class definitions are processed in two phases: compiling member declarations, and compiling function bodies (after the entire class has been seen). **For the names used in declarations, they must be seen before usage.**
- A type should not be subsequently redefined in a class if the version from an outer scope has been used in the class.
- If we want the name from the outer scope, we can ask for it explicitly: `::height`.
- **Names are only resolved where they appear within a file.** Global variables used in member functions can be declared after the class definition and before the member function definition.

## Constructors Revisited

- **Members are initialized in the order in which they appear in the class definition, not the initializer list.**
- If both in-class initializers and the constructor initializers exist, the latter will be used.
- In a delegating constructor, a single entry has the name of the class itself, and its arguments match another constructor. Both constructor bodies will be executed.
- The default constructor is used in default initialization or value initialization:
	- Default initialization: non-`static` variables or arrays without initializers.
	- Value initialization: array initialization with not enough initializers, local `static` variable without initializer, or explicit value initialization `T()`.
- **Note: `T obj()` defines a function instead of a value-initialized object.**
- Constructors that can be called with a single argument defines an implicit conversion to the class type. **We can use an argument of that type where an object of the class type is expected (function calls, copy initialization, etc.).** `explicit` can be used to suppress implicit such conversions (added inside the class). **However, explicit conversions with `static_cast` is still allowed.**
- **Only one class-type conversion is allowed.** That is, only the same type of the argument as in the constructor can be used. Types that can be converted to it are not allowed.
- Aggregate class: all data members are public, no constructors, no in-class initializers, no base classes or virtual functions. **It acts like C structs and can be initialized with a braced list of member initializers (trailing members are value initialized).**
- Literal class: all data members have literal type (can be set as `constexpr`), **at least one `constexpr` constructor (empty body, every data member should be initialized with a constant expression)**, `constexpr` initializer for a data member if it has a in-class initializer, default destructor. It can be used to generate `constexpr` objects/values.

## `static` Class Members

- `static` member functions do not have a `this` pointer. As a result, non-`static` members cannot be called inside `static` member functions.
- **`static` data members must be defined and initialized outside the class body.** Like member functions, after the class name is seen, the remainder of the definition is in the scope of the class.

```cpp
// define and initialize a static class member with a static member function
double Account::interestRate = initRate();
```

- For a `static const` integral type, we can provide in-class initializers. We must do so for a `static constexpr` of literal type. **However, in order to use them outside the class, an exterior definition is required: `constexpr int T::m`. The definition must not specify an initial value (the in-class initializer will be used).**
- **Static data members can have incomplete types (can be an instance of the class itself).**
- **Static data members can be used as default arguments in member functions.**

