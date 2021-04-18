---
title: rabbitmq
date: 2021-02-18 21:20:17
tags:
---

# Linux下安装配置启动RabbitMQ

RabbitMQ依赖erlang所以需要先安装erlang以及他需要的环境

安装erlang

```
第一步：下载 erlang包
wget https://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm
第二步  安装依赖
yum -y install epel-release
第三步 安装erlang
rpm -ivh erlang-solutions-1.0-1.noarch.rpm
yum install erlang
输入y继续
第四步 验证
erl
出现
	Erlang R16B03-1 (erts-5.10.4) [source] [64-bit] [async-threads:10] [hipe] [kernel-			poll:false]
 
	Eshell V5.10.4  (abort with ^G)
	1>
成功
```

安装RabittMQ

```
官网https://www.rabbitmq.com/install-rpm.html#downloads

```

