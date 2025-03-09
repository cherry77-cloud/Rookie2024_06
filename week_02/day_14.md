## 文件操作（打开、关闭与读取）

### 1. 文件的存储与分类
文件是存储在外部介质（如磁盘）上的数据集合。
- 文本文件：以可读的字符形式存储数据（如 .txt、.go 文件）。
- 二进制文件：以二进制形式存储数据（如 .exe、.png 文件）。
---
### 2. 文件打开与关闭文件
- 使用 `os.Open()` 打开文件，返回一个 `*os.File` 对象和一个错误信息。
- 打开文件后，必须调用 `Close()` 方法关闭文件，以释放资源。
- 为了防止忘记关闭文件，通常使用 `defer` 语句注册文件关闭操作。
```go
func main() {
    file, err := os.Open("./main.go") // 打开文件
    if err != nil {
        fmt.Println("open file failed!, err:", err)
        return
    }
    defer file.Close() // 确保文件关闭
}
```
---
### 3. file.Read()
- 定义：`func (f *File) Read(b []byte) (n int, err error)`
- 接收一个字节切片，返回读取的字节数和可能的错误。
- 当读到文件末尾时，返回 `io.EOF` 错误。
```go
func main() {
    file, err := os.Open("./main.go")
    if err != nil {
        fmt.Println("open file failed!, err:", err)
        return
    }
    defer file.Close()

    var tmp = make([]byte, 128) // 创建一个缓冲区
    n, err := file.Read(tmp)   // 读取数据
    if err == io.EOF {
        fmt.Println("文件读完了")
        return
    }
    if err != nil {
        fmt.Println("read file failed, err:", err)
        return
    }
    fmt.Printf("读取了%d字节数据\n", n)
    fmt.Println(string(tmp[:n])) // 打印读取的内容
}
```
循环读取文件
```go
func main() {
    file, err := os.Open("./main.go")
    if err != nil {
        fmt.Println("open file failed!, err:", err)
        return
    }
    defer file.Close()

    var content []byte          // 存储文件全部内容
    var tmp = make([]byte, 128) // 缓冲区
    for {
        n, err := file.Read(tmp) // 读取数据
        if err == io.EOF {
            fmt.Println("文件读完了")
            break
        }
        if err != nil {
            fmt.Println("read file failed, err:", err)
            return
        }
        content = append(content, tmp[:n]...) // 将读取的数据追加到 content
    }
    fmt.Println(string(content)) // 打印文件全部内容
}
```
---
