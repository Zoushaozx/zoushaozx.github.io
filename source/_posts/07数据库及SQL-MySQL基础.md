---
title: 数据库及SQL/MySQL基础
date: 2021-03-02 16:46:50
tags:
---

# SQL结构化查询语言

---

```
DQL 数据查询语言 从表中获得数据
DML 数据操作语言 添加 修改 删除表中的行
TPL 事务处理语言 
DCL 数据控制语言 确定单个用户和用户组对数据库对象的访问 控制表单个列的访问
DDL 数据定义语言 新建表 删除表
CCL 指针控制语言 
```

```
数据库概述
	关系型数据库 - 表
	RDBMS 关系型数据库管理系统 管理员 仓库
  database = N个 table
  table：
  	表结构 定义表的列名和列类型
  	表 一行一行的记录
```

```
DDL 数据定义语言
	show databases 显示所有数据库名称
	use 数据库名称 切换数据库
	create database if exists 数据库名称 charset=utf8；新建数据库
	drop database if exists 数据库名 输出数据库
	alter database 数据库名 character set utf8
```

```
数据类型
	浮点型
		double(5,2)最大5位，小数2位
		decimal用于金额
	整型 int	
	char 255 固长字符串类型
	varchar 65535 变长字符串类型
	text 字符串类型
	二进制blob
  时间 
  	date yyyy- MM-dd
  	time hh:mm:ss
  	timestamp 时间戳类型
```

```
DDL 数据定义语言 操作数据表
创建表
	create table if exists zoux(
	列名 列类型,
	...
	列名 列类型
	)
查看所有表
	show tables;
查看表结构
	desc 表名；
删除表
	drop table 表名;
修改表
	添加列 alter table 表名 add(列名 列类型)
	修改列类型 alter table 表名 modify 列名 列类型；
  修改列名 alter table 表名 change 原列名 新列名 列类型；
  删除列 alter table 表名 drop 列名；
  修改表名称 alter table 原表名 rename to 新表名；
```

```
DML数据操作语言	
	运算符 = != <> > < >= <= between...and...  in(...) not or and
	插入数据	insert into 表名（列名1，列名2,...）values(列值1，列值2，...)
  				 insert into 表名 values(列值1，列值2，...)
	修改数据	update 表名 set 列名=数据 条件 where 列名=数据 or 列名=数据
  删除数据	delete from 表名 where 条件
```

```
DCL 数据控制语言
理解：一个项目创建一个用户，一个项目对应的数据库只有一个
		这个用户只能对这个数据库有权限，其他数据库你就操作不了
创建用户 create user 用户名@ip地址 identified by '密码' 用户只能在指定的ip地址商登陆
				create user 用户名@'%' identified by '密码' 不限制IP地址
用户授权	grant 权限1,...权限n on 数据库.* to 用户名@ip地址
				 grant create,alter,drop,insert,update,delete,select on 数据库名.* to 用户名					@ip地址
         grant all on 数据库名.* to 用户名@ip地址 授予全部权限
撤销权限	revoke 权限1，..权限n on 数据库.* from 用户名@ip地址
查看权限	show grant for 用户名@IP地址
删除用户	drop user 用户名@IP地址
```

```
DQL数据查询语言 基础查询
查看整张表	select * from 表名; 
列运算 select 列名*1.5 from 表名;
连接字符串 select concat('我的名称是'，列名) from emp;
处理null值 select 列名+ifnull(列名,0) from 表名;
别名 select 列名 别名 from 表名;
去除完全重复的行 select distinct 列名 from 表名； 
```

```
DQL数据查询语言 条件查询
select * from 表名 where 列名 between 范围1 and 范围2；
select * from 表名 where 列名 in('列名','列名');
```

```
DQL数据查询语言 模糊查询
关键字 like 占位符号 _匹配一个字符同时限定长度 %不限至长度，匹配1-n个字符
查询带有‘小’的记录 select * from 表名 where 列名 like '%小%';
查询带长度为三，中间为‘小’ select * from 表名 where 列名 like '—小—';
```

```
DQL数据查询语言 排序
关键字 order by asc 升序 desc 降序
select * from 表名  order by 列名 asc 列名 desc;
```

```
DQL数据查询语言 聚合函数
select count(*) 人数,sum(列名) 总和,max(列名) 最高，min(列名) 最低，avg(列名) 平均 from 表名；
```

```
DQL数据查询语言 分组查询
group by
select 列名 ，count(*) from 表名 where 条件 group by 列名 having 条件2（count(*) >2）;
```

```
DQL数据查询语言 limit方言
select * from 表名 limit 0，10；
0=（当前页-1）*10
10 显示记录数
```

