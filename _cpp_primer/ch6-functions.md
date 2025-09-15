---
layout: post
title: Chapter 6 Functions
date: 2025/9/15
chapter: 6
toc: true
---

## 6.1 Function Basics

- Arguments are initializers for the function's parameters. All parameters are always initialized. **We have no guarantees about the order in which arguments are evaluated (section 4.1).**
  - Implicit type conversion is allowed just as in any initialization.
- A function's parameter list can be empty but not omitted. For compatibility with C, an empty parameter list can be specified as `void`.
  - Local variables at the outermost scope of the function body may not use the same name as any parameter - they behave like local variables.
  - **Parameter names are optional.** Unused parameters can be omitted in a declaration.
- **The return type of a function may not be an array type or a function type, but a pointer to either of them is acceptable (section 6.3 and 6.7).**

### Local Objects

- **Automatic objects** are objects that exist only while a block is executing. Parameters and local variables are automatic objects, which hide declarations of the same name made in an outer scope.
  - Storage for parameters/local variables is allocated when the function begins and released when the function ends.
- **Local `static` objects are initialized at the first time execution reaches their definition inside the function.** Their lifetime continues across function calls.
  - Local `static` objects are value initialized without an explicit initializer.

### Function Declarations

- Function declarations are also known as **function prototypes**.
- Like any other name, a function may be declared multiple times but defined only once. Functions should be declared in header files and defined in source files.
- In a declaration, a semicolon replaces the function body. Parameter names are often omitted in a declaration, but they can be used to improve readability.

### Separate Compilation

```bash
clang++ main.cpp hello.cpp -o main         # compile and link in one step

clang++ -c main.cpp -o main.o              # separate compilation to object code
clang++ -c hello.cpp -o hello.o
clang++ main.o hello.o -o main             # link object files
```


## 6.2 Argument Passing

> Parameter initialization works the same way as variable initialization.

### Passing Arguments by Value

- When we initialize a nonreference type parameter from an argument, the value of the initializer (argument) is copied. The function does not affect the argument.
  - The same applies to pointers: the address is copied.

### Passing Arguments by Reference

- When we initialize a reference type parameter from an argument, it is bound to the object from which it is initialized. Copies are avoided.
- Reference parameters let us return multiple results from a function.

### `const` Parameters and Arguments

- Just as in any other initialization, top-level `const`s (of both sides) are ignored. Because of this, **two functions with parameters differing only in top-level `const`s cannot be overloaded**.

```cpp
// We can pass either a const or a non-const int to the following functions
void fcn(const int i);
void fcn(int i);           // error: redefinition of fcn(int)
```

- Low-level `const`s are not ignored and thus be matched exactly.
- **Using a reference instead of a reference to `const` unduly limits the type of arguments that can be used with the function.** Use references to `const` whenever possible.
  - References to `const` can be bound to rvalues (e.g. arbitrary expressions, literals, objects of convertible types).
  - References to `const` can be bound to `const` lvalues (e.g. `const` objects, low-level `const` references).


### Array Parameters

- Since we cannot copy an array, and an array is usually converted to a pointer (section 3.5), we are actually passing a pointer (without the size) when we pass an array to a function.

```cpp
// These three declarations are equivalent
void print(const int*);
void print(const int[]);
void print(const int[10]);  // dimension is only for documentation purposes

int i = 0, j[2] = {0, 1};
print(&i);    // ok: int*
print(j);     // ok: int*, which can be used to initialize const int*
```

- Three common techniques to manage pointer parameters (sizes):
  - Add an end marker in the array (e.g. null character in C-style strings).
  - Pass pointers to the first and one past the last element (STL convention).
  - Pass a size parameter.
- **Use pointers to `const` whenever possible.**
- We may define a parameter that is a reference to an array - the size is part of the type. However, this also limits the usefulness of the function.
- **A multi-dimensional array is passed as a pointer to its first element, which is an array. Therefore, the sizes of the second and subsequent dimensions must be specified.**


### `main`: Handling Command-Line Options

- The second parameter of `main` (`argv`) is an array of pointers to C-style strings (null-terminated) (`char *argv[]`). The array can be alternatively defined as a pointer (`char **argv`).
- **The element just past the last in `argv` is guaranteed to be 0 (`nullptr`).**


### Functions with Varying Parameters

- Three primary ways to write a function that takes a varying number of arguments: `initializer_list` (values of the same type), variadic templates (section 16.4), ellipsis (for compatibility with C).
- **`initializer_list` from `initializer_list` header works like a `vector` of `const` values.**

```cpp
// Initialization
initializer_list<T> lst;           // default initialization: empty list
initializer_list<T> lst{a, b, c};  // list initialization (may use =)
initializer_list<T> lst2(lst);     // copy initialization (may use =)

// Limited operations
lst.size();
lst.begin();
lst.end();
```

- **We enclose a sequence of values in curly braces to pass them to a function that takes an `initializer_list` parameter** - they must be of the same type as elements in the `initializer_list` (or convertible to that type without precision loss).
- Note: `auto` can be used to deduce the type (`initializer_list<T>`) of an initializer list, and the same idea applies to using range `for` loop on an initializer list. **However, mixed types are not allowed - no implicit conversions are done.**

```cpp
auto lst = {0, 1, 2};         // lst is an initializer_list<int>
auto lst2 = {0.1, 0.2, 0.3};  // lst2 is an initializer_list<double>
auto lst3 = {0, 1, 2.2};      // error: mixed types

for (auto &i : {0, 1, 2}) { ... }    // ok: i is an int
for (auto &i : {0, 1, 2.2}) { ... }  // error: mixed types
```

- **Ellipsis parameters are used to be compatible with C, in which `varargs` is used to extract the arguments.**
  - An ellipsis parameter must appear as the last element in a parameter list.
  - The comma before the ellipsis is optional if there are other parameters.

```cpp
void foo(int, int, ...);
void foo(int, int...);
void foo(...);
```


## 6.3 Return Types and the `return` Statement

---

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
- **Local variables will hide uses of all declarations with that name in an outer scope. Functions with the same name will not overload. This is because name lookup happens before type checking.**

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
