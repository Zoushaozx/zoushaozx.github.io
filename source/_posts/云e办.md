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

逆向工程

```
根据新建pojo mapper controller 单表查询
mybatisplus，官网
https://mp.baomidou.com/guide/generator.html
逆向工程本身与项目没有太大关联，就新建module承载
1⃣️新建module，maven，quikerstart
2⃣️删除pom， name、url标签，maven.compiler.source/target 版本号为1.8，删除dependencies/build标签
3⃣️删除App类，新建包generator
4⃣️删除test文件夹
5⃣️指定父工程
	    <parent>
        <artifactId>yeb</artifactId>
        <groupId>com.zoux</groupId>
        <version>0.0.1-SNAPSHOT</version>
   		 </parent>
 6⃣️添加依赖
 		<!--web 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--mybatis 依赖-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.3.1.tmp</version>
        </dependency>
        <!--mybatis-plus 代码生成器依赖-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.3.1.tmp</version>
        </dependency>
        <!--    ferrmarker 依赖 模版-->
        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
        </dependency>
        <!--mysql 依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
7⃣️新建类MysqlGenerator
package com.zoux.generator;

import com.baomidou.mybatisplus.core.exceptions.MybatisPlusException;
import com.baomidou.mybatisplus.core.toolkit.StringPool;
import com.baomidou.mybatisplus.core.toolkit.StringUtils;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
import com.baomidou.mybatisplus.generator.engine.FreemarkerTemplateEngine;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class MysqlGenerator {
    // 演示例子，执行 main 方法控制台输入模块表名回车自动生成对应项目目录中

    /**
     * <p>
     * 读取控制台内容
     * </p>
     */
    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotBlank(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    public static void main(String[] args) {
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();

        // 全局配置
        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        gc.setOutputDir(projectPath + "/yeb-generator/src/main/java");
        //作者
        gc.setAuthor("zoux");
        //是否打开输出目录
        gc.setOpen(false);
        //xml  开启BaseResultMap
        gc.setBaseResultMap(true);
        //xml  开启BaseColumnList
        gc.setBaseColumnList(true);
        //实体属性 Swagger2 注解
        gc.setSwagger2(true); //实体属性 Swagger2 注解
        mpg.setGlobalConfig(gc);

        // 数据源配置
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://localhost:3306/yeb?useUnicode=true&useSSL=false&characterEncoding=utf8&serverTimezone=Asia" + "/Shanghai");
        // dsc.setSchemaName("public");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("rootzoux");
        mpg.setDataSource(dsc);

        // 包配置
        PackageConfig pc = new PackageConfig();
//        pc.setModuleName(scanner("模块名"));
        pc.setParent("com.zoux")
                .setEntity("pojo")
                .setMapper("mapper")
                .setService("service")
                .setServiceImpl("service.impl")
                .setController("controller");
        mpg.setPackageInfo(pc);

        // 自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };

        // 如果模板引擎是 freemarker
        String templatePath = "/templates/mapper.xml.ftl";
        // 如果模板引擎是 velocity
        // String templatePath = "/templates/mapper.xml.vm";

        // 自定义输出配置
        List<FileOutConfig> focList = new ArrayList<>();
        // 自定义配置会被优先输出
        focList.add(new FileOutConfig(templatePath) {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
//                return projectPath + "/src/main/resources/mapper/" + pc.getModuleName()
//                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
                return projectPath + "/yeb-generator/src/main/resources/mapper/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        /*
        cfg.setFileCreate(new IFileCreate() {
            @Override
            public boolean isCreate(ConfigBuilder configBuilder, FileType fileType, String filePath) {
                // 判断自定义文件夹是否需要创建
                checkDir("调用默认方法创建的目录，自定义目录用");
                if (fileType == FileType.MAPPER) {
                    // 已经生成 mapper 文件判断存在，不想重新生成返回 false
                    return !new File(filePath).exists();
                }
                // 允许生成模板文件
                return true;
            }
        });
        */
        cfg.setFileOutConfigList(focList);
        mpg.setCfg(cfg);

        // 配置模板
        TemplateConfig templateConfig = new TemplateConfig();

        // 配置自定义输出模板
        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别
        // templateConfig.setEntity("templates/entity2.java");
        // templateConfig.setService();
        // templateConfig.setController();

        templateConfig.setXml(null);
        mpg.setTemplate(templateConfig);

        // 策略配置
        StrategyConfig strategy = new StrategyConfig();
        //数据库表映射到实体的命名策略
        strategy.setNaming(NamingStrategy.underline_to_camel);
        //数据库表字段映射到实体的命名策略
        strategy.setColumnNaming(NamingStrategy.no_change);

        //strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");

        //lombok 模型
        strategy.setEntityLombokModel(true);
        //生成@RestController 控制器 这个相对于普通controller 返回数据为json字符串
        strategy.setRestControllerStyle(true);
        // 公共父类
        //strategy.setSuperControllerClass("你自己的父类控制器,没有就不用设置!");
        // 写于父类中的公共字段
        //strategy.setSuperEntityColumns("id");
        strategy.setInclude(scanner("表名，多个英文逗号分割").split(","));
        strategy.setControllerMappingHyphenStyle(true);
        //表前缀
        //strategy.setTablePrefix(pc.getModuleName() + "_");
        strategy.setTablePrefix("t_");
        mpg.setStrategy(strategy);

        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }

}
8⃣️运行，输入表名，
9⃣️将所有生成的类拷贝到yeb-server
🔟更改路径，和添加swagger依赖
	        <!--Swagger2 依赖-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.7.0</version>
        </dependency>
        <!--Swagger第三方UI依赖-->
        <dependency>
            <groupId>com.github.xiaoymin</groupId>
            <artifactId>swagger-bootstrap-ui</artifactId>
            <version>1.9.6</version>
        </dependency>

```

Jwt Token工具类

```
1⃣️引入jwt，security依赖
	 		<!--security 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <!--JWT 依赖-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.0</version>
        </dependency>
2⃣️引入security配置，application.yml
jwt:
  #JWT存储的请求头
  tokenHeader: Authorization
  #JWT 加密使用的密钥
  secret: yeb-secret
  #JWT 的超期限时间 （60*60*24）
  expiration: 604800
  #JWT 负载中拿到开头
  tokenHead: Bearer
3⃣️新建包config.security
4⃣️新建JwtTokenUtil类 
5⃣️添加注解@Component 
	把普通pojo实例化到spring容器中，相当于配置文件中的泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类
6⃣️准备荷载变量
	CLAIM_KEY_USERNAME
	CLAIM_KEY_CREATED
	secret
	expiration
	
		private static final String CLAIM_KEY_USERNAME="sub";
    private static final String CLAIM_KEY_CREATED="created";
    @Value("${jwt.secret}")
    private String secret;
    @Value("${jwt.expiration}")
    private Long expiration;
    
7⃣️主要功能
	根据用户名生成用户名
	从token拿用户名
	判断touken是否失效
	是否可以刷新token
	刷新token
	
```

