---
title: Java常用类
date: 2021-04-04 17:26:43
tags:
---

## StringBuffer

```
线程安全
StringBuffer buf = new StringBuffer("str");
append() 数据追加
	buf.append("").append("");
toString();  转化为String
insert(0，“”) 数据插入
delete(start,end); 数据删除
buf.reverse(); 内容反转
```

## StringBuilder

```
线程不安全
```

## CharSequence接口

```
本身是一个接口 是字符串类的 公共接口
```

## AutoCloseable接口

```

```

