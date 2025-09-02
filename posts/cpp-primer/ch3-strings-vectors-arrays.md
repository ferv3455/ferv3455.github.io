---
layout: post
title: C++ Primer (5th Edition) Chapter 3 Strings, Vectors, and Arrays
date: 2024/9/19
---

# Chapter 3 Strings, Vectors, and Arrays

## 3.1 Namespace `using` Declarations

- A `using` declaration lets us use a name without qualifying the name with a namespace prefix: `using namespace::name`.
  - It is still ok to explicitly use the namespace afterwards.
  - Each `using` declaration can only introduce one name.
- **Headers should not include `using` declarations**. If a header has a `using` declaration, than every program that includes that header gets the same `using` declaration, which may not be intended.

## 3.2 Library `string` Type

- The header `string` must be included; `string` is defined in the `std` namespace.

### Defining and Initializing `string`s

- String literals are arrays of `const char`s, so initializing from a string literal is actually initializing from a character array.

```cpp
// Empty string
string s1;            // default initialization

// From another string
string s2(s1);		  // direct initialization
string s2 = s1; 	  // copy initialization

// From a string literal (basically the same as from a character array)
string s3("value");   // direct initialization
string s3 = "value";  // copy initialization

// From a single character
string s4(n, 'c');    // direct initialization only (two values)

// From a character array (C-style string)
char arr[] = "value";
string s5(arr);       // direct initialization (null-terminated only)
string s5 = arr;      // copy initialization (null-terminated only)
string s5(arr, 3);    // direct initialization only (specific length)
```

### Operations on `string`s

<img src="./attachments/table-3-2-string-operations.png" >

- Strings are mutable in C++, unlike Python or Java.
- Reading and writing a string:
  - `getline(is, s)` reads the given stream up to and including the first newline, and stores it **not including the newline** in the string argument. Like the input operator, `getline` returns the `istream` argument.
- Basic operations:
  - `s.size()` returns a `string::size_type` value (an **unsigned type** big enough to hold any size). **Use `auto`/`decltype(s.size())` to define variables for storing the size.**
    - **These companion types make it possible to use the library types in a machine-independent manner.**
    - Be careful of expressions that mix signed and unsigned (size) data.
  - Strings are compared using the same strategy as a case-sensitive dictionary.
  - Adding two strings yields the concatenation of both strings.
- Type conversion:
  - The `string` library lets us (implicitly) convert both character literals and string literals to `string` objects. Therefore, we can use them where a `string` is expected. However, we need to be sure that **at least one operand to each `+` operator must be of `string` type** to trigger such type conversion.

```cpp
string s1 = "hello", s2 = "world";
string s3 = s1 + ", " + s2 + '\n';
string s4 = s1 + ", ";              // ok: adding a string and a literal
string s5 = "hello" + ", ";         // error: no string operand
string s6 = s1 + ", " + "world";    // ok: each + has a string operand (left to right)
string s7 = "hello" + ", " + s2;    // error: can't add string literals
```

### Dealing with the Characters in a `string`

- Functions for handling the characteristics of a character (defined in `cctype`):

<img src="./attachments/table-3-3-cctype-functions.png" >

- To access individual characters, we can use a subscript operator (`[]`, takes a `string::size_type` value) or an iterator (section 3.4).
  - Indexing outside of the string's bounds will **not** be checked and is undefined behavior.
- Range-based `for` iterates through the elements in a given sequence and performs some operation (read or change) on each value:

```cpp
for (declaration : expression) {
    statement
}

// Note: for const strings, each element is a reference to const char (section 2.5)
const string s = "xxx";
for (auto &c : s) {  // Here c is of type const char &
    // process character c
}
```


## 3.3 Library `vector` Type

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
