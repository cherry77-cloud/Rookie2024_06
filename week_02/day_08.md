## Go 语言函数详解

### 1. 函数基础

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

### 2. 参数处理
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
### 3. 返回值
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
### 4. 变量作用域

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

### 5. 函数类型与变量
在 Go 语言中，函数是一等公民，这意味着函数可以像普通变量一样被赋值、传递和返回。通过定义函数类型，我们可以将函数抽象为一种类型，从而更方便地使用函数。
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
