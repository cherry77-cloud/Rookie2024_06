## Go 语言中的结构体

在 Go 语言中，虽然缺少传统面向对象的“类”概念，但通过 **自定义类型** 和 **类型别名** 提供了灵活的类型定义机制。以下是两者的核心区别与应用场景：

---

### 1. 自定义类型

通过 `type` 关键字创建全新的类型:
- 独立类型：在类型系统中视为与原类型完全不同的类型。
- 方法扩展：可以为新类型定义方法。
- 类型转换：需要显式转换才能与底层类型交互。
```go
type NewInt int  // NewInt 是一个全新的类型，底层基于 int
type Celsius float64 // 自定义类型，表示摄氏度

// 为 Celsius 定义方法
func (c Celsius) ToFahrenheit() Fahrenheit {
    return Fahrenheit(c*9/5 + 32)
}

func main() {
    temp := Celsius(100)
    fmt.Println(temp.ToFahrenheit()) // 输出：212
}
```
---
### 2. 类型别名
- 透明替换：别名在编译时会被替换为原类型。
- 无方法扩展：不能为别名类型定义新方法。
- 兼容性用途：常用于代码重构或简化复杂类型名称
通过 `=` 符号为现有类型定义别名
```go
type MyInt = int  // MyInt 是 int 的别名，编译后等同于 int
type IntAlias = int

func main() {
    var a IntAlias = 10
    fmt.Printf("类型: %T\n", a) // 输出：int
}
```
| 特性               | 自定义类型（`type NewType int`）        | 类型别名（`type Alias = int`）         |
|--------------------|----------------------------------------|---------------------------------------|
| **类型系统**       | 全新类型，与原类型不兼容               | 编译后替换为原类型，完全兼容          |
| **方法扩展**       | 可定义新方法                           | 不可定义新方法                        |
| **类型转换**       | 需显式转换（`NewType(value)`）         | 无需转换，直接使用原类型              |
| **适用场景**       | 扩展类型功能，定义领域特定类型         | 代码兼容性、简化复杂类型名称          |
---

### 3. 结构体
在 `Go` 语言中，结构体（`struct`）是一种自定义的复合数据类型，用于封装多个不同类型的字段。结构体可以用来描述一组相关的属性，非常适合表示复杂的数据结构。
```go
// 使用 type 和 struct 关键字定义结构体
type 类型名 struct {
    字段名 字段类型
    字段名 字段类型
    …
}

type Person struct {
    name string
    city string
    age  int8
}

// 相同类型的字段可以写在一行
type Person struct {
    name, city string
    age        int8
}
```
---
### 4. 结构体实例化
在 `Go` 语言中，结构体实例化是为结构体分配内存并初始化其字段的过程。只有实例化后，才能使用结构体的字段。以下是结构体实例化的几种方式及其详细说明。
```go
// var 结构体实例 结构体类型
type Person struct {
    name string
    city string
    age  int8
}

func main() {
    var p1 Person
    p1.name = "沙河娜扎"
    p1.city = "北京"
    p1.age = 18
    fmt.Printf("p1=%v\n", p1)  // 输出：p1={沙河娜扎 北京 18}
    fmt.Printf("p1=%#v\n", p1) // 输出：p1=main.Person{name:"沙河娜扎", city:"北京", age:18}
}
// 使用 . 访问结构体字段。未显式初始化的字段会被赋予其类型的零值
```
匿名结构体
```go
// 匿名结构体是一种临时定义的结构体类型，无需提前声明
// 适用于临时数据结构。无法复用，每次使用时需重新定义
func main() {
    var user struct {
        Name string
        Age  int
    }
    user.Name = "小王子"
    user.Age = 18
    fmt.Printf("%#v\n", user) // 输出：struct { Name string; Age int }{Name:"小王子", Age:18}
}
```
创建指针类型结构体
```go
// 使用 new 关键字实例化结构体，返回结构体的指针：
var p2 = new(结构体类型)
func main() {
    var p2 = new(Person)
    fmt.Printf("%T\n", p2)     // 输出：*main.Person
    fmt.Printf("p2=%#v\n", p2) // 输出：p2=&main.Person{name:"", city:"", age:0}

    p2.name = "小王子"
    p2.age = 28
    p2.city = "上海"
    fmt.Printf("p2=%#v\n", p2) // 输出：p2=&main.Person{name:"小王子", city:"上海", age:28}
}
// new 返回的是结构体的指针。可以直接通过指针访问字段（Go 语言自动解引用）
```
取结构体的地址实例化
```go
func main() {
    p3 := &Person{}
    fmt.Printf("%T\n", p3)     // 输出：*main.Person
    fmt.Printf("p3=%#v\n", p3) // 输出：p3=&main.Person{name:"", city:"", age:0}

    p3.name = "七米"
    p3.age = 30
    p3.city = "成都"
    fmt.Printf("p3=%#v\n", p3) // 输出：p3=&main.Person{name:"七米", city:"成都", age:30}
}
```
| 实例化方式       | 语法                          | 返回值类型       | 特点                                   |
|------------------|-------------------------------|------------------|----------------------------------------|
| 基本实例化       | `var p1 Person`               | 结构体实例       | 直接分配内存，字段需手动初始化         |
| 匿名结构体       | `var user struct{...}`        | 结构体实例       | 临时使用，无需提前定义类型             |
| 指针实例化       | `var p2 = new(Person)`        | 结构体指针       | 返回指针，自动初始化为零值             |
| 取地址实例化     | `p3 := &Person{}`             | 结构体指针       | 语法糖，底层等价于 `new`               |
---

