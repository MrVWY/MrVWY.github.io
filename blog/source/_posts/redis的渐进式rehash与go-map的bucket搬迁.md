---
title: redis的渐进式rehash与Go Map的bucket搬迁
tags: []
id: '350'
categories:
  - - 杂谈
comments: false
date: 2020-07-31 17:30:37
---

redis的在处理哈希表的扩容和缩容引入了一个渐进式rehash机制，其中该机制和Go中的map里的bucket搬迁有一定的相似。
<!-- more -->
### redis的渐进式rehash

redis的字典的底层数据结构主要是哈希表，其采用链地址法来解决键冲突的问题。而当哈希表所保存的键值过多或者过少时，就会触发rehash机制。通过再初始化一个比原来大一倍的空间，最后将原哈希表的键值放到新哈希表中。 渐进式rehash所解决的问题就是当原哈希表的键值过于庞大，如果一次性的迁移会导致cpu，内存等使用率急剧上升，因此为了不影响服务器性能，渐进式rehash就采用一次只迁移一部分键值的方式，慢慢的将原哈希表的键值迁移到新的哈希表。

### Go Map的bucket搬迁

关于bucket的搬迁，只要还是看源码里的evacuate函数，关键点对在于hmap结构的nevacuate字段计数(h.nevacuate++)，最后通过对比是否到达了newbit来完成bucket的搬迁。 具体详细源码可以去runtime/map.go查看，就不一一分析。

### 采取该机制的作用

主要作用是防止哈希表的键值过多，一次性搬迁所引发的性能问题。而再搬迁过程中，遇到了查找或者更新操作就先遍历旧再遍历新，而遇到了插入操作就直接去新的哈希表里去插入。

### Reference

*   [Redis的设计与实现](http://redisbook.com/preview/dict/incremental_rehashing.html)
*   [runtime/map.go](https://github.com/golang/go/blob/f92337422ef2ca27464c198bb3426d2dc4661653/src/runtime/map.go#L1128)