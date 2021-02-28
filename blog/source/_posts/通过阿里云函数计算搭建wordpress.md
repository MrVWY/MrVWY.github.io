---
title: 通过函数计算搭建wordpress(一)
tags: []
id: '484'
categories:
  - - 部署
comments: false
date: 2020-09-24 16:02:58
---

### 所需工具

1.  nodejs和fun工具(阿里巴巴自创)，同时建议nodejs要安装最新版，把nodejs安装好之后就可以通过npm install @alicloud/fun -g来安装fun工具，文档链接为[fun](https://github.com/alibaba/funcraft/blob/master/docs/usage/installation-zh.md?spm=a2c4g.11186623.2.45.4616fc78j3Sr6U&file=installation-zh.md)
2.  在阿里云上设置你的AccessKey(如果有这步自行跳过)
3.  准备好你的域名，然后解析到阿里云的[计算节点域名](https://help.aliyun.com/document_detail/52984.html?spm=5176.11065259.1996646101.searchclickresult.1c642ae2Kdcg3k)上。
4.  准备php环境。
5.  准备git工具

### 配置

#### 1、clone工程

git clone https://github.com/awesome-fc/fc-wordpress.git

#### 2、选择你所要部署的数据库

通过解压fc-wordpress之后会有2种数据库给你选择，分别是sqlite和mysql(自行决定)，在此本人选择了mysql进行操作。 .env文件：使用命令cp .env\_example .env，查看ls -a。配置解析：

```
DEFAULT_REGION=cn-qingdao(这里本人选择青岛的计算节点)
ACCOUNT_ID=你阿里云的账户ID
ENDPOINT=http://你阿里云的账户ID.cn-qingdao.fc.aliyuncs.com
ACCESS_KEY_ID=你阿里云的AccessKey ID
ACCESS_KEY_SECRET=你阿里云的AccessKey Secret
```

初始化：

```
fun nas init
fun nas info
```

接着通过上面步骤之后就会自动创建一个文件目录/fc-wordpress/fc-web-mysql/.fun/nas/auto-default/fc-wp-mysql/wordpress，你可以通过php -S 0.0.0.0：80来测试一下是否出错，同时要自行创建一个管理员。 把wordpress上传到nas

```
func nas sync
fun nas ls nas:///mnt/auto/  #检查是否同步成功
```



#### 最后一步(有坑)

1.  修改fc-web-mysql下的index.php文件中的$host参数中的值为你的域名。
2.  修改fc-web-mysql下的template.yml文件中的字段fc-wp-demo(名字要独一无二)，注意这一步(本人当时就卡在这，因为官方的文件说的是修改 template.yml LogConfig 中的 Project, 任意取一个不会重复的名字即可)。
3.  修改fc-web-mysql下的template.yml文件中的 fc-wordpress-domain->Properties->DomainName中的值为你的域名
4.  其他的应该只是一些注释和描述，本人没细看，主要还是下面打红字的地方

```
Transform: 'Aliyun::Serverless-2018-04-03' 
Resources: 
  fc-wp-mysql: 
    Type: 'Aliyun::Serverless::Service' 
    Properties: 
      Description: 'run wordpress on FC' 
      NasConfig: Auto 
      LogConfig: 
        Project: fc-wp-demo
        Logstore: mysql-log 
..............
fc-wp-demo: 
  Type: 'Aliyun::Serverless::Log' 
  Properties: 
    Description: 'fc web log project' 
  mysql-log: 
    Type: 'Aliyun::Serverless::Log::Logstore' 
    Properties: 
      TTL: 10 
      ShardCount: 1 
fc-wordpress-domain: 
  Type: 'Aliyun::Serverless::CustomDomain' 
  Properties: 
     DomainName: 你的域名
     Protocol: HTTP 
     RouteConfig: 
        Routes: 
           '/\*': 
           ServiceName: fc-wp-mysql 
           FunctionName: wp-func
```



### 部署

```
fun deploy
```

部署成功之后，在你的阿里云上面就会相应的服务与函数。比如说\_FUN\_NAS\_fc-wp-mysql服务、fc-web-mysql服务和一个fun-generated-default-service服务(确保远端nas目录存在)。

### 注意事项(坑)

在func nas sync之前，建议访问后台把WordPress地址和站点地址全给出你的域名，不然在部署成功之后你访问网址就会导致有一些样式资源会走你所部署wordpress的服务器ip去获取资源，就不会走你的域名到达nas获取资源，这时就会出现404错误。

### 目前状况和测试

目前本人另外申请了一个域名还在备案，之后性能和测试再更新。  

### Reference

1.  [十分钟搭建基于 Wordpress 的 Serverless Web 应用](https://help.aliyun.com/document_detail/146729.html?spm=a2c4g.11186623.6.744.790dfc78XNX0RF#h2-3-3-9)