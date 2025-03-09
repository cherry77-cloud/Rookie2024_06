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
