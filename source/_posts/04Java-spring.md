---
title: Java-spring
date: 2021-04-18 08:54:28
tags:
---

## IOC理论

```
控制反转，
从本质上解决问题，不用去管理对象的创建。
系统的耦合性大大降低，可以更加专注的在业务的实现。
控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定的对象的方式。
spring中实现控制反转的是IOC容器，其实现方法是依赖注入（Dependency Injection，DI）。
```

## XML实现对象创建

```
在resource/下创建beans.xml
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="hello" class="com.zoux.pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>

    <!--使用Spring来创建对象，在Spring这些都称为Bean
     类型   变量名 = new 类型()
     Hello hello =  new Hello()
     id = 变量名
     class = new 的对象
     property 相当于给对象中的属性设置一个值
         ref 引用spring容器中创建好的对象
         value 具体的值，基本数据类型
     

     这个过程就是控制反转：
     控制：谁来控制对象的创建，传统应用程序的对象是由程序本身创建的，使用Spring后，对象是由Spring来创建。
     反转：程序本身不创建对象，而变成被动的接收对象。
     依赖注入：利用set方法来进行注入
     IOC是一种编程思想，由主动的编程变成被动的接收

     -->
</beans>
```

```
创建实体类
```

```java
package com.zoux.pojo;

public class Hello {
    private String str;

    public String getStr() {
        return str;
    }

    public void setStr(String str) {
        this.str = str;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "str='" + str + '\'' +
                '}';
    }
}

```

```
使用对象
```

```java
import com.zoux.pojo.Hello;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        //获取spring上下文对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在spring里面管理了，想要使用直接取出来就可以了
        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello.toString());
    }
}

```

## IOC创建对象探索

```
使用无参构造创建对象
使用有参构造创建对象
```

```
在配置文件加载的时候，容器中管理的对象就已经初始化了
```

```
 <!--使用Spring来创建对象，在Spring这些都称为Bean
     类型   变量名 = new 类型()
     Hello hello =  new Hello()
     id = 变量名
     class = new 的对象
     property 相当于给对象中的属性设置一个值
         ref 引用spring容器中创建好的对象
         value 具体的值，基本数据类型


     这个过程就是控制反转：
     控制：谁来控制对象的创建，传统应用程序的对象是由程序本身创建的，使用Spring后，对象是由Spring来创建。
     反转：程序本身不创建对象，而变成被动的接收对象。
     依赖注入：利用set方法来进行注入
     IOC是一种编程思想，由主动的编程变成被动的接收

     -->
```

## Spring配置

```
alias 别名 相较于name name可以取多个别名 优先级比alias更高
<alias name="user" alias="newUser"/>
```

```
id 
bean 对象的唯一标志符，类似对象名
```

```
class
bean 对象所对应的全限定名，包名+类型
```

```
import 可以将多个配置文件导入合并为一个
```

## DI 依赖注入

```
依赖：bean对象的创建依赖于容器
注入：bean对象的所有属性，由容器来注入
构造器注入
set方式注入
其它方式
```

## set方法注入

```
普通注入 value
<bean id="user" class="com.zoux.pojo.User" >
	<property name="name" value="zoux"/>
</bean>
```

```
Bean 注入
<bean id="user" class="com.zoux.pojo.User" >
	<property name="address" ref="address"/>
</bean>
```

```
数组注入
<bean id="user" class="com.zoux.pojo.User" >
	<property name="bookd" >
		<array>
			<value>红楼梦</value>
			<value>西游记</value>
			<value>三国演义</value>
			<value>水浒</value>
		</array>
	</property>
</bean>
```

```
List注入
<bean id="user" class="com.zoux.pojo.User" >
	<property name="hobbys" >
		<list>
			<value>听歌</value>
			<value>敲代码</value>
			<value>看电影</value>
			<value>DIY</value>
		</list>
	</property>
</bean>
```

```
Map注入
<bean id="user" class="com.zoux.pojo.User" >
	<property name="card" >
		<map>
			<entry key="身份证" value="123434545465465"/>
			<entry key="银行卡" value="3423489324832948329048"/>
		</map>
	</property>
</bean>
```

```
Set注入
<bean id="user" class="com.zoux.pojo.User" >
	<property name="games" >
		<set>
			<value>LOL</value>
			<value>DIY</value>
			<value>王者荣耀</value>
		</set>
	</property>
</bean>
```

```
null注入
<bean id="user" class="com.zoux.pojo.User" >
	<property name="wife" >
		<null/>
	</property>
</bean>
```

```
properties注入
<bean id="user" class="com.zoux.pojo.User" >  
	<property name="info" > 
	 <prop key="学号">123213421<prop/>
   <prop key="姓名">邹兴<prop/>
   <prop key="性别">男<prop/>
	</property>
</bean>
```



## 其他方式注入

