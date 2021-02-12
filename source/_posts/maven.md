---
title: maven
date: 2021-02-12 20:06:38
tags:
---

安装maven

```
http://maven.apache.org/download.cgi
下载maven3.6.3
cd /usr/local/
sudo tar -zxvf ~/Downloads/apache-maven-3.6.3-bin.tar.gz

添加环境变量~/.zshrc
export MAVEN_HOME=/usr/local/apache-maven-3.6.3
export PATH=$ PATH:$ MAVEN_HOME/bin

可能你的环境变量操作 /etc/profile就行
```

maven换源

```
新建文件夹用于存放maven下载的依赖包 repository
修改maven目录下的conf/setting.xml
在</ mirrors> 内部添加
	<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf>
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
	</mirror>
```

修改仓库位置：

```
<localRepository>你新建的存放位置</localRepository>
```

