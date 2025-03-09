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

### 4. bufio读取文件
- `bufio` 包在 `os.File` 的基础上封装了一层 `API`，提供了更高效、更便捷的文件读取功能
- `bufio.Scanner`：用于逐行读取文件内容。默认缓冲区大小为 `64 * 1024` 字节（`64KB`）
```go
func main() {
    file, err := os.Open("./xx.txt") // 打开文件
    if err != nil {
        fmt.Println("open file failed, err:", err)
        return
    }
    defer file.Close()

    scanner := bufio.NewScanner(file) // 创建 Scanner 对象
    for scanner.Scan() {              // 逐行读取
        fmt.Println(scanner.Text())   // 打印每一行内容
    }
    if err := scanner.Err(); err != nil { // 检查错误
        fmt.Println("read file failed, err:", err)
    }
}
```
---

### 5. os.ReadFile
```go
package main

import (
	"fmt"
	"os"
)

// os.ReadFile 读取整个文件
func main() {
	content, err := os.ReadFile("./main.go")
	if err != nil {
		fmt.Println("read file failed, err:", err)
		return
	}
	fmt.Println(string(content))
}
```
---

### 6. 写入文件
- `os.OpenFile()`函数能够以指定模式打开文件，从而实现文件写入相关功能。
- `os.O_WRONLY`	只写   `os.O_CREATE`  创建文件     `os.O_RDONLY`	只读
- `os.O_RDWR`	读写   `os.O_TRUNC`	  清空        `os.O_APPEND`	追加
```go
func OpenFile(name string, flag int, perm FileMode) (*File, error) {
	...
}

// Write 和 WriteString
func main() {
	file, err := os.OpenFile("xx.txt", os.O_CREATE|os.O_TRUNC|os.O_WRONLY, 0666)
	if err != nil {
		fmt.Println("open file failed, err:", err)
		return
	}
	defer file.Close()
	str := "hello 沙河"
	file.Write([]byte(str))       //写入字节切片数据
	file.WriteString("hello 小王子") //直接写入字符串数据
}

// os.WriteFile
func writeFile() {
	str := "hello 沙河"
	err := os.WriteFile("./xx.txt", []byte(str), 0666)
	if err != nil {
		fmt.Println("write file failed, err:", err)
		return
	}
}

// bufio.NewWriter
func main() {
	file, err := os.OpenFile("xx.txt", os.O_CREATE|os.O_TRUNC|os.O_WRONLY, 0666)
	if err != nil {
		fmt.Println("open file failed, err:", err)
		return
	}
	defer file.Close()
	writer := bufio.NewWriter(file)
	for i := 0; i < 10; i++ {
		writer.WriteString("hello沙河\n") //将数据先写入缓存
	}
	writer.Flush() //将缓存中的内容写入文件
}
```

---

```go
// CopyFile 拷贝文件函数
func CopyFile(dstName, srcName string) (written int64, err error) {
	// 以读方式打开源文件
	src, err := os.Open(srcName)
	if err != nil {
		fmt.Printf("open %s failed, err:%v.\n", srcName, err)
		return
	}
	defer src.Close()
	// 以写|创建的方式打开目标文件
	dst, err := os.OpenFile(dstName, os.O_WRONLY|os.O_CREATE, 0644)
	if err != nil {
		fmt.Printf("open %s failed, err:%v.\n", dstName, err)
		return
	}
	defer dst.Close()
	return io.Copy(dst, src) //调用io.Copy()拷贝内容
}
func main() {
	_, err := CopyFile("dst.txt", "src.txt")
	if err != nil {
		fmt.Println("copy file failed, err:", err)
		return
	}
	fmt.Println("copy done!")
}
```

```go
package main

import (
	"bufio"
	"flag"
	"fmt"
	"io"
	"os"
)

// cat命令实现
func cat(r *bufio.Reader) {
	for {
		buf, err := r.ReadBytes('\n') //注意是字符
		if err == io.EOF {
			// 退出之前将已读到的内容输出
			fmt.Fprintf(os.Stdout, "%s", buf)
			break
		}
		fmt.Fprintf(os.Stdout, "%s", buf)
	}
}

func main() {
	flag.Parse() // 解析命令行参数
	if flag.NArg() == 0 {
		// 如果没有参数默认从标准输入读取内容
		cat(bufio.NewReader(os.Stdin))
	}
	// 依次读取每个指定文件的内容并打印到终端
	for i := 0; i < flag.NArg(); i++ {
		f, err := os.Open(flag.Arg(i))
		if err != nil {
			fmt.Fprintf(os.Stdout, "reading from %s failed, err:%v\n", flag.Arg(i), err)
			continue
		}
		cat(bufio.NewReader(f))
	}
}
```
