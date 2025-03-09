## Go 语言基本数据类型详解

### 一、整型

#### 1. 标准整型类型
| 类型       | 描述                                   | 值范围                                                     |
|------------|----------------------------------------|-----------------------------------------------------------|
| `int8`     | 有符号8位整型                          | -128 ~ 127                                                |
| `int16`    | 有符号16位整型                         | -32768 ~ 32767                                            |
| `int32`    | 有符号32位整型                         | -2147483648 ~ 2147483647                                  |
| `int64`    | 有符号64位整型                         | -9223372036854775808 ~ 9223372036854775807                |
| `uint8`    | 无符号8位整型 (等价于 `byte`)          | 0 ~ 255                                                   |
| `uint16`   | 无符号16位整型                         | 0 ~ 65535                                                 |
| `uint32`   | 无符号32位整型                         | 0 ~ 4294967295                                            |
| `uint64`   | 无符号64位整型                         | 0 ~ 18446744073709551615                                  |

#### 2. 特殊整型类型
| 类型       | 描述                                                                 |
|------------|----------------------------------------------------------------------|
| `uint`     | 32位系统为 `uint32`，64位系统为 `uint64`                            |
| `int`      | 32位系统为 `int32`，64位系统为 `int64`                              |
| `uintptr`  | 无符号整型，用于存储指针地址                                         |

**注意**：
- `len()` 函数返回值的类型为 `int`
- 跨平台场景中避免使用 `int/uint`（可能因系统位数不同导致问题）

---

### 二、数字字面量语法
```go
v := 0b00101101    // 二进制 45
v := 0o377         // 八进制 255
v := 0x1p-2        // 十六进制浮点数 0.25
v := 123_456       // 使用下划线分隔数字 123456
```

### 三、浮点型

`Go` 语言支持两种浮点型数：`float32` 和 `float64`。这两种浮点型数据格式遵循 `IEEE 754` 标准

#### 1. 浮点型范围
| 类型       | 描述                      | 最大值常量            | 近似范围           |
|------------|---------------------------|-----------------------|--------------------|
| `float32`  | 32 位 IEEE-754 浮点数     | `math.MaxFloat32`     | ±3.4e38           |
| `float64`  | 64 位 IEEE-754 浮点数     | `math.MaxFloat64`     | ±1.8e308          |

#### 2. 打印浮点数
```go
package main
import (
    "fmt"
    "math"
)

func main() {
    fmt.Printf("%f\n", math.Pi)      // 输出: 3.141593
    fmt.Printf("%.2f\n", math.Pi)    // 输出: 3.14
}
```

### 四、复数
`Go` 语言支持两种复数类型：`complex64` 和 `complex128`

| 类型         | 描述                          | 存储方式      |
|--------------|-------------------------------|---------------|
| `complex64`  | 32 位实部 + 32 位虚部         | `float32 × 2` |
| `complex128` | 64 位实部 + 64 位虚部         | `float64 × 2` |

```go
var c1 complex64 = 1 + 2i
var c2 complex128 = 2 + 3i
fmt.Println(c1)  // 输出: (1+2i)
fmt.Println(c2)  // 输出: (2+3i)
```

### 五、布尔值
`Go`语言中以`bool`类型进行声明布尔型数据，布尔型数据只有`true`（真）和`false`（假）两个值
- 布尔类型变量的默认值为`false`。
- `Go` 语言中不允许将整型强制转换为布尔型.
- 布尔型无法参与数值运算，也无法与其他类型进行转换。

### 六、字符串基础

#### 1. 字符串定义
- `Go` 语言中的字符串以原生数据类型出现，使用 `UTF-8` 编码。
- 字符串的值用双引号 `"` 包裹，支持非 `ASCII` 字符。
- `Go`语言中要定义一个多行字符串时，就必须使用反引号字符

```go
s1 := "hello"
s2 := "你好"
s1 := `第一行
第二行
第三行
`
```

#### 2. 字符串常用操作
| 方法                          | 描述                     |
|-------------------------------|--------------------------|
| `len(str)`                    | 计算字符串长度（字节数） |
| `+` 或 `fmt.Sprintf`          | 字符串拼接               |
| `strings.Split(str, sep)`     | 按分隔符分割字符串       |
| `strings.Contains(str, substr)`| 判断是否包含子串         |
| `strings.HasPrefix(str, prefix)`| 判断是否以某前缀开头     |
| `strings.HasSuffix(str, suffix)`| 判断是否以某后缀结尾     |
| `strings.Index(str, substr)`  | 查找子串首次出现的位置   |
| `strings.LastIndex(str, substr)`| 查找子串最后一次出现的位置|
| `strings.Join([]string, sep)` | 将字符串切片连接为字符串 |

```go
package main
import (
    "fmt"
    "strings"
)

func main() {
    s := "hello,世界"
    fmt.Println(len(s))                    // 输出: 12
    fmt.Println(strings.Contains(s, "世")) // 输出: true
    fmt.Println(strings.Split("a,b,c", ",")) // 输出: [a b c]
    fmt.Println(strings.Join([]string{"a", "b", "c"}, "-")) // 输出: a-b-c
}
```

### 一、字符类型

#### 1. 字符定义
- 组成字符串的元素称为“字符”，用单引号 `'` 包裹。
- 示例：
```go
var a = '中'  // rune 类型
var b = 'x'   // byte 类型
```

| 类型     | 描述                   | 存储方式      |
|----------|------------------------|---------------|
| `byte`   | UTF-8 单字节（ASCII）  | `uint8` 别名  |
| `rune`   | Unicode 码点           | `int32` 别名  |

```go
func traversalString() {
    s := "hello沙河"
    for i := 0; i < len(s); i++ {   // 按字节遍历
        fmt.Printf("%v(%c) ", s[i], s[i])
    }
    fmt.Println()
}

func traversalString() {
    s := "hello沙河"
    for _, r := range s {           // 按 rune 遍历
        fmt.Printf("%v(%c) ", r, r)
    }
    fmt.Println()
}
```

#### 2、字符串修改
- 字符串是不可变的，修改需先转换为 []byte 或 []rune，再转回 string
```
func changeString() {
    s1 := "big"
    byteS1 := []byte(s1)
    byteS1[0] = 'p'
    fmt.Println(string(byteS1))  // 输出: pig

    s2 := "白萝卜"
    runeS2 := []rune(s2)
    runeS2[0] = '红'
    fmt.Println(string(runeS2))  // 输出: 红萝卜
}
```

#### 3. 总结
- byte：用于处理 ASCII 字符，存储为 uint8。
- rune：用于处理 Unicode 字符，存储为 int32。
- 字符串遍历：推荐使用 rune 遍历，避免多字节字符乱码。
- 字符串修改：需转换为 []byte 或 []rune，修改后再转回 string。
- Go 语言字符串是不可变的，修改字符串会重新分配内存。使用 rune 处理 Unicode 字符更安全
