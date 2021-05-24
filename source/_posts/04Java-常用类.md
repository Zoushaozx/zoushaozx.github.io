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



## collection

---

### 数组

```
缺点：
	长度不能修改
	只能存储单一类型元素
	数据增加麻烦
```

### 集合

```
可以动态的保存任意多个对象
add remove set get
```

```
集合框架体系图
```

![截屏2021-05-22 上午11.21.45](/Users/zouxing/Documents/hexo-blog/static/截屏2021-05-22 上午11.21.45.jpg)

```
Map框架体系图
```

![截屏2021-05-22 上午11.27.29](/Users/zouxing/Documents/hexo-blog/static/截屏2021-05-22 上午11.27.29.jpg)

## 集合解读

```
1，集合主要是两组，单列，双列集合
2，collection 接口有两个重要子接口 list set 他们的实现子类都是单列子类
3，map的实现子类 属于双列集合
```



## 迭代器iterator

```
itit 快速生成iterator while循环
command + j 查看 缩写

Iterator iterator = col.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }
```

## 增强for循环

```
底层iterator
for (Object o : col) {
            System.out.println(o);
        }
```

## List

```
1，实现List 都能够保持输入顺序
2，有顺序索引
3，内部元素有对应整数型序号记录位置
4，实现类，ArrayList LinkedList Vector
```

## list方法

```
add(下标,插入数据);  插入
addAll(下标,插入数据-多个); 插入
get(下标)  获取数据
indexOf("数据");  返回数据在集合首次出现的位置
lastIndexOf("数据") 返回数据在集合最后一次出现位置
remove(数据) 删除数据
set(1,"数据") 数据替换。索引不能越界
```

## 遍历方式

```
ArrayList LinkedList Vector

iterator
增强for
普通for
for(inti =0;i<list.size();i++){
	Object o = list.get(i);
	o.sout;
}
```

## ArrayList底层结构和源代码分析

```
ArrayList 能加入 null
底层是数组
ArrayList 线程不安全（效率高） 在多线程情况下 不建议使用 ArrayList
Vector 基本等同于 ArrayList 多线程使用
```

```
ArrayList 维护了 一个 Object 类型的数组 elementData
创建ArrayList 对象时，如果使用的是无参构造器，则初始elementData 容量为0 
	第一次添加，扩容为10，如果再次扩容 为原来的1.5倍
如果指定大小的构造器，初始elementData 容量为指定大小，扩容为原来1.5倍	
```

```
transient 表示瞬间 ，短暂的，表示该属性不会被序列化
```

## Vector底层结构和源代码分析

```
底层对象数组 protected Object[] elementData;
线程安全的
无参 默认10， 扩容2倍
有参 指定大小 扩容两倍
```

## LinkedList底层结构和源代码分析

```
底层 双向链表 双端队列
可以添加任意元素 元素可以重复 包括null
线程不安全 没有实现同步
添加删除效率很高
```

```
add 尾插
remove 头删
```

## ArrayList LinkedList 选用

```
改查比较多 ArrayList
增删比较多 LinkedList
```



## Set接口基本介绍

```
无序  添加顺序与取出顺序不一致  
无索引  
不允许重复数据，即只能存储一个null
```

## 常用方法

```
add
remove
set
iterator
增强for
```

## HashSet源码

```
底层是 HashMap 即 （数组+链表+红黑树）
```

```
1，HashSet 底层 HashMap 
2，添加一个元素时，先得到hash值 -会转成索引值
3，查看table相应索引位置 无数据直接加入 有数据调用 equals 比较 相同放弃添加 不相同就尾插
4，Java8 一条链表的元素个数到达 8  链表转为红黑树
```

```
开发技巧，在需要变量是才定义
```

