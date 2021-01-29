---
title: 关于XDEBUG_SESSION_START
tags: []
id: '337'
categories:
  - - 杂谈
comments: false
date: 2020-08-01 00:05:59
---

最近看到博客上居然有条连接请求到30多次，而且还是一段时间内请求的，这引起了我的注意。
<!-- more -->
### 问题来源

问题来自于博客统计到一段时间内有30几条请求的路径全都是“/?XDEBUG\_SESSION\_START=phpstorm”。从后台点进去之后发现进去的页面上面还带有管理员的操作栏，这下我慌了。

### 过程

首先从Google上了解到，xebug是php的一个扩展功能，具体用来调试代码的。但继续Google之后，居然发现了关于xebug的渗透攻击，这下可不能不管了。想从xebug的官方文档入手去修改php.ini。

#### 要点

同时还发现如果你浏览器还存在session的话，还可以通过在网址后面加上这条路径，就会之间**绕过**登陆页面直通后台。因此这也是日常使用的一个要点。

### 解决方案

目前解决方案主要是修改php.ini或者封了这条访问路径。

### References

*   [xebug攻击](https://blog.spoock.com/2017/09/19/xdebug-attack-surface/)
*   [xebug文档](https://xdebug.org/)