```
p命名空间 引入依赖 通过 property注入
<bean id="user" class="com.zoux.pojo.User" p:name="zoux" p:age="18" />
c命名空间 引入依赖 通过 构造器注入 空参构造器 有参构造器
<bean id="user" class="com.zoux.pojo.User" c:name="zoux" c:age="18" />
```

## bean 作用域

```
单例模式， 永远只有一个对象
<bean id="user" class="com.zoux.pojo.User" c:name="zoux" c:age="18" scope="singleton"/>
```

```
原型模式 每次都会产生不同对象
<bean id="user" class="com.zoux.pojo.User" c:name="zoux" c:age="18" scope="prototype"/>
```

```
request session application 只能在web开发中使用
```

## bean的自动装配

```
spring满足bean依赖的方式
spring会在上下文中自动寻找，并自动给bean装配属性
```

```
在xml中显示配置
```

```
ByName 会自动在容器上下文中查找，和自己set方法后面的值对应的beanid  会将 dog cat 类自动装入
<bean id="user" class="com.zoux.pojo.User" autowire="byName">
	<property name="name" value="zoux" >
</bean>
```

```
ByType自动装配  会自动在容器上下文中查找，和自己对象属性相同的bean 会将 dog cat 类自动装入
<bean id="user" class="com.zoux.pojo.User" autowire="byType">
	<property name="name" value="zoux" >
</bean>
```

```
总结：
byname，需要保证所有的bean的id唯一，并且这个bean需要自动注入的属性的set方法的值与注入的bean的id一致！【setDog bean名dog】
bytype，需保证这个bean的id唯一，，并且bean需要和自动注入的属性的类型一致
```



```
在java中显示配置
```

```
通过注解装配
```

```
xml 导入约束
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/beans/spring-context-3.0.xsd
">
  <context:annotation-config/>
</beans>
```

```
@Autowired
属性上可以使用 set方法可以使用
使用@Autowired 可以不用set方法，前提是自动装配的属性在IOC（Spring）容器中存在，且符合名字byname
@Autowired 可以搭配 @Qualifier 指定唯一的bean/属性
```

```
@Nullable
表示该属性可为空
```

```
@Autowired(required = false) 表示此属性可以为空
```

```
@Resource(name="")
```

```
@Resource  与 @Autowired 
```

```
隐式自动装配
```

## 使用注解开发

```
1，bean
@Component 将此类变成bean
```

```
2，属性注入
@Value 赋值 
@Value("zoux")
private String name ；
```

```
3，衍生注解
@Component 衍生注解 web-三层架构
	dao  @Repository
	service  @Service
	controller  @Controller
```

```
4，自动装配
@Autowired  通过名字、类型自动注入
	不唯一，可使用@Qualifier(value="xxx")指定
@Nullable 可为null
@Resource 通过名字、类型自动注入
```

```
5，作用域
@Scope("prototype") 
```

```
6，小结
xml，功能全，试用任何场所
注解，不是自己的类使用不了，维护相对复杂

最佳实现
	xml，管理bean
	注解，只负责完成属性注入

使用注解，需要在xml配置文件里面开启注解支持
<context:component-scan base-package="com.zoux"/>
<context:annotation-config/>
```

## 使用Java的方式配置Spring

```
完全不需要使用spring的xml配置，全权交给java来做！
JavaConfig 是spring的一个子项目，在spring4之后，它成为一个核心功能！
```

```
config类
@Configuration  //这个类也会被容器接管
//@Configuration 代表这是一个配置类，与beans.xml 类似
@ComponentScan("com.zoux.pojo")
@Import(ZouxConfig2.class)
public class ZouxConfig {
    //注册bean
    //方法名字 bean的id
    //方法返回值 bean的class属性
    @Bean
    public User getUser() {
        return new User();
        //就是返回要注入的bean对象！
    }
}
```

```
实体类
@Data
@Component //这个类被spring接管了，注册到了容器中
public class User {
    @Value("zouxing")
    private String name;
}
```

```
测试
public class MyTest {
    public static void main(String[] args) {
        //
        ApplicationContext context = new AnnotationConfigApplicationContext(ZouxConfig.class);
        User getUser = (User) context.getBean("getUser");
        System.out.println(getUser.getName());
    }
} 
```

## 代理模式 AOP

```
为什么学习代理模式？
1，可以使真实角色的操作更加存粹，不用关注一些公共的业务
2，公共业务就交给代理角色，实现业务的分工
3，公共业务发生扩展的时候，方便集中管理
缺点：一个真实角色就会产生一代理类；代码量会翻倍～开发效率会变低
```

```
代理模式分类：
	静态代理
	动态代理
```

## 静态代理

角色分析：

- 抽象角色：一般会使用接口或抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色之后，会附加操作
- 客户：访问代理对象的人

```
改动原有的业务代码，在公司中是大忌！
```

