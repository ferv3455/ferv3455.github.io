---
layout: post
title: Learning Go Chapter 7-8 Types and Errors
date: 2024/9/19
---

# Learning Go 笔记 3：类型

## 第七章 类型、方法和接口

### 类型与方法

- 定义：`type <name> <type>`，支持各种类型，如 `struct {}`、`int`、`func(string)int` 等；
    - 可以将自定义类型定义为另一类型，但**不会继承方法，且需要类型转换才能赋值**；
    - 通常用于提高代码可读性。
- 对于**自定义的类型**，可以定义适用于该类型实例的**方法**，**在 `func` 和方法名之间添加接收方**：`func (p Person) String() string {}`（接收方变量名常用首字母）；
- **函数不能重载，方法同样不能重载**（不同类型的同名方法不受限制）；
- **方法的接收方同样可以使用指针（pointer receiver）**：如果方法会修改实例，或需要处理**非法实例（空指针）**，则必须使用指针。否则可以使用值（value receiver）；
- **指针依然使用点操作符 `.` 访问实例的属性与方法**（自动转换）：

```go
type Counter struct {
    total       int
    lastUpdated time.Time
}

func (c *Counter) Increment() {
    c.total++
    c.lastUpdated = time.Now()
}
func (c Counter) String() string {
    return fmt.Sprintf("total: %d, last updated: %v", c.total, c.lastUpdated)
}
```

- 如果用 nil 指针调用值接收方方法，则会引发错误（panic），而指针接收方方法则会成功调用：

```go
func (it *IntTree) Insert(val int) *IntTree {
    if it == nil {
        return &IntTree{val: val}
    }
    if val < it.val {
        it.left = it.left.Insert(val)
    } else if val > it.val {
        it.right = it.right.Insert(val)
    }
    return it
}
```

- 本质上方法就是一般的函数（与 Python 类似，实例是参数之一），可以直接提取函数：
    - Method value: `f1 := myAdder.AddTo` 会自动将实例作为参数之一填入；
    - Method expression: `f2 := Adder.AddTo` 调用时**需要在第一个参数填入实例**。

### 枚举类型 iota

```go
type MailCategory int
const (
    Uncategorized MailCategory = iota
    Personal
    Spam
    Social
    Advertisements
)
```

- **在 const 块中，可以只标注变量名，这样会直接沿用上一行的定义。** iota 利用了这一点进行枚举类型的定义，在每行赋值后自增 iota，实现递增数值的赋值。iota 会在创建 const 块时清零；
- 利用 iota 赋值自增的特性，也可以给枚举变量赋值为其他值，如 2 的幂等。

### 嵌入 embedding

- 在结构类型的定义中，在一行中只填写另一类的名称，可以创建**嵌入域**（embedded field）。**基于嵌入域类型定义的数据域和方法都可以直接通过上层实例访问**（类似子类）：

```go
type Manager struct {
    Employee
    Reports []Employee
}

m := Manager{
    Employee: Employee{
        Name: "Bob Bobson",
        ID: "12345",
    },
    Reports: []Employee{},
}
fmt.Println(m.ID)            // prints 12345
fmt.Println(m.Description()) // method for type Employee
```

- 如果出现了同名数据域或方法，则内层的会被覆盖。可以通过**显式指定内层类型访问**：`m.Employee.Name`，**其中 `m.Employee` 就是 `Employee` 类型的实例**，可用于赋值；
- 嵌入并非继承（两个类无法直接进行类型转换），也不支持多态（不同类拥有同名方法时，方法的选择**仅取决于调用实例的变量声明类型，而非实际类型**），仅简单地扩充了方法集合。

### 接口 interface

- 使用 `interface` 关键字定义类别，指定接口的方法集合，名称常以 “er“ 结尾：

```go
// Interface type definition
type Stringer interface {
    String() string
}

// Variable definition with anonymous interface
var stringer interface {
    String() string
}
```

- 接口是隐式实现的，只需要某个类实现了接口指定的方法集合，那么该类就实现了这个接口，**该类的实例可以被直接赋值给接口类型的变量**，并调用方法。这是一种类型安全的鸭子类型；
- 从应用角度来看，接口指定了调用者需要哪些功能，而无需知道这些功能具体是通过哪个类型实现的（鸭子类型），这由程序的顶层调用者决定；
- 接口可以嵌入另一接口的定义中；
- 编程习惯：尽可能**接受接口类型作为参数，返回结构类型实例**；
- 在调用函数时，每个接口类型的参数都需要额外分配堆内存，因此需要结合实际情况平衡表现；
- **接口底层由两个指针实现，分别指向实例的实际底层类型（用于查找方法）以及实例值**，因此抽象接口类型的零值也为 nil（需要都为 nil）：
    - nil 接口调用方法会导致异常（无类型），而有类型的 nil 接口则可调用指针接收方方法。

```go
var s *string
fmt.Println(s == nil) // prints true
var i interface{}
fmt.Println(i == nil) // prints true
i = s
fmt.Println(i == nil) // prints false
i = nil
fmt.Println(i == nil) // prints true
```