### 5. Go 语言结构体初始化
在 `Go` 语言中，结构体的初始化是为结构体字段赋予初始值的过程。未初始化的结构体字段会默认设置为对应类型的零值。
1. 默认初始化
```go
// 未显式初始化的结构体字段会被赋予其类型的零值。
type Person struct {
    name string
    city string
    age  int8
}

func main() {
    var p4 Person
    fmt.Printf("p4=%#v\n", p4) // 输出：p4=main.Person{name:"", city:"", age:0}
}
```
2. 使用键值对初始化
```go
// 可以指定部分字段，未指定的字段默认为零值。
// 适用于字段较多或需要选择性初始化的场景。
p5 := Person{
    name: "小王子",
    city: "北京",
    age:  18,
}
fmt.Printf("p5=%#v\n", p5) // 输出：p5=main.Person{name:"小王子", city:"北京", age:18}

p7 := &Person{
    city: "北京",
}
fmt.Printf("p7=%#v\n", p7) // 输出：p7=&main.Person{name:"", city:"北京", age:0}
```
3. 使用值的列表初始化
```go
// 必须初始化所有字段。值的顺序必须与字段声明顺序一致。
// 不能与键值对初始化混用
p8 := &Person{
    "沙河娜扎",
    "北京",
    28,
}
fmt.Printf("p8=%#v\n", p8) // 输出：p8=&main.Person{name:"沙河娜扎", city:"北京", age:28}
```
4. 指针初始化
```
p6 := &Person{
    name: "小王子",
    city: "北京",
    age:  18,
}
fmt.Printf("p6=%#v\n", p6) // 输出：p6=&main.Person{name:"小王子", city:"北京", age:18}
```
| 初始化方式       | 语法                                      | 特点                                   |
|------------------|-------------------------------------------|----------------------------------------|
| 默认初始化       | `var p Person`                           | 字段默认为零值                         |
| 键值对初始化     | `p := Person{name: "Alice", age: 25}`     | 可选择性初始化字段，未指定字段为零值   |
| 值的列表初始化   | `p := Person{"Alice", "北京", 25}`        | 必须初始化所有字段，顺序与声明一致     |
| 指针初始化       | `p := &Person{name: "Alice", age: 25}`    | 返回结构体指针，语法简洁               |
---
