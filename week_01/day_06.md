# Go 语言基础语法指南

### 标识符规则
- 组成元素：字母/数字/`_`
- 必须以`字母`或`_`开头
- 有效示例：`userName`, `_id`, `MAX_SIZE`

### 关键字列表 (25个)
```go
break    default      func    interface  select
case     defer        go      map        struct
chan     else         goto    package    switch
const    fallthrough  if      range      type
continue for          import  return     var
```

### 保留字列表
```go
Constants:   true  false  iota  nil

Types:       int  int8  int16  int32  int64  
             uint  uint8  uint16  uint32  uint64  uintptr
             float32  float64  complex128  complex64
             bool  byte  rune  string  error

Functions:   make  len  cap  new  append  copy  close  delete
             complex  real  imag panic  recover
```
