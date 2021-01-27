---
title: Introduction
type: docs
---

# 关于我

{{< columns >}}
## 一个中年码农的自白。

要好好学习啊

<--->

## 我有两只猫

一只叫小新，一只叫妮妮。
{{< /columns >}}


## 好好学习编程

目前在使用 **Golang** ，现在的面试题可变态了。怎么让下面的大小写分别输出。

    package main

    import (
        "fmt"
        "sync"
    )

    const N  = 26

    func main()  {
        const GOMAXPROCS  = 1
        runtime.GOMAXPROCS(GOMAXPROCS)

        var wg sync.WaitGroup
        wg.Add( 2*N)

        for i:=0; i<N; i++ {
            go func(i int) {
                defer wg.Done()
                fmt.Printf("%c", 'a'+i)
            }(i)
            go func(i int) {
                defer wg.Done()
                fmt.Printf("%c", 'A'+i)
            }(i)

        }
        wg.Wait()
    }


## 如何开心的过好每一天

路漫漫其修远兮