![截屏2021-05-16 上午11.04.57](/Users/zouxing/Desktop/截屏2021-05-16 上午11.04.57.jpg)

## 动态代理

- 动态代理与静态代理角色一样
- 动态代理的代理类是动态生成的
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口--- JDK动态代理 【我们在这里使用】
  - 基于类：cglib
  - java字节码实现：javasist

需要了解两个类：Proxy：代理，InvocationHandler：调用处理程序

```
好处，
1，可以使真实角色的操作更加存粹，不用关注一些公共的业务
2，公共业务就交给代理角色，实现业务的分工
3，公共业务发生扩展的时候，方便集中管理
4，一个动态代理类代理的是一个接口，一般就是对应的一类业务
5，一个动态代理类可以代理多个类，只要实现了同一个接口
```

## spring AOP实现

```
导入依赖
<dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
        </dependency>
```

### 方式一：使用spring的API接口![截屏2021-05-16 下午8.32.52](/Users/zouxing/Library/Application Support/typora-user-images/截屏2021-05-16 下午8.32.52.jpg)

```
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">
    <!--    注册bean-->
    <bean id="userService" class="com.zoux.service.UserServiceImpl"/>
    <bean id="log" class="com.zoux.log.Log"/>
    <bean id="afterLog" class="com.zoux.log.AfterLog"/>
    <!--    使用原声spring APi接口-->
    <!--    配置aop:需要导入约束-->
    <aop:config>
        <!--        切入点 execution表达式(要执行的位置)-->
        <aop:pointcut id="pointcut" expression="execution(* com.zoux.service.UserServiceImpl.*(..) )"/>
        <!--        执行环绕增加！-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
```

```
public class Log implements MethodBeforeAdvice {

    @Override
    //method 要执行的目标对象的方法
    //args 参数
    //target 目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
    }
}

```

```
public class AfterLog implements AfterReturningAdvice {
    @Override
    //returnValue
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了" + method.getName() + "返回结果为：" + returnValue);
    }
}

```

```
public class UserServiceImpl implements UserService{

    @Override
    public void add() {
        System.out.println("增加了一个用户");
    }

    @Override
    public void update() {
        System.out.println("修改了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户");
    }

    @Override
    public void select() {
        System.out.println("查询了一个用户");
    }
}

```

```
public interface UserService {
    public void add();
    public void update();
    public void delete();
    public void select();
}
```

```
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //动态代理 代理的是一个接口
        UserService userService = (UserService) context.getBean("userService");
        userService.select();
    }
}

```

### 方式二：使用自定义类实现AOP![截屏2021-05-16 下午8.45.56](/Users/zouxing/Library/Application Support/typora-user-images/截屏2021-05-16 下午8.45.56.jpg)

```
public class DiyPointCut {
    public void before() {
        System.out.println("======方法执行前====");
    }

    public void after() {
        System.out.println("======方法执行后====");
    }
}

```

```
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">
    <!--    注册bean-->
    <bean id="userService" class="com.zoux.service.UserServiceImpl"/>
    <bean id="log" class="com.zoux.log.Log"/>
    <bean id="afterLog" class="com.zoux.log.AfterLog"/>

    <!--    方式二：自定义类-->
    <bean id="diy" class="com.zoux.diy.DiyPointCut"/>
    <aop:config>
        <!--        自定义切面，ref 要引用的类-->
        <aop:aspect ref="diy">
            <!--            切入点-->
            <aop:pointcut id="point" expression="execution(* com.zoux.service.UserServiceImpl.*(..))"/>
            <!--            通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point" />
        </aop:aspect>
    </aop:config>
</beans>
```

### 方式三：使用注解实现

```
//使用注解方式实现AOP
@Aspect  //标注这个类是一个切面
public class AnnotationPointCut {
    @Before("execution(* com.zoux.service.UserServiceImpl.*(..))")
    public void before() {
        System.out.println("====方法执行前======");
    }

    @After("execution(* com.zoux.service.UserServiceImpl.*(..))")
    public void after() {
        System.out.println("====方法执行后======");
    }

    //在环绕增强中，参数，代表我们要获取的切入点
    @Around("execution(* com.zoux.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint joinPoint) throws Throwable{
        System.out.println("环绕前");
        Signature signature = joinPoint.getSignature();
        System.out.println("signature:"+signature);
        //执行方法
        Object proceed = joinPoint.proceed();
        System.out.println("环绕后");

    }
}

```

```
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">
    <!--    注册bean-->
    <bean id="userService" class="com.zoux.service.UserServiceImpl"/>
    <bean id="log" class="com.zoux.log.Log"/>
    <bean id="afterLog" class="com.zoux.log.AfterLog"/>
    <!--    方式三-->
    <bean id="annotationPointCut" class="com.zoux.annotation.AnnotationPointCut"/>
    <!--    开启注解支持-->
    <aop:aspectj-autoproxy/>
</beans>
```

