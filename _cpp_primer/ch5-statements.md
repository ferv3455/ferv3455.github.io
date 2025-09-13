---
layout: post
title: Chapter 5 Statements
date: 2024/9/20
chapter: 5
toc: true
---

## Simple Statements

- Null statements should better be commented.
- Compound statements are used when a single statement but the logic of the program needs more than one (`while`, `for` loops). Curly braces make them into one statement.

## Statement Scope

- **Variables can also be defined in conditions. It's visible within the statement.**

## Conditional Statements

- Each `else` is matched with the closest preceding unmatched `if`.
- The expression in the `switch` statement will be converted to integral type.
- **`case` labels must be integral constant expressions.**
- **A label may not stand alone; it must precede a statement or another `case` label.**
- It is illegal to jump from a place where a variable with an initializer is out of scope to a place where that variable is in scope. **If we need to define and initialize a variable for a particular `case`, we can define the variable inside a block.**

## Iterative Statements

- All variables in the `init-statement` must have the same base type.
- The range `for` statement has the form of `for (declaration : expression) ...`. **The expression must represent a sequence, such as a braced initializer list, an array, etc.**
- A range `for` is defined based on the traditional `for`. **On each iteration the control variable is defined and initialized by the next value, and then the statement is executed:**

```cpp
for (declaration : expression) statement
// Is equivalent to
for (auto beg = expression.begin(), end = expression.end(); beg != end; ++beg)
{
	declaration = *beg;
	statement
}
```

- The `do while` loop does not allow variable definitions inside the condition (it will not be evaluated until after the statement is executed).

## Jump Statements

- For `for` loops, `continue` will continue execution at the expression in the header.
- `goto` cannot transfer control from a point where an initialized variable is out of scope to a point where that variable is in scope. **However, backward jumping over an initialized variable definition is okay (the previous definition will be destroyed).**

## `try` Blocks and Exception Handling

- `throw` expressions raise exceptions to indicate that something can't be handled.
- `try` blocks start with `try` and ends with multiple `catch` clauses (exception handlers). Each catch clause consists of `catch`, the declaration of a object, and a block.
- **Error types can be used in the `stdexcept` library. Each of the library exception classes defines a member function `what` that returns a C-style string (a copy of the input string).**
- The search for a handler reverses the call chain. If no matching `catch` is found, the function terminates, and its caller is searched next. If no appropriate `catch` is found, execution is transferred to a library function `terminate` to stop the execution of the program.
- `exception` defines the most general kind of exception class `exception`. `new` defines `bad_alloc`. `type_info` defines `bad_cast`. **They can only be default initialized.**
- **Standard exceptions defined in `stdexcept` (initialized with a string):**

<img src="/posts/cpp-primer/attachments/Pasted image 20240920141303.png">
