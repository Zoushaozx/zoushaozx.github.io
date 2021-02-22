---
title: git
date: 2021-02-12 15:55:40
tags:
---
# git 添加同户名

```
git config --global user.name "Zoushaozx" 
git config --global user.email "1216181745@qq.com"
```



# 新建密钥

```
ssh-keygen -o
三个回车

```

# 配置secret

```
git 个人配置 developer settings
personal access tokens
generate new token 

然后进入博客仓库
settings
secrets
new repository secret

```

# git推代码

```
github新建仓库
复制仓库地址
进入想要推代码的根目录
git init 
git commit -m "描述"		
git push --set-upstream git@github.com:Zoushaozx/yeb_back.git master
```

