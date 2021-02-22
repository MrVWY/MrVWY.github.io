---
title: 通过函数计算搭建wordpress(二)
tags: []
id: '513'
categories:
  - - 部署
comments: false
date: 2020-09-28 22:08:52
---

在上文通过fun工具去创建了wordpress的函数计算，但是在实际安装中也遇到了不少的坑，在此记录下来。
<!-- more -->
### 前言

经过上一篇文章，wordpress的函数计算已经成功的部署在我的阿里云上，但是操作比较复杂，你可以使用傻瓜式部署方法选择其上面的。

### 问题

当部署成功之后，正准备install wordpress，结果来了一个tables prefix must not be empty的错误。网上的做法是通过修改wordpress目录下的wp-config.php文件。这一步涉及到的问题便大了。 一方面是不同地域的ecs和nas是不能直接连接挂载，如果要跨地域挂载，就要交钱......，但网上的说法都是直接复制粘贴阿里云nas文档的命令，完全忽略了这个跨地域问题。对于挂载点，你可以通过阿里云上的专有网络VPC查看你的ecs在哪个网络，接着通过添加挂载点让你的ecs挂载上nas，前提是同一地域。 另一方面就是不知道部署上去的数据库的相关配置在哪，为此我还专门开了一个云数据库来进行配置。 最后本人买了一个腾讯云上的云数据库，通过修改wp-config.php的内容使worpdress连上数据库进行install。

### Reference

1.  [nas文档](https://help.aliyun.com/document_detail/90529.html?spm=a2c4g.11174283.6.568.7cc34da2KwgQuJ)