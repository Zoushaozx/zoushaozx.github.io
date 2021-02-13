---
title: 云e办
date: 2021-02-12 19:11:21
tags:
---

# 云e办，前后端分离，前端vue 后端Java



## 后端技术栈

```
springboot 
springmvc 
mybatisplus 对项目侵入少 代码编写简单
Lombok	注解插件
AutoGenerator	
Swagger2	接口文档
springsecret 安全框架
jwt 令牌
captcha 图形验证码
redis	
easypoi 文档导入导出
rabbitmq 消息队列 异步处理
mail 邮件 
websocket 在线聊天
fastDFS 文件服务器 承载静态资源
```

## 前端技术栈

```
vue
element UI
vue_cli 项目搭建
vuex 状态管理
vuerouter 路由管理
axios 通讯框架
es6 前端语法
webpack 打包
websocket 在线聊天
vue_chat 在线聊天
font_awesome 字体
js-file-download 文件下载
```



搭建后端项目

```
搭建数据库 yeb.sql
lombok 插件
整个项目采用了maven 聚合的项目

创建父工程：
	1⃣️spring initialize->next->修改Group/Artifact->next->spring boot devtools->Project location（项目存放地址）
	2⃣️父工程只是用来做pom依赖管理，就可以只保留pom.xml
	3⃣️修改pom文件
    删除dependencies标签
    删除build标签
    加packaging标签
		    <packaging>pom</packaging>
创建子项目：
	1⃣️new->Module->Maven->Create from archetype->org...:maven-ar..-quickstart等待默认依赖加载完成
	2⃣️删除main和test下面的App和Apptest类
	3⃣️更改包名，com.xxxx.server
	4⃣️修改server的pom依赖
		我们会发现，父工程下面多了，这个是关联server的
    <modules>
        <module>yeb-server</module>
    </modules>
    
    此时观察server的pom是否有关联父工程的parent标签，没有就加上 
    <parent>
        <artifactId>yeb</artifactId>
        <groupId>com.zoux</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    
    name和url标签可以删除
    
    maven.compiler.source/maven.compiler.target 版本改为1.8
    
    删除build标签 
    删除dependency标签
    
    添加依赖
    	<!--web 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--lombok 依赖-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <!--mysql 依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <!--mybatis 依赖-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.3.1.tmp</version>
        </dependency>
         
	5⃣️新建resources目录，并设置配置文件
  	在main下面新建resources，并设置为source目录 mark directory as -> resource root
  	并在resources里面新建文件，
  	application.yml
  	内容：
  		server:
 			 # 端口
			  port: 8081

  		spring:
        #数据源配置
        datasource:
          driver-class-name: com.mysql.cj.jdbc.Driver
          url: jdbc:mysql://localhost:3306/yeb?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
          username: root
          password: rootzoux
          hikari:	
            # 连接池名
            pool-name: DateHikariCP
            # 最小空闲连接数
            minimum-idle: 5
            #最大连接存活最大时间，默认600000 10分钟
            idle-timeout: 180000
            #最大连接数，默认10
            maximum-pool-size: 10
            #从连接池返回的连接自动提交
            auto-commit: true
            #连接存活的最大时间，0表示永久存活，默认1800000 30分钟
            max-lifetime: 1800000
            #连接超时，30000 默认30秒
            connection-timeout: 30000
            #测试连接是否可用的查询语句
            connection-test-query: SELECT 1
				# Mabatis-plug 配置
        mybatis-plus:
          #配置 Mapper映射文件
          mapper-locations: classpath*:/mapper/*Mapper.xml
          #配置 Mabatis数据返回类型别名 默认别名是类名
          type-aliases-package: com.zoux.server.pojo
          #自动驼峰命名
          configuration:
            map-underscore-to-camel-case: false

        # mybatis SQL 打印 （接口所在的包，不是mapper.xml 所在的包）
        logging:
          level:
            com.zoux.server.mapper: debug
            
	6⃣️在resources目录下新建mapper
	7⃣️添加包名，pojo，service，mapper，service.impl,controller
	8⃣️启动类YebApplication
			import org.springframework.boot.SpringApplication;
			import org.springframework.boot.autoconfigure.SpringBootApplication;

			@SpringBootApplication
			public class YebAppication {
   			 public static void main(String[] args) {
       			 SpringApplication.run(YebAppication.class, args);
   			 }
			}
	9⃣️添加mapper扫描
	@MapperScan("com.zoux.server.mapper")
	🔟在resources下新建comfig目录，将YebAppication.yml
```

