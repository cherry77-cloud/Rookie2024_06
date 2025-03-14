## Go 语言函数详解

### 一. 函数基础

```go
func 函数名(参数) (返回值) {
    // 函数体
}
```
- 函数名: 由字母、数字、下划线组成。但函数名的第一个字母不能是数字。在同一个包内，函数名也称不能重名
- 参数: 参数由参数变量和参数变量的类型组成，多个参数之间使用逗号分隔
- 返回值: 返回值由返回值变量和其变量类型组成，也可以只写返回值的类型，多个返回值必须用`()`包裹，并用逗号分隔。
- 函数体: 实现指定功能的代码块
- 函数可以赋值给变量、作为参数传递或返回值
- 首字母大写表示包外可访问，小写表示仅包内可用
- 调用有返回值的函数时，可以不接收其返回值

```go
// 无参无返回值
func sayHello() {
    fmt.Println("Hello")
}

// 有参有返回值
func add(x, y int) int {
    return x + y
}
```
---

### 二. 参数处理
- 函数的参数中如果相邻变量的类型相同，则可以省略类型
- 可变参数是指函数的参数数量不固定。`Go`语言中的可变参数通过在参数名后加`...`来标识
- 本质上，函数的可变参数是通过切片来实现的

```go
// 类型简写
func sum(x, y int) int {  // 等价于 (x int, y int)
    return x + y
}

// 可变参数
func sum(nums ...int) int {  // nums 实际是切片
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}

// 固定参数 + 可变参数
func show(prefix string, nums ...int) {
    fmt.Print(prefix + ":")
    for _, num := range nums {
        fmt.Printf(" %d", num)
    }
}
```
---
### 三. 返回值
```go
// 多返回值
func swap(x, y int) (int, int) {
    return y, x
}

// 命名返回值
func calc(x, y int) (sum int, sub int) {
    sum = x + y
    sub = x - y
    return  // 自动返回已命名的返回值
}

// 返回值补充
func someFunc(x string) []int {
	if x == "" {
		return nil // 没必要返回[]int{}
	}
	...
}
```
---
### 四. 变量作用域

```go
package main

import "fmt"

var globalVar = 100 // 全局变量

func main() {
    fmt.Println(globalVar) // 100

    localVar := 200 // 局部变量
    fmt.Println(localVar) // 200

    if true {
        blockVar := 300 // 语句块变量
        fmt.Println(blockVar) // 300
    }
    // fmt.Println(blockVar) // 错误：blockVar 不可见
}
```
| 变量类型     | 作用域范围   | 生命周期         | 特点                                   |
|--------------|--------------|------------------|----------------------------------------|
| 全局变量     | 整个包       | 程序运行期间     | 始终存在于内存中                       |
| 局部变量     | 函数内部     | 函数调用期间     | 函数返回时销毁                         |
| 语句块变量   | 语句块内部   | 语句块执行期间   | 语句块结束时销毁                       |
---

### 五. 函数类型与变量
在 `Go` 语言中，函数是一等公民，这意味着函数可以像普通变量一样被赋值、传递和返回。通过定义函数类型，我们可以将函数抽象为一种类型，从而更方便地使用函数。
```go
// type 函数类型名 func(参数列表) 返回值类型
type calculation func(int, int) int
// 该类型的函数接收两个 int 类型的参数，并返回一个 int 类型的值

// 加法函数
func add(x, y int) int {
    return x + y
}

// 减法函数
func sub(x, y int) int {
    return x - y
}

var c calculation      // 声明一个 calculation 类型的变量 c
c = add                // 将 add 函数赋值给 c
fmt.Println(c(1, 2))   // 调用 c，输出：3

func main() {
    var c calculation // 声明一个 calculation 类型的变量 c
    c = add           // 将 add 函数赋值给 c
    fmt.Printf("type of c: %T\n", c) // 输出：type of c: main.calculation
    fmt.Println(c(1, 2)) // 输出：3

    f := add // 将 add 函数赋值给变量 f
    fmt.Printf("type of f: %T\n", f) // 输出：type of f: func(int, int) int
    fmt.Println(f(10, 20)) // 输出：30
}
```

- 回调函数: 函数类型常用于实现回调函数
- 函数作为返回值: 函数类型可以作为返回值，用于实现工厂模式或策略模式


```go
func calculate(x, y int, op calculation) int {
    return op(x, y)
}

func main() {
    result := calculate(10, 20, add) // 使用 add 函数
    fmt.Println(result) // 输出：30
}

func getOperation(op string) calculation {
    switch op {
    case "add":
        return add
    case "sub":
        return sub
    default:
        return nil
    }
}

func main() {
    op := getOperation("add")
    fmt.Println(op(10, 20)) // 输出：30
}
```
---

### 六. 匿名函数与闭包
在 `Go` 语言中，匿名函数和闭包是非常强大的特性。它们允许我们在不定义命名函数的情况下直接使用函数，并且可以捕获和保留外部作用域的变量
```go
// 匿名函数是没有函数名的函数，可以直接定义并使用
func(参数列表) 返回值类型 {
    // 函数体
}

// 赋值给变量
add := func(x, y int) int {
    return x + y
}
fmt.Println(add(10, 20)) // 输出：30

// 立即执行
func(x, y int) {
    fmt.Println(x + y)
}(10, 20) // 输出：30

// 作为回调函数
func process(numbers []int, callback func(int)) {
    for _, num := range numbers {
        callback(num)
    }
}

process([]int{1, 2, 3}, func(n int) {
    fmt.Println(n * 2)
})
```

