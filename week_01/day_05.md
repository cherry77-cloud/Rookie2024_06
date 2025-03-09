## Go 语言数组详解

### 一、数组定义与特性

#### 1. 基本概念
- **数组**是相同数据类型元素的集合，长度在声明时固定，不可修改。
- **语法**：`var 数组变量名 [元素数量]T`
- **长度是类型的一部分**：`[3]int` 和 `[5]int` 是不同类型

```go
var a [3]int       // 声明一个长度为3的int数组
var b [4]int       // 错误示例：a = b （类型不兼容）
```

---

### 二、数组核心特性

| 特性         | 说明                                                                 |
|--------------|----------------------------------------------------------------------|
| **固定长度** | 数组在声明时长度固定，不可动态调整。                                 |
| **类型严格** | `[n]T` 是独立类型，不同长度的数组即使元素类型相同也是不同类型。      |
| **下标访问** | 数组下标从 `0` 开始，越界访问会触发 `panic`。                        |
| **值类型**   | 数组是值类型，赋值或传参时会复制整个数组，修改副本不会影响原数组。   |
| **操作符支持** | 支持 `==` 和 `!=` 比较，但仅限内存布局相同的数组（长度和类型一致）。 |

```go
package main
import "fmt"

func main() {
    // 类型严格示例
    var a [3]int
    var b [4]int
    // a = b  // 错误：类型不匹配

    // 值类型示例
    c := [3]int{1, 2, 3}
    d := c  // 复制整个数组
    d[0] = 100
    fmt.Println(c)  // 输出: [1 2 3]（原数组未改变）
    fmt.Println(d)  // 输出: [100 2 3]

    // 操作符支持示例
    e := [3]int{1, 2, 3}
    f := [3]int{1, 2, 3}
    fmt.Println(e == f)  // 输出: true
}
```
---

### 三、数组初始化
- **方法一**：指定长度初始化
```go
func main() {
	var testArray [3]int                                   // 数组会初始化为int类型的零值
	var numArray = [3]int{1, 2}                            // 使用指定的初始值完成初始化
	var cityArray = [3]string{"北京", "上海", "深圳"}       // 使用指定的初始值完成初始化
	fmt.Println(testArray)                                 // [0 0 0]
	fmt.Println(numArray)                                  // [1 2 0]
	fmt.Println(cityArray)                                 // [北京 上海 深圳]
}
```

- **方法二**：编译器推断长度
```go
func main() {
	var testArray [3]int
	var numArray = [...]int{1, 2}
	var cityArray = [...]string{"北京", "上海", "深圳"}
	fmt.Println(testArray)                                   // [0 0 0]
	fmt.Println(numArray)                                    // [1 2]
	fmt.Printf("type of numArray:%T\n", numArray)            // type of numArray:[2]int
	fmt.Println(cityArray)                                   // [北京 上海 深圳]
	fmt.Printf("type of cityArray:%T\n", cityArray)          // type of cityArray:[3]string
}
```

- **方法三**：指定索引初始化
```go
func main() {
	a := [...]int{1: 1, 3: 5}
	fmt.Println(a)                      // [0 1 0 5]
	fmt.Printf("type of a:%T\n", a)     // type of a:[4]int
}
```
---

### 四、数组操作

#### 1. 数组的遍历

```go
\\ 传统 for 循环
a := [...]string{"北京", "上海", "广州"}
for i := 0; i < len(a); i++ {
    fmt.Println(a[i])
}

\\ for range 遍历
for index, value := range a {
    fmt.Printf("索引:%d 值:%s\n", index, value)
}
```

#### 2. 多维数组
```go
// 二维数组（3行2列）
matrix := [3][2]string{
    {"A1", "A2"},
    {"B1", "B2"},
    {"C1", "C2"},
}
fmt.Println(matrix[1][0])  // 输出: B1

// 自动推断第一维长度
autoMatrix := [...][2]int{
    {1, 2},
    {3, 4},
}  // 类型为 [2][2]int


// 遍历多维数组
for _, row := range matrix {
    for _, val := range row {
        fmt.Printf("%s\t", val)
    }
    fmt.Println()
}
```

#### 3. 多维数组只有第一层可以使用`...`来让编译器推导数组长度
```go
//支持的写法
a := [...][2]string{
    {"北京", "上海"},
    {"广州", "深圳"},
    {"成都", "重庆"},
}
//不支持多维数组的内层使用...
b := [3][...]string{
    {"北京", "上海"},
    {"广州", "深圳"},
    {"成都", "重庆"},
}
```
