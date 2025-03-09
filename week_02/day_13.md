## time 包

### 1. 时间类型
- `Go` 语言中使用 `time.Time` 类型表示时间。
- 通过 `time.Now()` 可以获取当前时间对象，并从中提取年、月、日、时、分、秒等信息。
```go
func timeDemo() {
    now := time.Now() // 获取当前时间
    fmt.Printf("current time:%v\n", now)

    year := now.Year()     // 年
    month := now.Month()   // 月
    day := now.Day()       // 日
    hour := now.Hour()     // 小时
    minute := now.Minute() // 分钟
    second := now.Second() // 秒
    fmt.Println(year, month, day, hour, minute, second)
}
```
---
### 2. 时区处理
- `Go` 语言使用 `time.Location` 来表示时区。
- 时区是根据全球不同地区的经度划分的，中国通常使用东八区（北京时间）。
- 可以通过 `time.FixedZone` 创建一个固定偏移量的时区，或者使用 `time.LoadLocation` 加载系统时区数据库中的时区。
```go
func timezoneDemo() {
    // 东八区（北京时间）的固定偏移量
    secondsEastOfUTC := int((8 * time.Hour).Seconds())
    beijing := time.FixedZone("Beijing Time", secondsEastOfUTC)

    // 加载纽约时区
    newYork, err := time.LoadLocation("America/New_York") // UTC-05:00
    if err != nil {
        fmt.Println("load America/New_York location failed", err)
        return
    }

    // 创建时间对象
    timeInUTC := time.Date(2009, 1, 1, 12, 0, 0, 0, time.UTC)
    sameTimeInBeijing := time.Date(2009, 1, 1, 20, 0, 0, 0, beijing)
    sameTimeInNewYork := time.Date(2009, 1, 1, 7, 0, 0, 0, newYork)

    // 比较时间是否相同
    fmt.Println(timeInUTC.Equal(sameTimeInBeijing)) // true
    fmt.Println(timeInUTC.Equal(sameTimeInNewYork)) // true
}
```
- 时区信息在时间处理中非常重要，尤其是在跨时区应用中。
- `time.LoadLocation` 依赖于系统的时区数据库，如果系统没有安装时区数据库，可能会导致加载失败。
- 在不明确程序运行环境的情况下，建议使用 `time.FixedZone` 来定义时区偏移量，避免依赖系统时区数据库。
---

### 3. Unix 时间
`Unix` 时间是从 1970 年 1 月 1 日 `00:00:00 UTC` 开始计算的秒数。`Go` 语言提供了多种方法获取 `Unix` 时间戳：
- `Unix()`：秒级时间戳。
- `UnixMilli()`：毫秒级时间戳
- `UnixMicro()`：微秒级时间戳
- `UnixNano()`：纳秒级时间戳
```go
func timestampDemo() {
    now := time.Now()
    fmt.Println(now.Unix())      // 秒级时间戳
    fmt.Println(now.UnixMilli()) // 毫秒级时间戳
    fmt.Println(now.UnixMicro()) // 微秒级时间戳
    fmt.Println(now.UnixNano())  // 纳秒级时间戳
}

func timestamp2Time() {
    t := time.Date(2022, 02, 22, 22, 22, 22, 22, time.UTC)
    sec := t.Unix()
    msec := t.UnixMilli()
    usec := t.UnixMicro()

    fmt.Println(time.Unix(sec, 22))      // 秒级转时间对象
    fmt.Println(time.UnixMilli(msec))    // 毫秒级转时间对象
    fmt.Println(time.UnixMicro(usec))    // 微秒级转时间对象
}

// 时间间隔
const (
    Nanosecond  Duration = 1
    Microsecond          = 1000 * Nanosecond
    Millisecond          = 1000 * Microsecond
    Second               = 1000 * Millisecond
    Minute               = 60 * Second
    Hour                 = 60 * Minute
)
```
---
### 4. 时间操作
```go
// func (t Time) Add(d Duration) Time：在时间对象上增加一个时间间隔
func main() {
    now := time.Now()
    later := now.Add(time.Hour) // 当前时间加 1 小时
    fmt.Println(later)
}

// func (t Time) Sub(u Time) Duration：计算两个时间点之间的差值。
func main() {
    t1 := time.Now()
    t2 := t1.Add(time.Hour)
    diff := t2.Sub(t1) // 1 小时
    fmt.Println(diff)
}

// func (t Time) Equal(u Time) bool：判断两个时间是否相同，考虑时区。
func main() {
    t1 := time.Now()
    t2 := t1.Add(0)
    fmt.Println(t1.Equal(t2)) // true
}

// func (t Time) Before(u Time) bool：判断时间 t 是否在时间 u 之前
func main() {
    t1 := time.Now()
    t2 := t1.Add(time.Hour)
    fmt.Println(t1.Before(t2)) // true
}

// func (t Time) After(u Time) bool：判断时间 t 是否在时间 u 之后。
func main() {
    t1 := time.Now()
    t2 := t1.Add(-time.Hour)
    fmt.Println(t1.After(t2)) // true
}
```