- 闭包是一个函数与其相关的引用环境的组合。简单来说，`闭包 = 函数 + 引用环境`
- 闭包可以捕获并保留外部作用域的变量
- 闭包的生命周期与捕获的变量绑定，即使外部函数执行结束，捕获的变量仍然有效


```go
// 计数器
func adder() func(int) int {
    var x int // 外部变量
    return func(y int) int {
        x += y // 修改外部变量
        return x
    }
}

func main() {
    f := adder()
    fmt.Println(f(10)) // 输出：10
    fmt.Println(f(20)) // 输出：30
    fmt.Println(f(30)) // 输出：60
}

// 带初始值的闭包
func adder2(base int) func(int) int {
    return func(y int) int {
        base += y // 修改外部变量 base
        return base
    }
}

func main() {
    f := adder2(100)
    fmt.Println(f(10)) // 输出：110
    fmt.Println(f(20)) // 输出：130
}

// 后缀生成器
func makeSuffixFunc(suffix string) func(string) string {
    return func(name string) string {
        if !strings.HasSuffix(name, suffix) {
            return name + suffix
        }
        return name
    }
}

func main() {
    jpgFunc := makeSuffixFunc(".jpg")
    txtFunc := makeSuffixFunc(".txt")
    fmt.Println(jpgFunc("test")) // 输出：test.jpg
    fmt.Println(txtFunc("test")) // 输出：test.txt
}

// 多闭包共享环境
func calc(base int) (func(int) int, func(int) int) {
    add := func(i int) int {
        base += i
        return base
    }
    sub := func(i int) int {
        base -= i
        return base
    }
    return add, sub
}

func main() {
    add, sub := calc(10)
    fmt.Println(add(1), sub(2)) // 输出：11 9
    fmt.Println(add(3), sub(4)) // 输出：12 8
}
```

#### 闭包的本质
- 闭包的本质是函数和其引用环境的组合。它允许函数访问和修改外部作用域的变量，即使外部函数已经执行完毕。
- 捕获变量：闭包会捕获外部作用域的变量。
- 生命周期：捕获的变量会一直存在，直到闭包不再被引用。
- 共享环境：多个闭包可以共享同一个外部变量。

#### 闭包的应用场景
- 延迟计算：闭包可以延迟计算，直到真正需要时才执行。
- 回调函数：闭包可以作为回调函数，捕获上下文信息。
- 工厂模式：闭包可以用于生成特定行为的函数。
- 状态管理：闭包可以用于封装状态，避免全局变量污染。

| 特性       | 说明                                                                 |
|------------|----------------------------------------------------------------------|
| 匿名函数   | 没有函数名的函数，可以直接定义和使用。                               |
| 闭包       | 函数 + 引用环境，可以捕获和修改外部变量。                            |
| 捕获变量   | 闭包可以访问和修改外部作用域的变量。                                 |
| 生命周期   | 捕获的变量会一直存在，直到闭包不再被引用。                           |
| 应用场景   | 延迟计算、回调函数、工厂模式、状态管理等。                           |

---

### 七. `defer`语句
`Go` 语言中的 `defer` 语句会将其后面跟随的语句进行延迟处理。在 `defer` 归属的函数即将返回时，将延迟处理的语句按 `defer` 定义的逆序进行执行，也就是说，先被 `defer` 的语句最后被执行，最后被 `defer` 的语句，最先被执行。

```go
func main() {
    fmt.Println("start")
    defer fmt.Println(1)
    defer fmt.Println(2)
    defer fmt.Println(3)
    fmt.Println("end")
}
// start end 3 2 1
```
- 由于 `defer` 语句延迟调用的特性，所以 `defer` 语句能非常方便地处理资源释放问题。比如：资源清理、文件关闭、解锁及记录时间等
- 在 `Go` 语言的函数中 `return` 语句在底层并不是原子操作，它分为给返回值赋值和`RET`指令两步。而`defer`语句执行的时机就在返回值赋值操作后，`RET`指令执行前
- 返回值 = x -> 运行`defer` -> `RET`指令

```go
func f1() int {
    x := 5
    defer func() {
        x++
    }()
    return x
}

func f2() (x int) {
    defer func() {
        x++
    }()
    return 5
}

func f3() (y int) {
    x := 5
    defer func() {
        x++
    }()
    return x
}

func f4() (x int) {
    defer func(x int) {
        x++
    }(x)
    return 5
}
// 函数	返回值	分析
// f1	5	defer 修改的是局部变量 x，不影响返回值。
// f2	6	defer 修改的是命名返回值 x，影响返回值。
// f3	5	defer 修改的是局部变量 x，不影响命名返回值 y。
// f4	5	defer 函数的参数是值传递，不影响外部的命名返回值 x。

// 在 return 赋值后，RET 指令前执行
// 如果函数有命名返回值，defer 可以修改返回值
// defer 函数的参数是值传递，不会影响外部变量
```
---
### 八. 内置函数介绍

| 内置函数       | 介绍                                                                 |
|----------------|----------------------------------------------------------------------|
| `close`        | 主要用来关闭 `channel`                                               |
| `len`          | 用来求长度，比如 `string`、`array`、`slice`、`map`、`channel`        |
| `new`          | 用来分配内存，主要用来分配值类型，比如 `int`、`struct`。返回的是指针 |
| `make`         | 用来分配内存，主要用来分配引用类型，比如 `chan`、`map`、`slice`      |
| `append`       | 用来追加元素到数组、`slice` 中                                       |
| `panic` 和 `recover` | 用来做错误处理                                           |
