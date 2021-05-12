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

## ![截屏2021-05-07 上午9.04.54](/Users/zouxing/Library/Application Support/typora-user-images/截屏2021-05-07 上午9.04.54.jpg)

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

## 自动配置

```
启动时自动扫描加载spring.factories所有的自动配置类，导入对应的start就会生效
1⃣️springboot在启动的时候，从类路径下的/MATA-INF/spring.factories获取指定的值；
2⃣️将这些自动配置的类导入容器，自动配置就会生效，帮助我们进行自动配置！
```

## application.yml配置文件

```
语法
server
	port: 8888

为实体类赋值
	配置文件
	person:
		name: zoux
	具体实体类
		添加注解 @ConfigurationProperties(perfix="person")
```

## 实体类生成数据表

```
@Entity
@Getter
@Setter
@FieldNameConstants
@Table(name = "sys_name")
```



## 多配置文件

```
优先级：项目根目录/config/ > 根目录下的配置文件 >resource/config/配置文件 >resource/配置文件

springboot多环境配置
	server:
		port: 8080
	#选择激活的环境模块
	spring:
  	profiles:
  		active: dev
	---
  server:
  	port: 8081
  #配置环境名称	
  spring:
  	profiles: dev
  ---
  server:
  	port: test
  spring:
  	profiles:dev
```

## 添加静态资源

```
resource目下静态资源优先级
	resource >static >public
public 放公共资源 js
static 放静态资源 图片
resource 放上传文件
templates 资源 一般只能controller 访问 需要使用模板引擎
```

## 访问页面

```
controller 返回页面名称
templates  页面
```

## thymeleaf语法

```
<div th:utext="${msg}"></div>
<h3 th:each="user:${users}">[[${user}]]</h3>
```

---



## 不同项目数据交互

### 生产数据端

```
1⃣️新建maven模块 application.xml/spring.application.name= xxxx
2⃣️启动入口，注解
  @EnableAsync
  @SpringBootApplication
  @EnableEurekaClient
  @EnableFeignClients(basePackages = {"com.gzqylc", "gzqylc.com"})
  @ComponentScan(basePackages = { "com.gzqylc", "gzqylc.com" })
3⃣️开放数据域controller
	这个url用于产生数据
4⃣️例子
```

```java
@RestController
@Route(value = "api/public/user/", title = "员工相关接口")
public class UserDataApiController extends BaseController {
	@Route(value = "getContacts", title = "通讯录树")
    public List<OrgTreeSelectNode> getContacts(){
        return userService.getContacts().stream().filter(n -> n.getId().equals("O001")).collect(Collectors.toList());
    }
  }
```

### 消费数据端

```
1⃣️建立连接 service层
	注解@FeignClient(name = "hgs-mdd-server-data-api")
	定义方法
		@RequestMapping("api/public/user/getContacts")
    List<OrgTreeSelectNode> getContacts();
2⃣️	实现数据接收 controller层
	@Route(value = "/getContacts", title = "通讯录树")
  public List<OrgTreeSelectNode> getContacts(){
        return workPlatFormApiService.getContacts();
    }
3⃣️例子    
```

#### service

```java
@FeignClient(name = "hgs-mdd-server-data-api")
public interface WorkPlatFormApiService {

    @RequestMapping("api/public/user/getContacts")
    List<OrgTreeSelectNode> getContacts();
  
}
```

#### controller

```java
@Slf4j
@RestController
@Route(value = "api/workPlatForm", title = "景区管家提供的接口")
public class WorkPlatFormApiController extends BaseController {

    @Autowired
    private WorkPlatFormApiService workPlatFormApiService;

    @Route(value = "/getContacts", title = "通讯录树")
    public List<OrgTreeSelectNode> getContacts(){
        return workPlatFormApiService.getContacts();
    }
    }
```

---

## 项目打包上线

---

```
聚合项目主项目打包方式pom
静态资源尽量放到resource/static
rz -y 上传jar包
nohup java -jar plan_server-1.0.0.jar  运行项目
ps -ef|grep javakill -9 进程号  关闭项目
```

---

## mvc结构

```
domain @Entity
dao @Repository
controller @RestController
service @Service
```

## jpa存储文字乱码

```
url加上   ?characterEncoding=utf-8
```

