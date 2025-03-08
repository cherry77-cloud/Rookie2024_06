# Go 语言基础语法

#### 标识符规则
- 组成元素：字母/数字/`_`
- 必须以`字母`或`_`开头
- 有效示例：`userName`, `_id`, `MAX_SIZE`

#### 关键字列表 (25个)
```go
break    default      func    interface  select
case     defer        go      map        struct
chan     else         goto    package    switch
const    fallthrough  if      range      type
continue for          import  return     var
```

#### 保留字列表 (37个)
```go
Constants:   true  false  iota  nil

Types:       int  int8  int16  int32  int64  
             uint  uint8  uint16  uint32  uint64  uintptr
             float32  float64  complex128  complex64
             bool  byte  rune  string  error

Functions:   make  len  cap  new  append  copy  close  delete
             complex  real  imag panic  recover
```

#### 变量系统
- Go语言中的变量需要声明后才能使用，同一作用域内不支持重复声明。并且Go语言的变量声明后必须使用。
- Go语言在声明变量的时候，会自动对变量对应的内存区域进行初始化操作。每个变量会被初始化成其类型的默认值，例如：整型和浮点型变量的默认值为`0`。 字符串变量的默认值为空字符串。布尔型变量默认为`false`。切片、函数、指针变量的默认为`nil`。
- 匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。
```go
// 标准声明
var count int
var name string

// 批量声明
var (
    age int
    score float32
    isActive bool
)

// 初始化特性
var a int           // 默认值 0
var s = "hello"     // 类型推导
var x, y = 1, true  // 多重初始化

// 短声明方式（函数内部）
func main() {
    width := 100           // 自动推断为int
    title := "Go Handbook" // 自动推断为string
}

// 匿名变量机制
func getData() (int, string) {
    return 2023, "Success"
}
year, _ := getData()  // 忽略状态信息
```

#### 常量系统
- 相对于变量，常量是恒定不变的值，多用于定义程序运行期间不会改变的那些值。 常量的声明和变量声明非常类似，只是把`var`换成了`const`，常量在定义的时候必须赋值。
- `const`同时声明多个常量时，如果省略了值则表示和上面一行的值相同。
```go
const PI = 3.1415926
const (
    VERSION = "1.0"
    MAX_CONN = 1000
)

const (
    n1 = 100
    n2
    n3
)
```
