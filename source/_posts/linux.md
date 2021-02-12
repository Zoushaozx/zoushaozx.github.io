---
title: linux
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

