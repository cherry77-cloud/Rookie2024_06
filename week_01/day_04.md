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
```
---
```go
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
```
---
```go
func ifDemo2() {
    if score := 65; score >= 90 {  // score 作用域仅限于 if 代码块
        fmt.Println("A")
    } else {
        fmt.Println("C")  // 输出: C
    }
}
```


### 二、循环结构

#### 1. `for` 循环

```go
for 初始语句; 条件表达式; 结束语句 {
    循环体
}
```
---
```go
func forDemo() {
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
```go


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
```
