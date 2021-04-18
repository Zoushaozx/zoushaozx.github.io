---
title: SQL进阶及查询练习
date: 2021-03-03 10:03:51
tags:
---

# mysql编码问题

---

```
解决小黑框查询显示问题
```

---

# 约束

---

```
主键约束
主键 非空 唯一 可引用
创建约束	empno int primary key
				 alter table 表名 add primary key （列名）
删除约束	alter table 表名 drop primary key；         
```

```
主键自增长
	create table emp(
    empno int primary key auto_increment,
    ename varchar(50)
    );
auto_increment 类型必须为int
使用uuid为主键
```

```
非空约束 not null
唯一 unique
ename varchar(50) not null unique
```

---

# 关系模型

---

```
一对一 主键等于外键
create table hasband (
hid int primary key auto_increment,
hname varchar(50)
);

create table wife(
wid int primary key auto_increment,
wname varchar(50),
constraint fk_wife_hasband foreign key(wid) references hasband(hid)
);
```

```
多对多 利用中间表存储关系
create table stu
(
sid int primary key auto_increment,
sname varchar(50)
);

create table tea
(
tid int primary key auto_increment,
tname varchar(50)
);

create table stu_tea(
sid int ,
tid int,
constraint fk_student foreign key(sid) references stu(sid),
constraint fk_teacher foreign key(tid) references tea(tid)
);
```

---

# 多表查询

---

```
合并结果集
	结果集列的类型和列数相同
	union 去除重复行
	union all 不去除重复行
select * from 表名
union all 
select * from 表名
```

```
内连接  方言
select e.ename,e.sal,d.dname
from emp e ,dept d
where e.deptno =d.deptno;

内连接 标准
select  e.ename,e.sal,d.dname
from emp e ,dept d
on e.deptno =d.deptno;

内连接 自然连接
select e.ename,e.sal,d.dname
from emp e ,dept d；
```

```
外连接 左外连接
select e.ename,e.sal,d.dname
from emp e left outer join dept d
on e.deptno =d.deptno

外连接 右外连接
select e.ename,e.sal,d.dname
from emp e right outer join dept d
on e.deptno =d.deptno

外连接 全连接
select e.ename,e.sal,d.dname
from emp e left outer join dept d
on e.deptno =d.deptno
union
select e.ename,e.sal,d.dname
from emp e right outer join dept d
on e.deptno =d.deptno
```

```
子查询
位置
	where 
	from
条件
	单行单列 
	select * from 表名 别名 where 列名 [= > < >= <= !=] (select * from 表名 别名 where 条件)
	
	多行单列 
	select * from 表名 别名 where 列名 [in all any] (select * from 表名 别名 where 条件)
	
	单行多列
	select * from 表名 别名 where (列名，列名) in (select * from 表名 别名 where 条件)
	
	多行多列
	select * from 表名 别名,(select * from 表名 别名 where) 别名 where 条件； 
```

