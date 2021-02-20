---
title: 云服务器&linux
date: 2021-02-12 15:56:56
tags:
---
查找文件在什么地方

```
 find / -name node
 which node
```

查看日志

```
tail -100f test.log      实时监控100行日志

tail  -n  10  test.log   查询日志尾部最后10行的日志;

 tail -n +10 test.log    查询10行之后的所有日志;
```

查看端口运行情况

```
 netstat -nultp
```

安装wget

```
yum install -y wget
```

删除相相关软件

```
rpm -q -a
可查询到当前系统中安装的所有的软件包
rpm -e [package name]
确定了要卸载的软件的名称，就可以开始实际卸载该软件了
rpm -e [package name] --nodeps
忽略依赖关系的卸载可能会导致系统中其它的一些软件无法使用
rpm -ql [package name]
知道rpm包安装到哪里
```

文件上传

```
输入rz命令，看是否已经安装了lrzsz
yum   -y  install  lrzsz命令进行安装
rpm -qa lrzsz
rz -y选择文件
```

fdfs

```
https://www.cnblogs.com/qiaolizhi/p/12461901.html
```

