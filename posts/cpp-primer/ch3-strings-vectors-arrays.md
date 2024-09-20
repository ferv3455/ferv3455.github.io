---
layout: post
title: C++ Primer (5th Edition) Chapter 3 Strings, Vectors, and Arrays
date: 2024/9/19
---

# Chapter 3 Strings, Vectors, and Arrays

## Namespace `using` Declarations

- `using std::cin` can be used to resolve a single name.
- Headers should not include `using` declarations.

## Library `string` Type

- Ways to initialize the string:
	- **It can also be initialized or assigned with a null-terminated character array.**

<img src="./attachments/Pasted image 20240918151352.png" >

- Reading and writing a string:
    - `getline` ignores the newline character at the end.
    - `size` returns a `string::size_type` value (unsigned). **Use `auto`/`decltype(s.size())` to save its value instead of a `int`.**
    - **String literals cannot be concatenated directly.**
    - **Concatenation can be performed with one of the operands being a character array.**

<img src="./attachments/Pasted image 20240918151401.png" >

- Functions for characters (defined in `cctype`):

<img src="./attachments/Pasted image 20240918153209.png" >

- `string` is mutable in C++. By iterating with `for (auto &c : s)`, characters can be changed.

## Library `vector` Type

- Instantiation: compiler creates classes or functions from templates.
- Ways to initialize a vector:
	- If list initialization isn't possible, the compiler will look for other ways to initialize the object from given values (replacing braces with parentheses).
	- **It can also be initialized with begin and end pointers to array elements.**

<img src="./attachments/Pasted image 20240918161240.png" >

- Vector operations:
	- Like strings, `size` returns `vector<...>::size_type`.

<img src="./attachments/Pasted image 20240918162252.png" >

## Introducing Iterators

- Standard container iterator operations:

<img src="./attachments/Pasted image 20240918162707.png" >

- The real types of iterators are `iterator` or `const_iterator` defined according to the type. `const_iterator` behaves like a `const` pointer (cannot write).
- **Some operations (`push_back`) will invalidate all iterators into the container.**
- Iterator arithmetic:
	- Subtraction returns `difference_type` corresponding to the iterator type.

<img src="./attachments/Pasted image 20240918163254.png" >

## Arrays

- An array is a container of a single type with fixed size (known at compile time, constant expression `constexpr`).
- Arrays can be list initialized (dimension can be omitted, or it must be longer than the list).
- Character arrays can be initialized from a string literal (along with the NULL character). If the dimension is specified, it must be greater than the string length (with NULL).
- Array variables cannot be copied or assigned.
- `int *(&arry)[10] = ptrs` defines a reference to an array of ten pointers.
- **An array can also be accessed with a range `for`.** When accessed via subscription, the index should have type `size_t` (unsigned, defined in `cstddef`). **However, the built-in subscript operator doesn't force the value to be unsigned: a negative subscript is acceptable (n element before the pointer).**
- **Using `auto` with an array will yield a pointer type. However, `decltype` will yield the array type with equal length.**
- `begin(a), end(a)` returns pointers to the front and the end (one past the last) of the array (defined in `iterator`).
- Subtracting two pointers returns type `ptrdiff_t` (defined in `cstddef`).
- Character arrays representing strings can be processed with `cstring` library:

<img src="./attachments/Pasted image 20240918171344.png" >

## Multidimensional Arrays

- Elements may be left out of the initializer list: `{ {0}, {4}, {8}}`, `{0, 3, 6, 9}`.
- Range `for` can also be used on multidimensional arrays. **However, for outer loops, loop variables should always be defined as a reference (`auto &`). Otherwise, they will be regarded as pointers.**
- The multidimensional array variable is equivalent to a pointer to the first inner array (not the first element). The type of inner arrays can be aliased or defined as `auto`.
