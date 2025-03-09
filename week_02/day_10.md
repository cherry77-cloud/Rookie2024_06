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

### 4. 结构体实例化
