---
title: Go Sort Library Common functions
date: 2021-02-28 11:05:18
categories:
  - - Go
comments: false
tags:
---

#### IntsAreSorted

```go
func IntsAreSorted(a []int) bool
```

IntsAreSorted检查a是否已排序为递增顺序

#### Reverse

Reverse包装一个Interface接口并返回一个新的Interface接口，对该接口排序可生成递减序列。

```go
s := []int{5, 2, 6, 3, 1, 4} // unsorted
sort.Sort(sort.Reverse(sort.IntSlice(s)))
fmt.Println(s) //[6 5 4 3 2 1]
```

#### Search

```go
func Search(n int, f func(int) bool) int
```

Search函数采用二分法搜索找到[0, n)区间内最小的满足f(i)==true的值i。

例如在一个递增顺序的整数切片中找到值x

```go
x := 23
i := sort.Search(len(data), func(i int) bool { return data[i] >= x })
if i < len(data) && data[i] == x {
	// x is present at data[i]
} else {
	// x is not present in data,
	// but i is the index where it would be inserted.
}
```

#### StringsAreSorted

```go
func StringsAreSorted(a []string) bool
```

StringsAreSorted检查a是否已排序为递增顺序。

#### SearchStrings

```go
func SearchStrings(a []string, x string) int
```

SearchStrings在递增顺序的a中搜索x，返回x的索引。如果查找不到，返回值是x应该插入a的位置（以保证a的递增顺序），返回值可以是len(a)。