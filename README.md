# timewheel
Golang实现的时间轮

[![Go Report Card](https://goreportcard.com/badge/github.com/ouqiang/timewheel)](https://goreportcard.com/report/github.com/ouqiang/timewheel)

![时间轮](https://raw.githubusercontent.com/ouqiang/timewheel/master/timewheel.jpg)

# 安装

```shell
go get -u github.com/ouqiang/timewheel
```

# 使用

```go
package main

import (
    "github.com/ouqiang/timewheel"
    "time"
)

func main()  {
    // 初始化时间轮
    // 第一个参数为tick刻度, 即时间轮多久转动一次
    // 第二个参数为时间轮槽slot数量
    // 第三个参数为回调函数
    tw := timewheel.New(1 * time.Second, 3600, func(data interface{}) {
        // do something
    })
    
    // 启动时间轮
    tw.Start()
    
    // 添加定时器 
    // 第一个参数为延迟时间
    // 第二个参数为定时器唯一标识, 删除定时器需传递此参数
    // 第三个参数为用户自定义数据, 此参数将会传递给回调函数, 类型为interface{}
    tw.AddTimer(5 * time.Second, conn, map[string]int{"uid" : 105626})
    
    // 删除定时器, 参数为添加定时器传递的唯一标识
    tw.RemoveTimer(conn)
    
    // 停止时间轮
    tw.Stop()
    
    select{}
}
```

