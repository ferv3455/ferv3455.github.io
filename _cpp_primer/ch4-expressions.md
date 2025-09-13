---
layout: post
title: Chapter 4 Expressions
date: 2024/9/19
chapter: 4
toc: true
---

## Fundamentals

- Small integral type operands are generally promoted to larger types.
- **When we use an object as an rvalue, we use the object’s value (its contents). When we use an object as an lvalue, we use the object’s identity (its location in memory).** We cannot use an rvalue when an lvalue is required.
- When we apply `decltype` to an expression, the result is a reference type if the expression yields an lvalue (e.g. `decltype(*p)`).
- For operators that do not specify evaluation order (except for logical, conditional and comma operators), it is an error for an expression to refer to and change the same object (undefined behavior).

## Basic Operators

- The new standard requires the quotient to be rounded toward zero. `(m/n)*n + m%n == m`.
- The assignment operator can be defined by templates/classes to take an initializer list.
- Assignment is right associative.
- **The prefix increment/decrement operators return the object itself as an lvalue. The postfix ones return a copy of the object's original value as an rvalue.**
- There are no requirements on using logical/arithmetic right shift.

## Member Access, Conditional, `sizeof`, and Comma Operators

- **The precedence of increment operators is higher than that of the dereference operator, so it is common to use `*p++`.**
- Dereference has a lower precedence than dot (member access).
- The conditional operator `?:` is right associative, so it can be nested without parentheses.
- The conditional operator has fairly low precedence.
- **`sizeof expr`/`sizeof (type)` returns a constant expression of type `size_t`. It calculates the size without evaluating its operand (dereferencing an invalid pointer is safe).**
- **`sizeof` an array is the size of the entire array. `sizeof` a `string` or `vector` returns only the size of the fixed part (no dynamic allocated elements).**
- The result of a comma expression is the value of its right-hand expression. It is commonly used in `for` loops.

## Type Conversions

- **For operands of differing types, integral promotions happen first (cast to a larger type), and then the signed operand is converted to unsigned, or the integral operand is cast to a floating-point type.** Since type sizes are different across machines, the results can be machine dependent.
- **Integral types will always be promoted to `int` or a larger integral type.**
- Class types can define type conversions that the compiler will apply automatically (e.g. `while (cin >> s) ...`).
- Named casts: `static_cast` (**allow potential loss of precision, commonly used**), `dynamic_cast` (covered in Chapter 19), `const_cast` (cast away/add `const`), `reinterpret_cast` (low-level reinterpretation of the bit pattern without warnings). Old-style casts does the same conversion as one of these.
- `static_cast` can also be used on casting `char *` to `string` (create a new one).

## Operator Precedence Table

<img src="./attachments/Pasted image 20240919174802.png">
<img src="./attachments/Pasted image 20240919174815.png">
