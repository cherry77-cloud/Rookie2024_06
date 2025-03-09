### Go 语言中的 map
`map` 是 `Go` 语言中提供的一种基于键值对(`key-value`)的无序集合，其内部通过散列表(`hash table`)实现。`map` 是引用类型，必须初始化后才能使用

#### 一. map 的定义
```go
// KeyType：键的类型   ValueType：值的类型
map[KeyType]ValueType

// map 的默认初始值为 nil，需要使用 make() 函数分配内存：
make(map[KeyType]ValueType, [cap])
// cap：可选参数，表示 map 的初始容量。
```

2. map 的基本使用
示例
go
复制
func main() {
    scoreMap := make(map[string]int, 8)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    fmt.Println(scoreMap)          // 输出：map[小明:100 张三:90]
    fmt.Println(scoreMap["小明"])  // 输出：100
    fmt.Printf("type of a:%T\n", scoreMap) // 输出：type of a:map[string]int
}
声明时填充元素
go
复制
func main() {
    userInfo := map[string]string{
        "username": "沙河小王子",
        "password": "123456",
    }
    fmt.Println(userInfo) // 输出：map[username:沙河小王子 password:123456]
}
3. 判断某个键是否存在
语法
go
复制
value, ok := map[key]
如果 key 存在，ok 为 true，value 为对应的值。

如果 key 不存在，ok 为 false，value 为值类型的零值。

示例
go
复制
func main() {
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100

    v, ok := scoreMap["张三"]
    if ok {
        fmt.Println(v) // 输出：90
    } else {
        fmt.Println("查无此人")
    }
}