- 空接口可用于存储任意类型的值，通常用作占位符（存储未知类型），或通用类型扩展。

### 底层类型转变 type assertion/switch

- **类型断言**（type assertion）可用于将较高级的接口类还原为底层类型：`i2, ok := i.(int)`。实现接口的底层实体类**必须和断言一致，否则异常**（必须完全一致，即使互为别名也不正确）；
- 类型断言只是**把底层类型显现出来**，**只能用于接口**，在运行时执行检测，因此会导致异常（panic）；而类型转换可以对任意类型使用，在编译时检测，只会抛出编译错误；
- 如果需要从多个可能的类型中判断底层类型，可以使用 type switch：

```go
switch j := i.(type) {   // usually i := i.(type)
    case nil:
        // i is nil, type of j is interface{}
    case int:
        // j is of type int
    case MyInt:
        // j is of type MyInt
    case io.Reader:
        // j is of type io.Reader
    case string:
        // j is a string
    case bool, rune:
        // i is either a bool or rune, so j is of type interface{}
    default:
        // no idea what i is, so j is of type interface{}
}
```

- 常见应用：判断是否实现了另一接口、支持多个版本的 API；
- 此类类型转变的方式无法处理被封装的类型，因此处理 `error` 接口类时，需要使用 `errors.Is` 与 `errors.As` 等函数进行异常关系分析。

### 接口的其他注意事项

- 简单的单方法接口可以用来指定复杂设计中函数的类型（**使用接口指定方法，在一个新的函数类型 Adapter 上实现这个方法，在该方法中调用这个函数本身**），类似于函数类型的定义与使用；
- 接口显式地指定了代码涉及的功能，无需与实体类之间进行绑定，简洁地实现了依赖注入（dependency injection），只需在最顶层函数修改实体类的使用即可；
- 接口的使用也使得测试更便捷：只需将实体类的方法替换，计算并输出附加信息即可。

## 第八章 错误

### 错误接口 error

- `error` 是单方法接口，包含一个 `Error()` 方法。与一般接口一样，nil 是该接口的零值；
- Go 中不抛出异常，而是通过返回错误（最后一个返回值）的形式分类处理异常。**如果返回错误，则理论上需要将其他返回值置零**；
- 以下两种方式为常见的错误值生成方式：
    - `errors.New()` 函数基于异常信息创建 `error` 错误接口类型的值。**错误信息不应包含大写，且不应以标点或换行符终止**；
    - `fmt.Errorf()` 函数基于格式字符串的方式创建异常。
- 常用哨兵错误（sentinel error）表示需要终止处理流程的错误，用 `Err` 开头的类名表示。可以使用别名 `string` 类来自定义简单的哨兵错误类，来实现 `error` 接口；
- 自定义错误类时，需要注意未初始化的错误类实例并不等于 nil（`error` 为接口，需要类型和指针均为 nil 时才相等）。

### 错误类型包装

- 可以给现有返回的错误添加额外信息，创建错误链；
- 错误包装：
    - `fmt.Errorf` 可用来基于现有错误创建新的错误。**支持 `%w` 格式字符串**，可以将错误类型传入，展示错误信息（常用 `: %w`）。也可以使用 `%v`，此时不会对错误进行包装；
    - 也可以使用自定义错误类型，**需要同时实现 `Error()` 方法和 `Unwrap()` 方法**；
- 使用上述方式包装的错误可以使用 `errors.Unwrap` 解包，返回内部的错误；
- `errors.Is(err, target)` 可用于判断错误及其包装的所有错误中**是否包含指定错误实例**（使用等于号比较各级错误与目标错误实例是否相等）。也可自行实现自定义类的 `Is` 方法，在调用 `errors.Is` 函数时会使用该方法；
- `errors.As(err, ptr)` 可用于判断错误及其包装的所有错误中**是否有与指针类型一致的错误**，并将错误储存在指针位置。其中第二个参数也可以指向接口变量。也可自行实现 `As` 方法；
- 如果需要在函数中多次重复使用相同的 `fmt.Errorf` 语句，则可以**使用 `defer` 语句，在 `return` 语句执行后统一包装错误**。

### 异常恢复

- 异常（panic）是运行时错误。异常发生时，当前函数强制终止，然后**根据函数调用栈的顺序依次执行各级函数中 `defer` 语句的内容**，逐级返回，然后退出程序，展示错误栈；
- `panic(arg)` 抛出自定义的异常，支持任意类型的参数；
- 可以在 `defer` 指定的函数中调用 `recover()` ，返回传入异常的实例，并使程序恢复运行，否则返回 nil。`recover` 并不会体现异常的类型等；
- `panic` 和 `recover` 并非其他语言中的异常处理常用手段。**它们通常用于在致命异常发生时执行指定的错误日志输出步骤，记录关键信息后再退出程序（`os.Exit(1)`）**。也可以用于在开发 API 时将异常限制在自定义代码内部。
