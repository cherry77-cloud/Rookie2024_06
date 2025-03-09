## Go 语言流程控制详解

### 一、分支结构

#### 1. `if-else` 语句

```go
if 表达式1 {
    分支1
} else if 表达式2 {
    分支2
} else {
    分支3
}


func ifDemo1() {
    score := 65
    if score >= 90 {
        fmt.Println("A")
    } else if score > 75 {
        fmt.Println("B")
    } else {
        fmt.Println("C")  // 输出: C
    }
}
```

#### 2. 特殊写法(带初始化语句)
- 带初始化语句的 `if` 中声明的变量作用域仅限于当前 `if` 代码块。

```go
if 初始化语句; 条件表达式 {
    // 代码块
}

func ifDemo2() {
    if score := 65; score >= 90 {  // score 作用域仅限于 if 代码块
        fmt.Println("A")
    } else {
        fmt.Println("C")  // 输出: C
    }
}
```
---

### 二、循环结构

#### 1. `for` 循环

```go
for 初始语句; 条件表达式; 结束语句 {
    循环体
}

func forDemo1() {
    for i := 0; i < 10; i++ {
        fmt.Println(i)  // 输出 0~9
    }
}

// 类似 while 循环
i := 0
for i < 10 {
    fmt.Println(i)
    i++
}

// 无限循环
for {
    // 循环体（需配合 break 退出）
}
```

#### 2. `for range`迭代

```go
// 遍历数组/切片/字符串
for index, value := range collection {}

// 遍历 map
for key, value := range map {}

// 遍历通道
for value := range channel {}

// Go 1.22+ 支持遍历整数
for i := range 5 {
    fmt.Println(i)  // 输出 0 1 2 3 4
}
```
---

### 三、多路分支结构

#### 1. `switch-case` 语句
```go
switch 变量 {
case 值1:
    // 代码块
case 值2, 值3:  // 多值匹配
    // 代码块
default:
    // 默认代码块
}

func switchDemo1() {
    finger := 3
    switch finger {
    case 3:
        fmt.Println("中指")  // 输出: 中指
    default:
        fmt.Println("无效输入")
    }
}
```


- 表达式判断
```go
switch {
case 条件表达式1:
    // 代码块
case 条件表达式2:
    // 代码块
}

func switchDemo2() {
    age := 30
    switch {
    case age > 25 && age < 35:
        fmt.Println("好好工作吧")  // 输出
    }
}
```

- `fallthrough` 穿透
```go
switch {
case s == "a":
    fmt.Println("a")
    fallthrough  // 继续执行下一个 case
case s == "b":
    fmt.Println("b")  // 会被执行
}
```

---

### 四、跳转控制

#### 1. `goto` 语句
```go
func gotoDemo2() {
    for i := 0; i < 10; i++ {
        for j := 0; j < 10; j++ {
            if j == 2 {
                goto breakTag  // 跳转到标签
            }
        }
    }
breakTag:
    fmt.Println("结束循环")
}
```

#### 2. `break` 与 `continue`
```go
// 带标签的 break
BREAKDEMO:
for i := 0; i < 10; i++ {
    break BREAKDEMO  // 直接跳出外层循环
}

// 带标签的 continue
CONTINUEDEMO:
for i := 0; i < 5; i++ {
    for j := 0; j < 5; j++ {
        continue CONTINUEDEMO  // 跳到外层循环
    }
}
```

---

### 关键总结

| 控制结构       | 特点                                                                 |
|----------------|----------------------------------------------------------------------|
| `if-else`      | 支持初始化语句，变量作用域限于代码块                                 |
| `for`          | 可省略初始/结束语句，支持无限循环                                   |
| `for range`    | 支持数组/切片/字符串/map/通道遍历，Go 1.22+ 支持整数迭代             |
| `switch-case`  | 支持多值匹配、表达式判断和 `fallthrough` 穿透                       |
| `goto`         | 需配合标签使用，适合快速跳出多层嵌套                                |
| `break`        | 可指定标签跳出特定循环                                              |
| `continue`     | 可指定标签跳过当前循环迭代                                          |

---

- 在 Go 中是语句而非运算符，不能嵌入表达式（如 `a = b++` 非法）
- 每个 `switch` 语句只能有一个 `default` 分支
- 遍历数组、切片、字符串、map 和通道时，优先使用 `for range`
