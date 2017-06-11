# timewheel
Golang实现的时间轮


![时间轮](https://raw.githubusercontent.com/ouqiang/timewheel/master/timewheel.jpg)

# 原理
轮中的实线指针指向轮子上的一个槽（slot），它以恒定的速度顺时针转动，每转动一步就指向下一个槽，每次转动称为一个滴答（tick）。

一个滴答的时间称为是间轮的槽间隔si（slot interval），它实际上就是心跳时间。

该轮共有N个槽，因此它运转一周的时间是N×si 。每个槽指向一条定时器链表，每条链表上的定时器具有相同的特性：它们的定时时间相差N×si的整数倍。时间轮正是利用这个关系将定时器散列到不同的链表中。

假如现在指针指向槽cs，我们要添加一个定时时间为ti的定时器，则该定时器将被插入ts（timer slot）对应的链表中：ts = (cs + (ti / si)) %N

基于排序链表的定时器使用唯一的一条链表来管理所有定时器，所以插入操作的效率随着定时器数目的增多而降低。而时间轮使用哈希表的思想，将定时器散列到不同的链表上。

这样每条链表上的定时数目都将明显减少，插入操作的效率受定时器数目的影响较少。

很显然，对时间轮而言，要提高定时精度，就要使si值足够小；要提高执行效率，则要求N值足够大。 
 

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

