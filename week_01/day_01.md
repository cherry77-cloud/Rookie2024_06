<div align="center">
  <h1>基础语法</h1>
</div>

### 标识符规则
- 组成元素：字母/数字/`_`
- 必须以`字母`或`_`开头
- 有效示例：`userName`, `_id`, `MAX_SIZE`

### 关键字列表 (25个)
```go
break    default      func    interface  select
case     defer        go      map        struct
chan     else         goto    package    switch
const    fallthrough  if      range      type
continue for          import  return     var
```

### 保留字列表 (37个)
```go
Constants:   true  false  iota  nil

Types:       int  int8  int16  int32  int64  
             uint  uint8  uint16  uint32  uint64  uintptr
             float32  float64  complex128  complex64
             bool  byte  rune  string  error

Functions:   make  len  cap  new  append  copy  close  delete
             complex  real  imag panic  recover
```

### 变量系统
- `Go`语言中的变量需要声明后才能使用，同一作用域内不支持重复声明。并且`Go`语言的变量声明后必须使用。
- `Go`语言在声明变量的时候，会自动对变量对应的内存区域进行初始化操作。每个变量会被初始化成其类型的默认值，例如：整型和浮点型变量的默认值为`0`。 字符串变量的默认值为空字符串。布尔型变量默认为`false`。切片、函数、指针变量的默认为`nil`。
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
---
### 常量系统
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
---
### iota
- `iota`是`go`语言的常量计数器，只能在常量的表达式中使用。
- `iota`在`const`关键字出现时将被重置为`0`。`const`中每新增一行常量声明将使`iota`计数一次(`iota`可理解为`const`语句块中的行索引)。 
```go
const (
    n1 = iota    // 0
    n2 = 100     // 100
    n3 = iota    // 2
    n4           // 3
)
const n5 = iota //0

const (
    _  = iota
    KB = 1 << (10 * iota)
    MB = 1 << (10 * iota)
    GB = 1 << (10 * iota)
    TB = 1 << (10 * iota)
    PB = 1 << (10 * iota)
)

const (
    a, b = iota + 1, iota + 2    // 1, 2
    c, d                         // 2, 3
    e, f                         // 3, 4
)
```
---
<div align="center">
  <h1>常用环境变量</h1>
</div>

### 核心配置
- **`GOROOT`**: `Go` 的安装路径（存放编译器、标准库）。
- **`GOPATH`**: 工作目录（存放代码、依赖、二进制文件，默认 `~/go` 或 `%USERPROFILE%\go`）。
- **`GOBIN`**: `go install` 安装的二进制文件存放路径。

### 模块管理（`Go Modules`）
- **`GO111MODULE`**: 控制 `Go Modules` 模式：
  - `on`: 强制启用。
  - `off`: 禁用。
  - `auto`: 自动。
- **`GOPROXY`**: 模块代理服务器地址（加速依赖下载）。
- **`GOPRIVATE`**: 私有模块路径（不通过代理和校验数据库）。
- **`GONOPROXY`**: 指定不通过代理下载的模块路径。
- **`GONOSUMDB`**: 指定不校验哈希的模块路径。
- **`GOSUMDB`**: 校验和数据库地址（默认 `sum.golang.org`）。
- **`GOINSECURE`**: 允许通过 `HTTP` 下载的模块路径。

### 编译构建
- **`GOOS`**: 目标操作系统（交叉编译）。
- **`GOARCH`**: 目标 `CPU` 架构（交叉编译）。
- **`CGO_ENABLED`**: 是否启用 `CGO`（`1` 启用，`0` 禁用）。
- **`CC/CXX`**: 指定 `C/C++` 编译器（用于 `CGO`）。
- **`GOEXE`**: 指定生成的可执行文件后缀（如 `Windows` 的 `.exe`）。
- **`GOMODCACHE`**: 模块缓存目录（存放下载的依赖包）。

### 测试与调试
- **`GOFLAGS`**: 为 `go` 命令设置默认参数。
- **`GODEBUG`**: 启用调试功能（如内存、`GC` 行为）。
- **`GOTRACEBACK`**: 控制程序崩溃时的堆栈信息级别。
- **`GOTESTTIMEOUT`**: 设置测试超时时间（`Go 1.20+`）。

### 其他实用变量
- **`GOMAXPROCS`**: 设置程序可用的最大 `CPU` 核心数。
- **`GOWORK`**: 启用工作区模式（多模块开发，`Go 1.18+`）。
- **`GOTOOLCHAIN`**: 指定使用的 `Go` 工具链版本。
- **`GO_EXTLINK_ENABLED`**: 控制是否使用外部链接器（`CGO`）。

---

<div align="center">
  <h1>常用命令行工具</h1>
</div>

### 项目初始化与依赖管理
- **`go mod init <模块名>`**: 初始化 `Go` 模块（创建 `go.mod` 文件）。
- **`go mod tidy`**: 自动添加缺失的依赖，删除未使用的依赖。
- **`go get <包路径>`**: 下载并安装依赖包。
- **`go get -u <包路径>`**: 更新依赖到最新版本。
- **`go list -m all`**: 查看当前项目的所有依赖。
- **`go list -json <包路径>`**: 查看包的详细信息（`JSON` 格式）。

### 编译与运行
- **`go run <文件>`**: 直接运行 `Go` 文件（不生成二进制）。
- **`go build`**: 编译当前目录的代码，生成可执行文件（默认输出到当前目录）。
- **`go build -o <输出名>`**: 指定输出文件名。
- **`go install`**: 编译并安装到 `$GOPATH/bin`。
- **`go clean`**: 清理编译生成的文件（如可执行文件、临时文件）。
- **`go env`**: 查看所有 `Go` 环境变量。
- **`go version`**: 查看 `Go` 版本。

---

<div align="center">
  <h1>优势</h1>
</div>

### 直接编译为机器码，无运行时依赖
- **静态编译**: `Go` 代码直接编译成机器码（如 `Linux` 的 `ELF`、`Windows` 的 `PE` 格式），无需虚拟机或解释器。
- **单一二进制文件**: 所有依赖（包括标准库和第三方包）被打包到最终的可执行文件中，部署时无需安装额外环境。
- **跨平台支持**: 通过 `GOOS` 和 `GOARCH` 环境变量，可轻松编译不同操作系统和架构的程序。

### 高效的并发模型：`Goroutine` 与调度器
- **`Goroutine`**: 轻量级线程（协程），初始栈大小仅 `2KB`（线程通常为 `1-2MB`），可轻松创建百万级并发。
- **`GMP` 调度模型**:
  - **`Goroutine`**: 用户态协程。
  - **`Machine`**: 操作系统线程。
  - **`Processor`**: 逻辑处理器，负责调度 `Goroutine` 到线程。
- **非阻塞异步 `IO`**: 基于 `epoll`（`Linux`）或 `kqueue`（`BSD`）的高效网络模型，避免线程切换开销。

### 高效且低延迟的垃圾回收（`GC`）
- **并发标记清除算法**: `Go` 的 `GC` 在大多数阶段与用户代码并发执行，减少停顿时间。
- **三色标记法**: 通过白、灰、黑三色标记对象，精准追踪存活对象。
