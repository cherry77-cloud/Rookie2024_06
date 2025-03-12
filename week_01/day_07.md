## `Go` 语言中的 `map`
`map` 是 `Go` 语言中提供的一种基于键值对(`key-value`)的无序集合，其内部通过散列表(`hash table`)实现。`map` 是引用类型，必须初始化后才能使用

### 一. `map` 的定义
```go
// KeyType：键的类型   ValueType：值的类型
map[KeyType]ValueType

// map 的默认初始值为 nil，需要使用 make() 函数分配内存：
make(map[KeyType]ValueType, [cap])
// cap：可选参数，表示 map 的初始容量。
```
---
### 二. `map` 的基本使用
```go
func main() {
    scoreMap := make(map[string]int, 8)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    fmt.Println(scoreMap)          // 输出：map[小明:100 张三:90]
    fmt.Println(scoreMap["小明"])  // 输出：100
    fmt.Printf("type of a:%T\n", scoreMap) // 输出：type of a:map[string]int
}

func main() {
    userInfo := map[string]string{
        "username": "沙河小王子",
        "password": "123456",
    }
    fmt.Println(userInfo) // 输出：map[username:沙河小王子 password:123456]
}
```
---

### 三. 判断某个键是否存在
```go
value, ok := map[key]
// 如果 key 存在，ok 为 true，value 为对应的值。
// 如果 key 不存在，ok 为 false，value 为值类型的零值。

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
```
---
### 四. `map` 的遍历
```go
// 遍历键值对
func main() {
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    scoreMap["娜扎"] = 60

    for k, v := range scoreMap {
        fmt.Println(k, v)
    }
}

// 只遍历键
func main() {
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    scoreMap["娜扎"] = 60

    for k := range scoreMap {
        fmt.Println(k)
    }
}

// 遍历顺序：map 是无序的，遍历时的顺序与添加键值对的顺序无关。
```
---

### 五. 使用 `delete()` 函数删除键值对
```go
func main() {
    scoreMap := make(map[string]int)
    scoreMap["张三"] = 90
    scoreMap["小明"] = 100
    scoreMap["娜扎"] = 60

    delete(scoreMap, "小明") // 删除键为 "小明" 的键值对

    for k, v := range scoreMap {
        fmt.Println(k, v)
    }
}
```
---

### 六. 按照指定顺序遍历 `map`
```go
func main() {
    rand.Seed(time.Now().UnixNano()) // 初始化随机数种子

    var scoreMap = make(map[string]int, 200)

    for i := 0; i < 100; i++ {
        key := fmt.Sprintf("stu%02d", i) // 生成 stu 开头的字符串
        value := rand.Intn(100)          // 生成 0~99 的随机整数
        scoreMap[key] = value
    }

    // 取出 map 中的所有 key 存入切片 keys
    var keys = make([]string, 0, 200)
    for key := range scoreMap {
        keys = append(keys, key)
    }

    // 对切片进行排序
    sort.Strings(keys)

    // 按照排序后的 key 遍历 map
    for _, key := range keys {
        fmt.Println(key, scoreMap[key])
    }
}
```
---
### 七. 元素为 `map` 类型的切片
```go
func main() {
    var mapSlice = make([]map[string]string, 3) // 声明一个切片，元素为 map

    for index, value := range mapSlice {
        fmt.Printf("index:%d value:%v\n", index, value)
    }

    fmt.Println("after init")

    // 对切片中的 map 元素进行初始化
    mapSlice[0] = make(map[string]string, 10)
    mapSlice[0]["name"] = "小王子"
    mapSlice[0]["password"] = "123456"
    mapSlice[0]["address"] = "沙河"

    for index, value := range mapSlice {
        fmt.Printf("index:%d value:%v\n", index, value)
    }
}
```
---
### 八. 值为切片类型的 `map`
```go
func main() {
    var sliceMap = make(map[string][]string, 3) // 声明一个 map，值为切片类型

    fmt.Println(sliceMap)
    fmt.Println("after init")

    key := "中国"
    value, ok := sliceMap[key]
    if !ok {
        value = make([]string, 0, 2) // 初始化切片
    }

    value = append(value, "北京", "上海")
    sliceMap[key] = value

    fmt.Println(sliceMap)
}
```
---
- 无序性：`map` 中的键值对是无序的
- 引用类型：`map` 是引用类型，必须初始化后才能使用
- 动态扩容：`map` 会根据需要自动扩容
- 添加/修改：直接赋值，如 `map[key] = value`
- 删除：使用 `delete(map, key)`
- 判断键是否存在：使用 `value, ok := map[key]`
- 零值：未初始化的 `map` 为 `nil`，不能直接使用
- 遍历顺序：`map` 的遍历顺序是随机的
- `map` 的查找、插入和删除操作的平均时间复杂度为 `O(1)`
