## Go 语言切片(Slice)

### 一、切片引入背景

- **数组的局限性**: 数组的长度是固定的并且数组长度属于类型的一部分
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
