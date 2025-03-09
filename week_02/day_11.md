## Go语言包（Package）知识点总结

### 一、包的定义与结构

包的作用
- 支持代码模块化与复用，组织代码结构。
- 每个包对应一个文件夹，文件夹内所有.go文件归属同一包。
- main包是程序入口，编译为可执行文件；非main包编译为库文件。
```go
package packagename  // 文件首行声明归属包，包名不必与文件夹名一致
```
命名规则
- 包名避免使用-符号，建议与功能相关（如utils、logger）。
- 同一文件夹下的文件必须属于同一包。

### 二、标识符可见性
导出规则
- 首字母大写：标识符对外可见（如变量、函数、结构体字段）。
- 首字母小写：标识符仅包内可见（私有）。
- 结构体字段名首字母大写才可被外部包访问。

```go
// 示例：包内定义
var privateVar int      // 不可导出
const PublicConst = 100 // 可导出
type privateStruct struct{} 
func PublicFunc() {}
```

```go
type User struct {
    Name string  // 可导出
    age  int     // 不可导出
}
```

### 三、包的导入（Import）

```go
import "fmt"                     // 默认包名
import alias "path/to/package"   // 别名导入
import _ "path/to/package"       // 匿名导入（仅执行init函数）
```

```go
import (
    "fmt"
    "os"
    alias "github.com/xxx/pkg"
)
```

- 禁止循环导入：包A导入包B，包B不能反向导入包A。
- 未使用的包：会触发编译错误。
- 匿名导入：常用于初始化数据库驱动等场景（如_ "github.com/go-sql-driver/mysql"）。

### 四、初始化函数 init()

- 无参数、无返回值，不可手动调用。
- 每个`.go`文件可定义多个`init()`，按声明顺序执行。
- 执行顺序：依赖包的init() -> 当前包的全局变量初始化 -> 当前包的init()

``` go
package main
import "fmt"
var x = 10
func init() { fmt.Println("init:", x) }
func main() { fmt.Println("main") }
```

### 五、依赖管理（Go Module）
- go.mod：定义模块路径、依赖及版本。
- go.sum：记录依赖的哈希校验值。
- 语义化版本（SemVer）：v1.2.3（主版本.次版本.修订号）。
- 主版本变更（如v2+）需在模块路径中体现：module github.com/xxx/mymodule/v2。

```bash
go mod init <module-name>    # 初始化模块
go mod tidy                 # 自动添加/移除依赖
go get <package>@<version>  # 添加依赖（如go get github.com/gin-gonic/gin@v1.9.1）
go list -m all              # 查看所有依赖
```
