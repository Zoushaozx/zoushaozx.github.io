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
	一 根据用户名生成token
	1，根据用户信息生成token
		public String generatorToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        claims.put(CLAIM_KEY_USERNAME, userDetails.getUsername());
        claims.put(CLAIM_KEY_CREATED, new Date());
        return generatorToken();
        }
  2，根据荷载生成JWT TOKEN
  	private String generatorToken(Map<String,Object> claims) {
        return Jwts.builder()
                .setClaims(claims)
                .setExpiration(generateExpirationDate())
                .signWith(SignatureAlgorithm.ES512,secret)
                .compact();
                }
  3，生成token失效时间
   private Date generateExpirationDate() {
        return new Date(System.currentTimeMillis() + expiration * 1000);
    }
    
 二 从token拿用户名
  1，从token中获得登录用户名
  public String getUserNameFromToken(String token) {
        String username;
        try {
            Claims claims = getClaimsFromToken(token);
            username = claims.getSubject();
        } catch (Exception e) {
            username = null;
        }
        return username;
    }
  2，从token里面获取荷载
  private Claims getClaimsFromToken(String token) {
        Claims claims = null;
        try {
            claims = Jwts.parser()
                    .setSigningKey(secret)
                    .parseClaimsJws(token)
                    .getBody();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return claims;
    }
    
    
	三 判断token是否失效
	1，token是否过期
	 private boolean isTokenExpired(String token) {
        Date expiredDate = getExpiredDateFromToken(token);
        return expiredDate.before(new Date());
    }
    private Date getExpiredDateFromToken(String token) {
        Claims claims = getClaimsFromToken(token);
        return claims.getExpiration();
    }
	2，token里面的用户名与userDetail里面用户名是否一致
	public boolean validateToken(String token, UserDetails userDetails) {
        String username = getUserNameFromToken(token);
        return username.equals(userDetails.getUsername()) && !isTokenExpired(token);
    }
    
    
	四 是否可以刷新token
	 public boolean canRefresh(String token) {
        return !isTokenExpired(token);
    }
	五 刷新token
	public String refreshToken(String token) {
        Claims claims = getClaimsFromToken(token);
        claims.put(CLAIM_KEY_CREATED, new Date());
        return generatorToken(claims);
    }
```

公共返回对象

```
登录流程：
	前端传用户名和密码
	后端校验用户名和密码，
		有错误就重新输入
		正确就会生成JWT令牌，并且返回给前端，前端获得JWT令牌就会放入到请求头里面，任何请求都会携带JWT令牌，
		后端会有对应的拦截器，对JWT令牌做相应的验证，验证通过才能访问对应的接口，验证不通过，令牌可能失效了，可能是用户名有问题，或者密码有问题，不是合法登陆
		
1⃣️在pojo新建公共返回对象RespBean
2⃣️加注解@Data，生成getters and setters
	@NoArgsConstructor 无参
	@AllArgsConstructor	有参
3⃣️成功，失败返回方法，公共变量，code message obj
 public static RespBean success(String message) {
        return new RespBean(200, message, null);
    }
```

登录之后返回token

```
一 前端传用户名密码，后端校验
	1⃣️Admin类，里面实现springsecurity框架UserDetails
	实现方法，除了getAuthorities的返回值不变，isEnabled返回值为属性字段enabled，
	其他的都返回true
	2⃣️新建登陆实体类 AdminLoginParam
		添加lombok注解
			@Data
			@EqualsAndHashCode 此注解会生成equals(Object other) 和 hashCode()方法 callSuper是否调用父类中的方法
			@Accessors(chain = true)，Accessors翻译是存取器。通过该注解可以控制getter和setter方法的形式chain为true，setter方法返回当前对象
		  @ApiModel(value = "AdminLogin对对象",description = "" swagger注解
		  @ApiModelProperty(value = "密码",required = true) swagger注解，required是否必填
	3⃣️controller编写，LoginController
		注解：
			@Api(tags = "LoginController") 表示标识这个类是swagger的资源 
			@RestController 等同于@Controller + @ResponseBody，表明了这个类是一个控制器类+表示方法的返回值直接以指定的格式写入Http response body
      @ApiOperation(value = "登陆之后返回token")表示一个http请求的操作 
       @PostMapping("/login") 映射一个POST请求 
   4⃣️添加方法 login，参数 AdminLoginParam，HttpServletRequest
      public RespBean login(AdminLoginParam adminLoginParam, HttpServletRequest request);
   5⃣️引入IAdminService，并加入方法login，参数（用户名，密码，request）
   6⃣️在IAminService里面实现login
   7⃣️在AdminServiceImpl实现login
   	 注入UserDetailsService
   	 通过UserDetailsService，获得UserDetails
   	 注入PasswordEncoder
   	 利用PasswordEncoder 匹配一下密码
   	 判断userDetails账户是否禁用
   	 注入JWT工具类
   	 生成token
   	 注入tokenHead，jwt配置
   	 将token装入map
		 使用springsecurity框架，登陆成功后，获取用户信息可以使用springsecurity提供的对象userdetails等
		 所以登录时，要将用户信息放入springsecurity全文中
		 UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
        SecurityContextHolder.getContext().setAuthentication(authenticationToken);
	8⃣️passwordEncoder对象等待处理
```

获取当前登录用户信息

```
登陆时将当前登录对象设制到springsecurity全局，所以可以利用Principal获取当前登录用户对象
1⃣️在LoginController新建方法getAdminInfo
2⃣️利用Principal获取用户对象
3⃣️将获得admin对象密码置空，避免前端获取明文密码
4⃣️在IAdminService定义getAdminByUserName
5⃣️在AdminServiceImpl实现getAdminByUserName
	注入AdminMapper
	return adminMapper.selectOne(
			newQueryWrapper<Admin()
					   .eq("username",username)
					   .eq("enabled", true)
					   );

```

退出登录

```
后端返回200状态码，前端根据状态码，将请求头里面的token删除 
1⃣️在LoginController里面 新建方法logout 返回状态码200
```

配置security登陆过滤器

```
1⃣️准备配置类SecurityConfig，继承 WebSecurityConfigurerAdapter
2⃣️注解@Configuration
3⃣️注入Bean UserDetailsService，重写这个容器里的
		@Override
    @Bean
    public UserDetailsService userDetailsService() {
        return username -> {
            Admin admin = adminService.getAdminByUserName(username);
            if (null != admin) {
                return admin;
            }
            return null;
        };
    }
4⃣️重写configure方法，能够将我们重写的UserDetyailsService 注入到springsecurit
5⃣️实现passwordEncoder容器
6⃣️重写springsecurity配置configure(HttpSecurity http)
	关闭scrf，使用JWT不需要csrf
	 http.csrf()
        .disable()
	关闭session相关的
		基于token不需要session
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
	允许登陆访问
		.antMatchers("/login", "logout")
	配置生效
		.permitAll();
	访问请求要求认证
                .anyRequest()
                .authenticated()
   缓存关闭
                .headers()
                .cacheControl();
   添加jwt登陆授权拦截器
        http.addFilterBefore();
   自定义未授权，或者未登录的结果返回
        http.exceptionHandling()
                .accessDeniedHandler()
                .authenticationEntryPoint();
7⃣️准备jwt的登陆过滤器 JwtAuthorizationTokenFilter 继承OncePerRequestFilter 重写doFilterInternal==相当于前置拦截
	通过request获取Header
	进行token校验
		token是否存在，token是否以tokenHead为开头
	利用JwtUtils获取用户名
	进行用户名判断
		判断用户名是否为空，同时检查springsecurity全局上下文找不到用户对象，即未登录
	获取userdetails，相当于登陆了
	判断token是否有效，有效就将token重新放入用户对象	
		重新设置用户对象,更新荷载
		重新设置用户对象，更新details
		设置springsecurity全局上下文
	经过一系列验证，最后进行放行
8⃣️回到SecurityConfig类，利用Bean注解将JwtAuthorizationTokenFilter暴露出来
	并填入【添加jwt登陆授权拦截器】同时加上参数，UsernamePasswordAuthenticationFilter.class
```

security自定义返回结果

```
1⃣️新建类RestAuthorizationEntryPoint，实现AuthenticationEntryPoint
2⃣️通过response设置字符集
3⃣️设置数据交流格式
4⃣️获取输出流
5⃣️自定义返回类型

1⃣️新建类RestfulAccessDeniedHandler实现AccessDeniedHandler重写handle方法
2⃣️通过response设置字符集
3⃣️设置数据交流格式
4⃣️获取输出流
5⃣️自定义返回类型


在SecurityConfig类中进行注入RestAuthorizationEntryPoint，RestfulAccessDeniedHandler
将注入的两个类放入配置中
```

swagger2配资

```
1⃣️pom依赖
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
 2⃣️配置swagger，新建Swagger2Config类
 		注解
 		@Component 配置类
		@EnableSwagger2 开启swagger2
 3⃣️准备APiInfo
 4⃣️注入bean，规定扫描那些包，具体什么包，任意路径	 	
 5⃣️启动测试，重写允许通过方法，添加通过资源
 	 public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers(
                "/login",
                "/logout",
                "/css/**",
                "/js/**",
                "/index.html",
                "favicon.ico",
                "/doc.html",
                "/webjars/**",
                "/swagger-resources/**",
                "/v2/api-docs/**"
        );
    }
```

swagger2提供Authorize

```
实现全局登录
1⃣️在swagger2config类createRestApi方法里面继续添加 
																	.securityContexts()
									                .securitySchemes();
2⃣️完成securitySchemes
	设置请求头信息
		新建List
		新建ApiKey，三个参数
		将ApiKey加入List
		返回List
3⃣️完成securityContexts
	设置登陆访问认证路径
	result.add(getContextByPath("/hello/.*"));
	默认权限
	.securityReferences(defaultAuth())
	指代路径
	.forPaths(PathSelectors.regex(pathRegex))
	默认权限
		授权范围
			AuthorizationScope authorizationScope = new AuthorizationScope("global", "accessEverything");
     	result.add(new SecurityReference("Authorization", authorizationScopes));
4⃣️json传参
更改LoginController/login 参数注解@RequestBody ，更改JwtTokenUtil/generatorToken/加密模式HS512
5⃣️注意，SecurityConfig/configure（WebSecurity web）里面放行的配置与SecurityConfig/configure(HttpSecurity http)的放行配置重合，
可以将SecurityConfig/configure(HttpSecurity http)的login放行配置进行注释
	.antMatchers("/login", "/logout")
  .permitAll()
6⃣️进行Authorize设置，将Bearer空格tokon进行保存
```

生成验证码

```
1⃣️引入Google验证码依赖
	 <!--验证码-->
        <dependency>
            <groupId>com.github.axet</groupId>
            <artifactId>kaptcha</artifactId>
            <version>0.0.9</version>
        </dependency>
2⃣️准备谷歌验证码配置类
	注解@Configuration
	准备bean
	具体配置
				//验证码生成器
        DefaultKaptcha defaultKaptcha = new DefaultKaptcha();
        //配置
        Properties properties = new Properties();
        //是否有边框
        properties.setProperty("kaptcha.border", "yes");
        //设置边框颜色
        properties.setProperty("kaptcha.border.color", "105,179,90");
        //边框粗细度，默认为1
        // properties.setProperty("kaptcha.border.thickness","1");
        //验证码
        properties.setProperty("kaptcha.session.key", "code");
        //验证码文本字符颜色 默认为黑色
        properties.setProperty("kaptcha.textproducer.font.color", "blue");
        //设置字体样式
        properties.setProperty("kaptcha.textproducer.font.names", "宋体,楷体,微软雅 黑");
        //字体大小，默认40
        properties.setProperty("kaptcha.textproducer.font.size", "30");
        //验证码文本字符内容范围 默认为abced2345678gfynmnpwx
        // properties.setProperty("kaptcha.textproducer.char.string", "");
        //字符长度，默认为5
        properties.setProperty("kaptcha.textproducer.char.length", "4");
        //字符间距 默认为2
        properties.setProperty("kaptcha.textproducer.char.space", "4");
        //验证码图片宽度 默认为200
        properties.setProperty("kaptcha.image.width", "100");
        //验证码图片高度 默认为40
        properties.setProperty("kaptcha.image.height", "40");
        Config config = new Config(properties);
        defaultKaptcha.setConfig(config);
        return defaultKaptcha;
3⃣️新建Controller
	注解
	类注解  @RestController
	@ApiOperation(value = "验证码")
  @GetMapping(value = "/captcha")
  添加方法captcha参数为（HttpServletRequest，HttpServletResponse）
  通过流传递图片到前端,需要对response响应头做相应处理
  	response.setDateHeader("Expires", 0);
    response.setHeader("Cache-Control", "no-store, no-cache, must-revalidate");
    response.addHeader("Cache-Control", "post-check=0, pre-check=0");
    response.setHeader("Pragma", "no-cache");
    response.setContentType("image/jpeg");
  注入DefaultKaptcha
  生成验证码
  获取验证码文本内容
  将验证码放入session
  根据text创建图像验证码
  输出流输出图片，格式为jpg
4⃣️添加放行SecurityConfig/configure(WebSecurity web)+"/captcha"
5⃣️接口文档验证码乱码-在注解添加 @GetMapping(value = "/captcha",produces = "image/jpeg")
```

验证码校验

```
1⃣️在AdminLoginParam添加接收参数
	@ApiModelProperty(value = "验证码",required = true)
    private String code;
2⃣️在LoginController，添加参数验证码，同时在IAdminService/AdminServiceImpl也进行改变
3⃣️在AdminServiceImpl，获取验证码
	String captcha = (String) request.getSession().getAttribute("captcha");
4⃣️验证码比较
	不为空前端输入与后端验证码进行匹配
```

根据用户id查询菜单列表

```
1⃣️更改pojo实体类Menu
	 @ApiModelProperty(value = "子菜单")
   @TableField(exist = false)
   private List<Menu> children;	
2⃣️更改MenuController
	更改路径/system/cfg
	注入 IMenuService
	在getMenuByAdminId返回getMenusByAdminId
	用户信息一般是从后端获取，而不是前端获取，因为可能会被篡改
3⃣️在IMenuService接口定义getMenusByAdminId
4⃣️在MenuServiceImpl实现getMenusByAdminId
	return menuMapper.getMenusAdminId(((Admin)(SecurityContextHolder.getContext().getAuthentication().getPrincipal())).getId());
5⃣️在MenuMapper定义getMenusAdminId
6⃣️在MenuMapper写查询方法 定义返回类型
	<!--根据用户id查询菜单列表-->
	<select id="getMenusAdminId" resultMap="Menus">
        select distinct m1.*,
                        m2.id            as id2,
                        m2.url           as url2,
                        m2.path          as path2,
                        m2.`component`   as component2,
                        m2.name          as name2,
                        m2.`iconCls`     as iconCls2,
                        m2.`keepAlive`   as keepAlive2,
                        m2.`requireAuth` as requireAuth2,
                        m2.`parentId`    as parentId2,
                        m2.enabled       as enabled2
        from `t_menu` m1,
             `t_menu` m2,
             `t_menu_role` mr,
             `t_admin_role` ar
        where m1.id = m2.`parentId`
          and m2.id = mr.`mid`
          and mr.`rid` = ar.`rid`
          and ar.`adminId` = #{id}
          and m2.enabled = true
        order by m2.id
    </select>
    
    <resultMap id="Menus" type="com.zoux.server.pojo.Menu" extends="BaseResultMap">
        <collection property="children" ofType="com.zoux.server.pojo.Menu">
            <id column="id2" property="id"/>
            <result column="url2" property="url"/>
            <result column="path2" property="path"/>
            <result column="component2" property="component"/>
            <result column="name2" property="name"/>
            <result column="iconCls2" property="iconCls"/>
            <result column="keepAlive2" property="keepAlive"/>
            <result column="requireAuth2" property="requireAuth"/>
            <result column="parentId2" property="parentId"/>
            <result column="enabled2" property="enabled"/>
        </collection>
    </resultMap>

```

Redis集成菜单功能

```
1⃣️添加依赖
	<!--spring data redis 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <!--commons-pool2 对象池依赖-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
2⃣️添加配置application.yml 
	redis:
    #超时时间
    timeout:  10000ms
    #服务器地址
    host: 47.100.92.101
    #服务器端口
    port: 6379
    database: 0
    #    password: root
    lettuce:
      #默认连接数，默认是8
      max-active: 1024
      #最大连接阻塞等待时间
      max-wait: 1000ms
      #最大空闲连接
      max-idle: 200
      #最小空闲连接
      min-idle: 5
3⃣️新建redis配置类RedisConfig 参数RedisConnectionFactory
	引入Bean RedisTemplate
	序列化
	String类型序列器
	 Hash类型序列器
	 将RedisConnectionFactory 加入到RedisTemplate
4⃣️更改Menucontroller->IMenuService->MenuServiceImpl/getMenusByAdminId方法
	注入RedisTemplate
	先去redis获取
	判断menus是否为空,就通过数据库进行查询
		将数据设置到redis中
5⃣️后期，当涉及到数据菜单更新时，我们要重置redis里面的数据
```

根据请求url判断角色

```
权限RBAC基本概念
	Role- Based Access Controller
	权限与角色相关联，用户通过扮演适当的角色从而得到这些角色的权限。
	管理是层级相互依赖的，权限赋予给角色，角色又赋予用户。
	权限设计清楚，管理方便
	RBAC实际上是who what how三元组之间的关系，也就是who 对what进行how的操作
	谁对什么资源做了怎样的操作。
	
1⃣️pojo-Menu添加属性
		@ApiModelProperty(value = "角色列表")
    @TableField(exist = false)
    private List<Menu> roles;
2⃣️在IMenuService定义getMenusWithRole
3⃣️在MenuServiceImpl实现getMenusWithRole
4⃣️在MenuMapper定义getMenusWithRole
5⃣️在MenuMapper.xml实现sql
	并实现返回map
6⃣️将实现的getMenusWithRole放入过滤器中
	顺便更新config目录结构，重启项目，验证是否出现错误
7⃣️	新建CustomFilter配置类
	注解@Component
	实现FilterInvocationSecurityMetadataSource重写内部方法 getAttributes
	注入IMenuService
	引入AntPathMatcher
		在做uri匹配规则发现这个类，根据源码对该类进行分析，它主要用来做类URLs字符串匹配；
		获取请求的url
		获得menus
		循环判断 我们当前的url与获取的url是否匹配
		判断请求url与角色允许url是否匹配
		利用jdk8新特性stream流map一下
		能够匹配就将匹配的角色放入/list
		如果url匹配不上就默认给一个登陆角色
```

判断用户角色

```
1⃣️修改pojo-Admin
		@ApiModelProperty(value = "角色")
    @TableField(exist = false)
    private List<Role> roles;
    更改getAuthorities
    	获取权限列表
2⃣️IAdminService定义getRoles
3⃣️在AdminServiceImpl实现getRoles
	注入RoleMapper
	新建方法getRoles
4⃣️在RoleMapper定义getRoles
5⃣️在RoleMapper.xml中实现sql
7⃣️在登录方法中加入getRoles
	在LoginController-getAdminInfo/loadUserByUsername实现方法
		添加获取用户角色    
8⃣️添加拦截
	新建类CustomUrlDecisionManager
		注解@Component
		实现AccessDecisionManager，并重写其方法decide
			获取当前url需要角色
			判断角色是否登陆即可访问的角色，此角色在CustomFilter中设置
			判断是否登陆
			判断用户角色是否为url所需角色
			如若不满足，抛出异常
				throw new AccessDeniedException("权限不足，请联系管理员！");
9⃣️配置securityconfig
	引入CustomFilter，CustomUrlDecisionManager
	在访问请求要求认证下添加
		动态权限配置
		.withObjectPostProcessor()
			new ObjectPostProcessor范型为FilterSecurityInterceptor重写其方法postProcess
				添加CustomFilter，CustomUrlDecisionManager
					o.setAccessDecisionManager(customUrlDecisionManager);
          o.setSecurityMetadataSource(customFilter);
        返回o
🔟准备测试类
	HelloController
	注解@RestController
		@GetMapping("/employee/basic/hello")
    public String hello2() {
        return "/employee/basic/hell";
    }

    @GetMapping("hello")
    public String hello() {
        return "hello";
    }

    @GetMapping("/employee/advanced/hello")
    public String hello3() {
        return "/employee/advanced/hello";
    }
```

职位管理功能实现

```
1⃣️自定义日期格式
	pojo-Position
	添加注解
		@JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")
2⃣️更改PositionController
	查询
    @ApiOperation(value = "获取所有职位信息")
    注解@RequestMapping("/system/basic/pos")		
    注入IPositionService
    创建方法getAllPositions
      返回return positionService.list();
  添加
  	方法入参注解@RequestBody 接收前端传递给后端的json字符串中的数据的
  	加入当前时间
  		position.setCreateDate(LocalDateTime.now());
  	positionService.save(position)
  更新
  	positionService.updateById(position)
  删除
  	注解@PathVariable 接收请求路径中占位符的值
  	positionService.removeById(id)
  批量删除
  	positionService.removeByIds(Arrays.asList(ids))
```

全局异常

```
1⃣️新建包exception并新建类GlobalException
	注解@RestControllerAdvice 表示控制器的一个增强类
	捕捉异常
    @ExceptionHandler
  
```

职称管理功能实现

```
1⃣️给pojo-Joblevel类的属性createDate加上注解
	@JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")
2⃣️修改JoblevelController注解@RequestMapping的路径与数据库相匹配
	@RequestMapping("/system/basic/joblevel")
	与职位管理功能类似
```

权限组角色功能实现

```
通过角色表和用户表管关联，进而给用户分配不同的角色
也可以通过菜单角色也菜单表关联，进而给用户分配不同的角色，可以拥有不同菜单的权限
权限组分为角色和菜单部分
1⃣️新建PermissionController
  	添加注解
  		@RestController
			@RequestMapping("/system/basic/permission")
		注入IRoleService
		注入IMenuService
    实现角色部分
    	查询角色
    	添加角色
    		给名字加上前缀
    	删除角色
    实现菜单部分
    	查询所有菜单
    		在IMenuService中定义getAllMenus方法
    		在MenuServiceImpl中实现getAllMenus方法
    		在MenuMapper定义getAllMenus方法
    		在MenuMapper.xml实现sql 编写resultMap
    	根据角色id查询菜单id
      	@ApiOperation(value = "根据角色id查询菜单id")
    		@GetMapping("/mid/{rid}")
    		public List<Integer> getMidByRid(@PathVariable Integer rid) {
           return menuRoleService.list(new QueryWrapper<MenuRole>().eq("rid", rid)).stream().map(MenuRole::getMid).collect(Collectors.toList());
        }
    更新角色菜单
       流程：
       	每次更新之前，将这个角色的菜单清空
       	然后根据传入的角色id与菜单id进行关联
				涉及到了两步加上注解@Transactional 事物注解
        	注入mapper
        	删除角色下所有菜单	
        		menuRoleMapper.delete(new QueryWrapper<MenuRole>().eq("rid", rid));
        	批量更新菜单
        		insertRecord
        		在menuRoleMapper下定义insertRecord 注意在入参加上注解@Param
						在mapper实现sql
							<!--  更新角色菜单  -->
              <insert id="insertRecord">
                  insert into t_menu_role(mid,rid) values
                  <foreach collection="mids" item="mid" separator=",">
                      (#{mid},#{rid})
                  </foreach>
              </insert>
```

存储过程的创建和使用

```
存储过程就是具有名字的一段代码，用来完成一个特定的功能
创建的存储过程保存在数据库的数据字典中

//定义了用户root，使用本地IP地址			创建了存储过程名字为deleteDep
CREATE DEFINER=`root`@`localhost` PROCEDURE `deleteDep`(in did int,out result int)
begin
  declare ecount int;
  declare pid int;
  declare pcount int;
  declare a int;
  //不存在id，或者有子部门
  select count(*) into a from t_department where id=did and isParent=false;
  if a=0 then set result=-2;
  else
  //查询部门下员工数量
  select count(*) into ecount from t_employee where departmentId=did;
  if ecount>0 then set result=-1;
  //有员工不能进行删除
  else 
  //查询 父id
  select parentId into pid from t_department where id=did;
  //正式删除
  delete from t_department where id=did and isParent=false;
  //获得受影响行数
  select row_count() into result;
  //查询部门的父部门
  select count(*) into pcount from t_department where parentId=pid;
  //判断当子部门为0时，就改变字段isParent。
  if pcount=0 then update t_department set isParent=false where id=pid;
  end if;
  end if;
  end if;
end
//定义了用户root，使用本地IP地址      创建了存储过程名字为addDep 参数 in表输入参数 depName表输入参数名称 out输出参数
CREATE DEFINER=`root`@`localhost` PROCEDURE `addDep`(in depName varchar(32),in parentId int,in enabled boolean,out result int,out result2 int)
begin
//定义了did int类型的
  declare did int;
  declare pDepPath varchar(64);
  //插入数据
  insert into t_department set name=depName,parentId=parentId,enabled=enabled;
  //获取受影响的行数
  select row_count() into result;
  //获取插入的最后一条的id
  select last_insert_id() into did;
  set result2=did;
  //查询路径 放入到pDepPath里面
  select depPath into pDepPath from t_department where id=parentId;
  /在原有的路径的基础上加上上一次插入的路径
  update t_department set depPath=concat(pDepPath,'.',did) where id=did;
  //经过上一句，这条数据库记录本身就上升为有子部门的记录了
  update t_department set isParent=true where id=parentId;
end
```

部门管理功能实现

```
1⃣️获取所有部门
	更改DepartmentController
		注解RequestMapping路径/system/basic/department
		注入IDepartmentService
		定义方法getAllDepartments
			注解
				@ApiOperation(value = "获取所有部门")
		    @GetMapping("/")
	在pojo-Department中
    	定义属性
    		@ApiModelProperty(value = "子部门列表")
        @TableField(exist = false)
        private List<Department> children;
        @ApiModelProperty(value = "返回结果，存储过程使用")
        @TableField(exist = false)
        private Integer result;
  在IDepartmentService定义getAllDepartments
  在DepartmentServiceImpl实现getAllDepartments
  	注入departmentMapper
  在DepartmentMapper定义getAllDepartments
  在DepartmentMapper.xml实现sql
  	利用mybatis提供的递归查询实现
  		在DepartmentServiceImpl的getAllDepartments方法提供入参（最大的parentid）
  		递归体现在DepartmentMapper.xml编写resultMap时
  			<resultMap id="DepartmentWithChildren" type="com.zoux.server.pojo.Department" extends="BaseResultMap">
        <collection property="children" ofType="com.zoux.server.pojo.Department"
                    select="com.zoux.server.mapper.DepartmentMapper.getAllDepartments" column="id">
        </collection>
    </resultMap>
    	其中select="com.zoux.server.mapper.DepartmentMapper.getAllDepartments" column="id">
    	为入口
2⃣️添加部门
	修改DepartmentController
		注解
			@ApiOperation(value = "添加部门")
    	@PostMapping("/")
    新建方法addDep
    在IDepartmentService定义addDep
    在DepartmentServiceImpl实现addDep
	    更改即将存储的department
	    	enabled=true
	    判断添加是否成功
    在DepartmentMapper定义addDep
    在DepartmentMapper.xml里面实现sql
    	不使用传统insert
    	使用存储过程
    		<!--    添加部门  -->
        <select id="addDep" statementType="CALLABLE">
            call addDep(#{name,mode = IN,jdbcType=VARCHAR},
                #{parentId,mode = IN,jdbcType=INTEGER},
                #{enabled,mode = IN,jdbcType=BOOLEAN},
                #{result,mode = OUT,jdbcType=INTEGER},
                #{id,mode = OUT,jdbcType=INTEGER})
        </select>
3⃣️删除部门 
	修改DepartmentController
		注解
			@ApiOperation(value = "删除部门")
   	  @DeleteMapping("/{id}")
   	新建方法deleteDep  
   	在IDepartmentService定义deleteDep
    在DepartmentServiceImpl实现deleteDep
    	判断删除是否成功
    在DepartmentMapper定义addDep
    在DepartmentMapper.xml里面实现sql
    	不使用传统insert
    	使用存储过程
      	<!--    删除部门  -->
        <select id="deleteDep" statementType="CALLABLE">
            call deleteDep( #{id,mode = IN,jdbcType=INTEGER},
                #{result,mode = OUT,jdbcType=INTEGER}
                )
        </select>
```

操作员管理功能

```
1⃣️获取所有操作员
	修改AdminController
		注解
      @RequestMapping("/system/admin")
      @GetMapping("/")
		注入adminService
		新建方法getAllAdmins参数keyword
		在IAdminService定义getAllAdmins方法
		在AdminServiceImpl实现getAllAdmins
		在AdminMapper定义getAllAdmins
		在AdminMapper.xml实现sql 编写resultMap
			<select id="getAllAdmins" resultMap="AdminWithRole">
        select
        a.*,
        r.id as rid,
        r.name as rname,
        r.nameZh as rnameZh
        from t_admin a
        LEFT JOIN
        t_admin_role ar ON a.id = ar.adminId
        LEFT JOIN
        t_role r ON r.id = ar.rid
        where a.id !=#{id}

        <if test="null!=keyword and ''!=keyword">
            and a.name   like CONCAT('%',#{keyword},'%')
        </if>
        ORDER BY
        a.id
    	</select>
    	<resultMap id="AdminWithRole" type="com.zoux.server.pojo.Admin" extends="BaseResultMap">
        <collection property="roles" ofType="com.zoux.server.pojo.Role">
            <id column="rid" property="id"/>
            <result column="rname" property="name"/>
            <result column="rnameZh" property="nameZh"/>
        </collection>
    </resultMap>
2⃣️更新操作员
	AdminController新建方法updateAdmin
		注解
			ApiOperation(value = "更新操作员")
   	  PutMapping("/")
   	  RequestBody
   	调用adminService.updateById（admin ）
    更改pojo-Admin	指定属性enabled，lombok注解@Getter(AccessLevel.NONE)
    	因为实现了UserDetails，所以有关enabled的get方法被重写，使用get方法时就会出现错误，关闭lombok，		get方法
3⃣️删除操作员
	在AdminController新建方法deleteAdmin
	注解
		@ApiOperation(value = "删除操作员")
    @DeleteMapping("/{id}")
    @PathVariable
  注入adminService
  	调用removeById(id)  
4⃣️获取所有角色
	在AdminController新建方法getAllRoles
	注解
		@ApiOperation(value = "获取所有角色")
    @GetMapping("/roles")
  注入roleService
  	调用list（）
5⃣️更新操作员角色
	在AdminController新建方法updateAdminOfRole
	注解：
		@ApiOperation(value = "更新操作员角色")
    @PutMapping("/role")
  注入adminService
  	调用updateAdminOfRole(adminId, rids)
  在AdminServiceImpl
  	注入adminRoleMapper
  		调用delete，根据adminId删除角色
  			delete(new QueryWrapper<AdminRole>().eq("adminId", adminId))
  		新建addAdminOfRole
  	判断addAdminOfRole方法返回结果
  在AdminRoleMapper定义addAdminOfRole方法
  在AdminRoleMapper.xml实现sql
  	<!--    更新操作员角色-->
    <update id="addAdminOfRole">
        insert into t_admin_role(adminId,rid) values
        <foreach collection="rids" item="rid" separator=",">
            (#{adminId},#{rid})
        </foreach>
    </update>	
```

员工管理功能-分页

```
1⃣️分页配置
	新建配置类MybatisPlusConfig
		注解@Configuration
		注入
			@Bean
			PaginationInterceptor
	新建公共分页返回对象pojo-RespPageBean
  	注解
  		@Data
			@NoArgsConstructor
			@AllArgsConstructor
			
		定义属性
    	 Long total	总条数
    	 List<?> data	数据list
2⃣️全局时间格式转换类DateConverter
		注解
			@Component
		实现Converter<String, LocalDate>
			重写convert
				return LocalDate.parse(source, DateTimeFormatter.ofPattern("yyyy-MM-dd"));
		给pojo-Employee于时间相关的属性加上注解，以便用于前端访问
			@JsonFormat(pattern = "yyyy-MM-dd",timezone = "Asia/Shanghai")
3⃣️将外接表在pojo-Employee体现
		@ApiModelProperty(value = "民族")
    @TableField(exist = false)
    private Nation nation;
4⃣️获取员工分页查询
	修改EmployeeController
		根据t_menu确定注解@RequestMapping("/employee/basic")路径
		新建方法getEmployee
			注解
			 @ApiOperation(value = "获取所有员工（分页）")
	     @GetMapping("/")
			参数
				@RequestParam(defaultValue = "1") Integer currentPage,
        @RequestParam(defaultValue = "10") Integer size,
        Employee employee,
        LocalDate[] beginDateScope
      注入employeeEcService，添加方法getEmployeeByPage
      	参数(currentPage, size, employee, beginDateScope)
      在IEmployeeEcService定义getEmployeeByPage
      在EmployeeEcServiceImpl实现getEmployeeByPage
      	开启分页 page/com.baomidou.mybatisplus.extension.plugins.pagination.Page;
      	Page<EmployeeEc> page = new Page<>(currentPage, size);
      	注入employeeMapper，
      		在employeeMapper定义getEmployeeByPage
      		参数(page, employee, beginDateScope)
      	将查询结果设置到IPage
      	通过RespPageBean将IPage拆解返回
      		RespPageBean respPageBean = new RespPageBean(employeeByPage.getTotal(), 							employeeByPage.getRecords());
      在EmployeeMapper定义getEmployeeByPage
      在EmployeeMapper.xml实现sql，编写resultMap
      	 <!--    获取所有员工(分页)-->
        <select id="getEmployeeByPage" resultMap="EmployeeInfo">
            SELECT
            e.*,
            n.id AS nid,
            n.`name` AS nname,
            p.id AS pid,
            p.`name` AS pname,
            d.id AS did,
            d.`name` AS dname,
            j.id AS jid,
            j.`name` AS jname,
            pos.id AS posid,
            pos.`name` AS posname
            FROM
            t_employee e,
            t_nation n,
            t_politics_status p,
            t_department d,
            t_joblevel j,
            t_position pos
            WHERE
            e.nationId = n.id
            AND e.politicId = p.id
            AND e.departmentId = d.id
            AND e.jobLevelId = j.id
            AND e.posId = pos.id
            <if test="null!=employee.name and ''!=employee.name">
                AND e.`name` LIKE concat( '%', #{employee.name}, '%' )
            </if>
            <if test="null!=employee.politicId ">
                AND e.politicId =#{employee.politicId}
            </if>
            <if test="null!=employee.nationId ">
                AND e.nationId =#{mployee.nationId}
            </if>
            <if test="null!=employee.jobLevelId ">
                AND e.jobLevelId =#{mployee.jobLevelId}
            </if>
            <if test="null!=employee.posId ">
                AND e.posId =#{mployee.posId}
            </if>
            <if test="null!=employee.engageForm and ''!=employee.engageForm">
                AND e.engageForm =#{mployee.engageForm}
            </if>
            <if test="null!=employee.departmentId ">
                AND e.departmentId =#{mployee.departmentId}
            </if>
            <if test="null!=beginDateScope and 2==beginDateScope.length">
            AND e.beginDate BETWEEN
            #{beginDateScope[0]} AND #{beginDateScope[1]}
       		  </if>
            ORDER BY
            e.id
         </select>
					
					<resultMap id="EmployeeInfo" type="com.zoux.server.pojo.Employee" extends="BaseResultMap">
            <association property="nation" javaType="com.zoux.server.pojo.Nation">
                <id column="nid" property="id"/>
                <result column="nname" property="name"/>
            </association>
            <association property="politicsStatus" javaType="com.zoux.server.pojo.PoliticsStatus">
                <id column="pid" property="id"/>
                <result column="pname" property="name"/>
            </association>
            <association property="department" javaType="com.zoux.server.pojo.Department">
                <id column="did" property="id"/>
                <result column="dname" property="name"/>
            </association>
            <association property="joblevel" javaType="com.zoux.server.pojo.Joblevel">
                <id column="jid" property="id"/>
                <result column="jname" property="name"/>
            </association>
            <association property="position" javaType="com.zoux.server.pojo.Position">
                <id column="posid" property="id"/>
                <result column="posname" property="name"/>
            </association>
        </resultMap>
```

员工管理功能-添加/删除/更新员工

```
1⃣️获取所有政治面貌
2⃣️获取所有职称
3⃣️获取所有民族
4⃣️获取所有职位
5⃣️获取所有部门
前六步步骤一摸一样
	在EmployeeController编辑
    注入departmentService
    @ApiOperation(value = "获取所有部门")
      @GetMapping("/department")
      public List<Department> getAllDepartment() {
          return departmentService.getAllDepartments();
      }
6⃣️获取工号
	在EmployeeController新建maxWorkId
	注入employeeEcService
	在IEmployeeEcService定义maxWorkId
	在EmployeeEcServiceImpl实现maxWorkId
		注入employeeMapper
    	查询sql	
    		employeeMapper.selectMaps(new QueryWrapper<Employee>().select("max(workId)"))
    返回格式化后的数据
    	String.format("%08d", Integer.parseInt(maps.get(0).get("max(workId)").toString()) + 1)
7⃣️添加员工
	在EmployeeController新建addEmp
	注入employeeEcService
	在IEmployeeEcService定义addEmp
	在EmployeeEcServiceImpl实现addEmp
		计算合同期限计算,保留2位小数
	注入employeeMapper
  	调用nsert(employee)
  返回提示	
8⃣️更新员工
	在EmployeeController新建updateEmp
	注解RequestBody
		if (employeeEcService.updateById(employee)) {
            return RespBean.success("更新成功");
        }
        return RespBean.error("更新失败");
9⃣️删除员工
	在EmployeeController新建deleteEmp
		注解PathVariable
	 if (employeeEcService.removeById(id)) {
            return RespBean.success("删除成功");
        }
        return RespBean.error("删除失败");
```

员工数据导入导出

```
1⃣️引入easy poi依赖
	 <!--   easy poi依赖     -->
        <dependency>
            <groupId>cn.afterturn</groupId>
            <artifactId>easypoi-spring-boot-starter</artifactId>
            <version>4.1.3</version>
        </dependency>
2⃣️pojo-Employee加上easypoi注解
		@ApiModelProperty(value = "性别")
		@Excel(name = "性别")
    private String gender;
    with长度 format格式化
   		@Excel(name = "出生日期", width = 30, format = "yyyy-MM-dd")
 		跨实体类注解
 			@ApiModelProperty(value = "职位")
      @TableField(exist = false)
      @ExcelEntity(name = "职位")
      private Position position;
      	进入到具体实体类加注解
      		  @ApiModelProperty(value = "职位")
            @Excel(name = "职位")
            private String name;
3⃣️导出数据
	在EmployeeController新建exportEmployee方法
		注解
    	@ApiOperation(value = "导出员工数据")
    	@GetMapping(value = "/export",produces = "application/octet-stream")
		参数
			HttpServletResponse response
		注入
			employeeService
				新建方法getEmployee
					将查询的数据存入List
					ExportParams params = new ExportParams("员工表", "员工表", ExcelType.HSSF);
        	Workbook workbook = ExcelExportUtil.exportExcel(params, Employee.class, list);
        	输出流
        		ServletOutputStream out = null;
        		//以流的形式传输
            response.setHeader("content-type", "application/octet-stream");
            //防止中文乱码
            response.setHeader("content-disposition", "attachment;filename=" + URLEncoder.encode("员工表.xls", "UTF-8"));
            out=response.getOutputStream();
            workbook.write(out);
            
		在IEmployeeService定义	getEmployee
    在EmployeeServiceImpl实现 getEmployee
    	注入employeeMapper
    在EmployeeMapper 定义	getEmployee
    在EmployeeMapper.xml实现sql，编写resultMap
    	<!--    导出员工数据-->
   	 <select id="getEmployee" resultMap="EmployeeInfo">
        SELECT e.*,
        n.id AS nid,
        n.`name` AS nname,
        p.id AS pid,
        p.`name` AS pname,
        d.id AS did,
        d.`name` AS dname,
        j.id AS jid,
        j.`name` AS jname,
        pos.id AS posid,
        pos.`name` AS posname
        FROM t_employee e,
        t_nation n,
        t_politics_status p,
        t_department d,
        t_joblevel j,
        t_position pos
        WHERE e.nationId = n.id
        AND e.politicId = p.id
        AND e.departmentId = d.id
        AND e.jobLevelId = j.id
        AND e.posId = pos.id
        <if test="null!=id">
            AND e.id = #{id}
        </if>
        ORDER BY e.id
    	</select>
    	
    	<resultMap id="EmployeeInfo" type="com.zoux.server.pojo.Employee" extends="BaseResultMap">
        <association property="nation" javaType="com.zoux.server.pojo.Nation">
            <id column="nid" property="id"/>
            <result column="nname" property="name"/>
        </association>
        <association property="politicsStatus" javaType="com.zoux.server.pojo.PoliticsStatus">
            <id column="pid" property="id"/>
            <result column="pname" property="name"/>
        </association>
        <association property="department" javaType="com.zoux.server.pojo.Department">
            <id column="did" property="id"/>
            <result column="dname" property="name"/>
        </association>
        <association property="joblevel" javaType="com.zoux.server.pojo.Joblevel">
            <id column="jid" property="id"/>
            <result column="jname" property="name"/>
        </association>
        <association property="position" javaType="com.zoux.server.pojo.Position">
            <id column="posid" property="id"/>
            <result column="posname" property="name"/>
        </association>
    	</resultMap>
3⃣️导入数据    	
	修改pojo类 Nation PoliticsStatus Department Joblevel Position
		修改@EqualsAndHashCode(callSuper = false,of = "name")
			重写关于name的equals和hashcode方法
		加入@NoArgsConstructor
		加入@RequiredArgsConstructor
		在属性name加上非空注解@NonNull
	在EmployeeController类里面新建importEmployee方法
  	注解
  		 @ApiOperation(value = "导入员工数据")
   		 @PostMapping("/import")
   	入参
    	MultipartFile file
    准备即将名字转id集合
   	 List<Nation> nationList = nationService.list();
   	导入参数
    	ImportParams params = new ImportParams();
    去掉标题行
    	params.setTitleRows(1);
    excel import 工具类 	
    	List<Employee> list = ExcelImportUtil.importExcel(file.getInputStream(), Employee.class, params);
    将子表名称转换为id
    	list.forEach(employee -> {
    		名族id
     		employee.setNationId(nationList.get(nationList.indexOf(new Nation(employee.getNation().getName()))).getId());
    	});
    保存数据集合
    	employeeService.saveBatch(list)
```



