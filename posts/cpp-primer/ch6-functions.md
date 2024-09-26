---
layout: post
title: C++ Primer (5th Edition) Chapter 6 Functions
date: 2024/9/21
---

# Chapter 6 Functions

## Function Basics

- Call operator `()`: takes an expression that is a function/pointer to a function, and a comma-separated list of arguments is inside the parentheses.
- **The return type of a function may not be an array type or a function type, but a pointer to either of them is acceptable.**
- Automatic objects exist only while a block is executing. Parameters are automatic objects initialized by the arguments passed to the function.
- **Each local `static` object is initialized before the first time execution pass through the object's definition. Local `static`s of built-in type are value-initialized to zero.**
- Parameter names are often omitted in a declaration, but they can assist in readability.

## Argument Passing

- Argument passing works exactly the same as variable initialization.
- When we copy an argument (pass by value) to initialize a parameter, top-levels `const`s are ignored. So a top-level `const` parameters can accept either a `const` or a non-`const` object. **Because of this feature, two functions with parameters of type `const int` and `int` cannot be overloaded (accept identical arguments).**
- Arrays will be passed as a pointer (no matter the declaration). The size of the array is irrelevant and unknown to the function. **When passing a multi-dimensional array, the first dimension will be ignored, and other dimensions indicate the type of the pointer.**
- **Array parameters can also be defined as a reference: `int (&arr)[10]`. The dimension is part of the type, and it can only accept arrays of the given size.**
- Taking a varying number of arguments: `initializer_list`, `...`, or variadic template.
	- `initializer_list` behaves as a `vector` of `const` values. **The type needs to be specified beforehand.** When the function is called, the sequence of values needs to be wrapped in curly braces.
	- **`initializer_list` can be automatically unpacked as parameters for another function.**
	- **Ellipsis parameters are used to be compatible for C.** It appears as the last element in a parameter list. Arguments can be extracted with C library `varargs`.

## Return Types and the `return` Statement

- **The return value is used to initialize a temporary at the call site (possible copy), and that temporary is the result of the function call (another copy if assigned to a variable).**
- Functions can return a braced list of values, which is used to initialize the temporary that represents the function's return.
- The main function is allowed to terminate without a return (implicitly return 0). `cstdlib` defines two preprocessor variables for program status.
- Functions cannot return arrays, but they can return pointers to arrays: **`int (*func(int i))[10]` declares a function returning a pointer to an `int` array of size 10.**
- To avoid complicated return types, trailing return types can be used: `auto func(int i) -> int(*)[10]` declares the same function.
- `decltype` can also used to avoid complicated return values.

## Overloaded Functions

- Using type alias will not make types different for overloading.
- Top-level `const` parameters are indistinguishable from ones without a top-level `const`.
- **Low-level `const` can be used in overloading. The compiler will prefer the non`const` versions when we pass a non`const` object or pointer to an overloaded function.**
- If more than one function matches the call, it is an ambiguous call.
- **Local variables will hide uses of all declarations with that name in an outer scope. Functions with the same name will not overload.**

## Features for Specialized Uses

- Only trailing parameters can have default arguments.
- Each parameter can have its default specified only once in a given scope (in the header).
- **A default argument can be any expression that has a type that is convertible to the type of the parameter, including global variables, function calls, etc.**

```cpp
// the declarations of wd, def, and ht must appear outside a function
sz wd = 80;
char def = ' ';
sz ht();
string screen(sz = ht(), sz = wd, char = def);
string window = screen(); // calls screen(ht(), 80, ' ')

void f2()
{
	def = '*'; // changes the value of a default argument
	sz wd = 100; // hides the outer definition of wd but does not change the default
	window = screen(); // calls screen(ht(), 80, '*')
}
```

- **`constexpr` function: the `return` type and the type of each parameter must be a literal type, and the function body must contain exactly only one `return` statement. Compilers will expand the function immediately (replace with a value inline).**
	- A `constexpr` function is permitted to return a non-constant value (when non-constant expression is passed in as arguments).
	- **The aim of introducing `constexpr` functions and types is to expand at compilation.**

```cpp
constexpr int new_sz() { return 42; }
// scale(arg) is a constant expression if arg is a constant expression
constexpr size_t scale(size_t cnt) { return new_sz() * cnt; }
```

- `inline` and `constexpr` functions normally are defined in headers.
- `assert(expr)` (defined in `cassert`) is a preprocessor macro that terminates if the expression is false.
- The behavior of `assert` depends on the preprocessor variable `NDEBUG`. If it is defined by using the command-line option `-D NDEBUG`, `assert` does nothing.
- **`NDEBUG` can also be used to write conditional debugging code. Local static variables `__func__`, `__FILE__`, `__LINE__`, `__TIME__`, `__DATE__` defined by the compiler can be utilized.**

## Function Matching

- Function matching involves the following steps:
	- Identifying the candidate functions (with the same function name).
	- Selecting viable functions (same number of arguments, matching/convertible types).
	- **Finding the best match, if any. The compiler will determine argument by argument which function is the best match. If one function is chosen for all arguments, it is the best match. Otherwise, an ambiguous call will be reported as an error.**
- Ranks of conversions for determining the best match:
	- An exact match. Converting arrays/functions to pointers and discarding top-level `const` also count as an exact match.
	- Match through a `const` conversion (adding low-level `const`).
	- Match through a promotion **(integral types: always promote to `int` or larger).**
	- Match through an arithmetic **(unlike promotion, all arithmetic conversions are treated as equivalent without precedence)** or pointer conversion (`nullptr`/0, `void *`).
	- Match through a class-type conversion.

```cpp
void manip(long);
void manip(float);
manip(3.14); // error: ambiguous call
```

## Pointers to Functions

- A function’s type is determined by its return type and the types of its parameters.
- **To declare a function pointer, we declare a pointer in place of the function name: `bool (*pf)(const string &, const string &)`.**
- Like arrays, the name of a function is automatically converted to a pointer. The pointer can be used directly to call it. **There is no need to dereference (`*`)/get the address (`&`).**
- When assigning an overloaded function to a pointer, the type must match exactly.
- **A function can be used as a function parameter. We can write a parameter that looks like a function type, but it will be treated as a pointer.**

```cpp
// third parameter is a function and is automatically treated as a pointer
void useBigger(const string &s1, const string &s2,
			bool pf(const string &, const string &));
// equivalent declaration: explicitly define the parameter as a pointer to function
void useBigger(const string &s1, const string &s2,
			bool (*pf)(const string &, const string &));
```

- **Type alias for functions (pointers):**

```cpp
typedef bool Func(const string&, const string&);
typedef decltype(lengthCompare) Func2; // equivalent type
typedef bool(*FuncP)(const string&, const string&);
typedef decltype(lengthCompare) *FuncP2; // equivalent type

using F = int(int*, int); // F is a function type, not a pointer
using PF = int(*)(int*, int); // PF is a pointer type
```

- As with arrays, we must write the return type as a function pointer to return it. A trailing return or `decltype`/type alias can simplify the declaration:

```cpp
int (*f1(int))(int*, int);
auto f1(int) -> int (*)(int*, int);
decltype(f) *f1(int);
```
