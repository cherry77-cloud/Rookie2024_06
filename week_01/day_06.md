## Go 语言切片(Slice)

### 一、切片引入背景

**数组的局限性**: 数组的长度是固定的并且数组长度属于类型的一部分

```go
// 只能处理固定长度数组
func arraySum(x [3]int) int {
    sum := 0
    for _, v := range x {
        sum += v
    }
    return sum
}

a := [3]int{1, 2, 3}
// a[3] = 4  // 越界错误，无法扩展
```

---
### 二、切片定义与初始化
切片`Slice`是一个拥有相同类型元素的可变长度的序列。它是基于数组类型做的一层封装。它非常灵活，支持自动扩容。切片是一个引用类型，它的内部结构包含地址、长度和容量。切片一般用于快速地操作一块数据集合

```go
// 1. 基本定义
var s []T        // 声明切片类型
var a []string   // 字符串切片（nil切片）
b := []int{}     // 初始化整型空切片
c := []bool{false, true} // 初始化布尔切片

// 2. 空切片与nil切片
var s1 []int          // len=0, cap=0, s1 == nil
s2 := []int{}         // len=0, cap=0, s2 != nil
s3 := make([]int, 0)  // len=0, cap=0, s3 != nil

func main() {
	// 声明切片类型
	var a []string                  // 声明一个字符串切片
	var b = []int{}                 // 声明一个整型切片并初始化
	var c = []bool{false, true}     // 声明一个布尔切片并初始化
	var d = []bool{false, true}     // 声明一个布尔切片并初始化
	fmt.Println(c == d)             // 切片是引用类型，不支持直接比较，只能和nil比较
}
```
- 动态长度: 长度可自动扩展，支持动态增减元素
- 引用类型: 赋值时共享底层数组，修改会影响所有关联切片
- 三元结构: 包含指针(指向数组)、长度(`len`)和容量(`cap`)
- 自动扩容: 通过`append`操作触发扩容，按特定策略分配新内存

---

### 三、切片操作详解
```go
// 简单表达式
arr := [5]int{1,2,3,4,5}
s1 := arr[1:3]     // [2 3], len=2, cap=4
s2 := arr[2:]      // [3 4 5], len=3, cap=3

// 完整表达式
a[low:high:max]: max 限制新切片的容量为 max-low, 适用于动态控制容量
t := arr[1:3:5]    // [2 3], len=2, cap=4 (max-low=5-1=4)

// 动态创建
s := make([]int, 2, 10)   // len=2, cap=10
fmt.Println(s)            // [0 0]

// 遍历操作
for i := 0; i < len(s); i++ {
    fmt.Println(s[i])
}

for index, value := range s {
    fmt.Printf("%d:%v\n", index, value)
}
```
- 声明与初始化: 未初始化的切片为 `nil`，初始化的空切片非 `nil`
- 长度与容量: 容量决定了切片的扩展潜力
- 底层共享: 切片赋值或切割后共享底层数组，修改相互影响
- 使用 `len()` 判空，避免直接比较切片
- 动态创建用 `make()`，可指定长度和容量
