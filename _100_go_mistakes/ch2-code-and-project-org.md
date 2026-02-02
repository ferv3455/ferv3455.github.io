---
layout: post
title: Chapter 2 Code and Project Organization
date: 2026/2/1
chapter: 2
toc: true
---

## #1 Unintended variable shadowing

> Avoid variable shadowing in most cases to prevent from referencing wrong variables and increase clarity. There are exceptions: `err` for errors.

```go
var client *http.Client
if tracing {
    client, err := createClientWithTracing()
    if err != nil {
        return err
    }
    log.Println(client) // this line ensures the code compiles - `client` is used
}
// Use client
// Mistake: client is always nil
```

- Assign the result to a temporary variable and then assign it to the outer variable.
- Create an `err` variable in the outer scope and use `=` directly.


## #2 Unnecessary nested code

> Align the happy path to the left; you should quickly be able to scan down one column to see the expected execution flow.

- When an `if` block returns, omit the `else` block in ***all*** cases.

```go
if foo() {
    // Case A
    return true
} else {
    // Case B
}

// Better: omit the else block

if foo() {
    // Case A
    return true
}
// Case B
```

- Flip the condition if necessary to reduce nesting.

```go
if s != "" {
    // ...
} else {
    return errors.New("empty string")
}

// Better: flip the condition

if s == "" {
    return errors.New("empty string")
}
// ...
```


## #3 Misusing init functions

> Init functions may be helpful in some situations (e.g., defining static configuration), but in most cases, we should handle initializations through ad hoc functions.

> *Background: `init` functions are executed after all constants and variables are evaluated in the initialized/imported package. Here "constants and variables" include package names (imported before `init`).*
> - *If multiple `init` functions are defined in a package, they are executed in the order of source file names and the order they appear in a file.*
> - *`_` operator can be used to import a package solely for its side effects.*
> - *`init` cannot be called explicitly.*

Drawbacks of `init` functions:
- **Error management** is limited in `init` functions - only panic is possible.
  - Scenario: the package is exported as a library, but the caller does not want to stop the entire application.
- The init function complicates writing **unit tests** - it will always be executed.
  - Scenario: the `init` function connects to the database, but in unit tests, you want to use a mock database.
- Usually the `init` function initializes a **global variable** (e.g., database connection), which has severe drawbacks (mutable, not isolated).
  - Scenario: unit tests cannot run in parallel because they share the same global variable.

Fix: use a constructor function to create an instance maintained by the caller.

Example of using `init` function (never panics, no global variables): [the official Go blog](http://mng.bz/PW6w)

```go
func init() {
	redirect := func(w http.ResponseWriter, r *http.Request) {
		http.Redirect(w, r, "/", http.StatusFound)
	}
	http.HandleFunc("/blog", redirect)
	http.HandleFunc("/blog/", redirect)

	static := http.FileServer(http.Dir("static"))
	http.Handle("/favicon.ico", static)
	http.Handle("/fonts.css", static)
	http.Handle("/fonts/", static)

	http.Handle("/lib/godoc/", http.StripPrefix("/lib/godoc/", http.HandlerFunc(staticHandler)))
}
```


## #4 Overusing getters and setters

> It is considered non-idiomatic to use getters and setters to access struct fields, but they can sometimes bring some value. Find the right balance between encapsulation and simplicity.

- If getters and setters are used, name the getter method `<FieldName>` (not `Get<FieldName>`) and the setter method `Set<FieldName>`.


## #5 Interface pollution

> Do not try to solve a problem abstractly but solve what has to be solved now. Don't design with interfaces, discover them.


> [The section is available online.](https://100go.co/5-interface-pollution/)

Three cases where interfaces are considered useful:
- **Common behavior** across multiple types (e.g. `sort.Interface` for types that can be sorted).

```go
// sort.Interface
type Interface interface {
    Len() int
    Less(i, j int) bool
    Swap(i, j int)
}
```

- **Decoupling** code from implementation (Liskov Substitution Principle). This benefits unit testing with mocks (or any kind of test doubles).

```go
type customerStorer interface {
    StoreCustomer(Customer) error
}

type CustomerService struct {
    storer customerStorer
}

func (cs CustomerService) CreateNewCustomer(id string) error {
    customer := Customer{id: id}
    return cs.storer.StoreCustomer(customer)
}
```

- **Restricting a type to a specific behavior**. By injecting an interface, the type can only use the methods defined in the interface.

Be cautious when creating abstractions in our code - **abstractions should be discovered, not created**. We should create an interface when we need it, not when we foresee that we could need it. In most cases, guessing the perfect level of abstraction pollutes our code with unnecessary abstractions, making it more complex to read.


## #6 Interface on the producer side

> Keeping interfaces on the consumer side avoids unnecessary abstractions in most cases.

It's not up to the producer to force a given abstraction for all the clients. Instead, it's up to the client to decide whether it needs some form of abstraction and then determine the best abstraction level for its needs.

**Create (unexported) interfaces in the consumer package, as it can define the most accurate abstraction for its need.** Go is not like Java/C# where interfaces define what a type can do (APIs). In Go, interfaces define what the consumer needs (Interface-Segregation Principle, I in SOLID).

However, when we know - not foresee - that an abstraction will be helpful for consumers, we can have it on the producer side (e.g., interfaces in the `encoding` package are used across the standard library).


## #7 Returning interfaces

> Be conservative in what you do (return structs), be liberal in what you accept from others (accept interfaces).

Returning an interface makes our design more complex and restricts flexibility because **we force all clients to use one particular type of abstraction**, which should instead be decided by each client.

However, if we know that an abstraction is helpful, returning an interface is acceptable. Example: `error`, `io.Reader` in `io.LimitReader` (it is an up-front abstraction).


