---
title: Why is my nil error value not equal to nil?
tags: []
id: '501'
categories:
  - - Go
comments: false
date: 2020-09-25 19:15:30
---



先来看一段代码

```go
func foo() error {
    var err *os.PathError = nil
    return err
}

func main(){
   err := foo()
   fmt.Println(err == nil) //false
   fmt.Println(err == (*os.PathError)(nil)) //true
}
```



### why nil is not nil?

前提：要了解go中的interface内部构造，分清楚空接口和非空接口。可以参考🔗[链接](http://www.hizjlhi.com/archives/354)。总的来说还是牵涉到interface的动静类型相关。 在上述代码中，通过foo函数返回的err值在非空接口中可以表示为\[nil，\*os.PathError\]，其与nil(\[nil，nil\])必然不相等。 而 (\*os.PathError)(nil) 的写法实际上等于 var a \*os.PathErrot = nil。这种 (\*os.PathError)(nil) 写法就比较少见。主要还是interface的动静态类型要区分。

### 回想

现在回想起来，当时问我两个nil比较是否相等，那时一脸懵逼。回想起来问题的本质应该是要从上述所讲的方面来出发来说明两个nil比较是否相等。

### Reference

1.  [Go Gotchas](https://yourbasic.org/golang/gotcha-why-nil-error-not-equal-nil/)
2.  [Frequently Asked Questions (FAQ) in Go offical website](https://golang.org/doc/faq#nil_error)
3.  [Understanding nil](https://www.youtube.com/watch?v=ynoY2xz-F8s)