# timewheel
Golang实现的时间轮


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
    "fmt"
)

func main()  {
    // tick刻度为1秒, 3600个槽, 执行的job
    tw := timewheel.New(1 * time.Second, 3600, func(data []interface{}) {
        fmt.Println(data)
        // do something
    })
    tw.Start()
    tw.Add(5 * time.Second, []interface{}{1})
    tw.Add(10 * time.Minute, []interface{}{2})
    tw.Add(35 * time.Hour, []interface{}{3})
    // 停止
    tw.Stop()
}
```

