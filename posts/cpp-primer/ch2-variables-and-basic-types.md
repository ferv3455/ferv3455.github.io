---
layout: post
title: C++ Primer (5th Edition) Chapter 2 Variables and Basic Types
date: 2024/9/19
---

# Chapter 2 Variables and Basic Types

## Primitive Built-in Types

- `wchar_t, char16_t, char32_t` are used for extended character sets.
- `char` is equivalent to either `signed char` or `unsigned char`.
- Assigning floating-point values to integral values will involve value trucation (not identical bit pattern).
- Assigning out-of-range values to unsigned types will involve modulo operation. **Assigning them to signed types are undefined.**
- Expressions with both unsigned and signed types will be converted to unsigned.
- Octal literals begin with `0`, and hexadecimal literals begin with `0x`.
- Prefix of a character/string literal: `u`(`char16_t`), `U` (`char32_t`), `L` (`wchar_t`), `u8` (`char` strings).
- Suffix of a integer/floating-point literal: (integer) `u/U`, `l/L`, `ll/LL`, (float)`f/F`, `l/L`.
- A string literal is an array of constant `char`s.
- Generalized escape sequence: `\x`(hex), `\` (oct).

## Variables

- Four ways of initialization: `= 0`(copy initialization), `(0)` (direct initialization), ` = {0}`, `{0}` (list initialization). **Loss of information from conversion is not allowed in list initialization.**
- Variables of built-in type defined inside a function are uninitialized by default.
- `extern` provides a declaration only. An `extern` that has an initializer is a definition.

## Compound Types

- `void *` can hold the address of any data pointer type (cast automatically).
- We cannot have a pointer to a reference, but we can define a reference to a pointer: `int *&r = p` (read from right to left).
- References cannot be bound to literals or expressions.

## `const` Qualifier

- `const` objects initialized from a compile-time constant will be replaced with its corresponding value during compilation.
- `const` variables are local to a file. To share it across files, `extern` needs to be added on both its definition and declarations.
- **A reference to `const` can be bound to a non-`const` object, literal or a general expression. In this way, a temporary object is created by the compiler to store the result of the expression, and is bound by the reference.**
- A reference/pointer to `const` can be bound to non-`const` objects.
- `const` pointers have unchangeable values: put the `const` after the `*`.
- Top-level `const`: object itself is `const`; low-level `const`: referring to a `const` object.
- `&` of a `const int` is low-level `const`.
- `constexpr` variables are implicitly `const` and must be initialized by constant expressions: literals, `const`, **fixed addresses of variables** (addresses of global variables, etc.).
- `constexpr` pointers are always top-level.

## Dealing with Types

- Type alias: `typedef int *...`, `using ... = int *`.
- `auto` tells the compiler to deduce the type from the initializer (must have one). Multiple variables defined with a single `auto` should have consistent types.
- **Top-level `const` will be ignored by `auto`. `const auto` is needed.**

```cpp
int i = 0;
const int ci = i;
auto &g = ci;            // g is a const int& that is bound to ci
const auto &j = 42;      // bind a const reference to a literal
auto k = ci, &l = i;     // k is int; l is int&
auto &m = ci, *p = &ci;  // m is const int&; p is a pointer to const int
```

- `decltype(f()) sum = x` uses the type of the return value of `f` to define `sum` without calling `f`. Expressions and variables can also be used.
- **Some expressions will cause `decltype` to yield a reference type while others won't: `*` operator yields a reference type, `+` on a reference yields a plain type, `()` operator yields a reference type.**

## Defining Our Own Data Structures

- We can define variables directly after the class body.
- In-class initializers are restricted to `{}` and `=`.
- Preprocessor: replace the line with designated contents (include, header guards).
