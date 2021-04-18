---
title: springboot
date: 2021-04-13 11:31:03
tags:
---

## application.yml配置

```
连接数据
  spring:
    datasource:
      url: jdbc:mysql://127.0.0.1:3306/next
      username: root
      password: rootzoux
      driver-class-name: com.mysql.cj.jdbc.Driver

能够根据实体类 生成数据表
  jpa:
      hibernate:
        ddl-auto: update
      show-sql: true

端口号
server
	port: 8888
```

## 初始化接口hello

```
package com.zoux.next_superhero.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        return "hello,world";
    }
}

```

