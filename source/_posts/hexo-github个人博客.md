---
title: hexo-github个人博客
date: 2021-02-12 15:56:24
tags:
---

安装hexo

```
sudo npm install -g hexo-cli
```

初始化博客

```
选择一个权限较大的文件夹
hexo init hexo-blog
```

运行博客

```
进入博客文件夹
hexo server
```

hexo文件目录介绍

```
scaffolds md模版
source 文章和页面md
themes 主题样式
	_comfig.yml 主题相关配置
_comfig.yml hexo应用核心配置
```

替换主题

```
github搜索：hexo-theme 
确定在博客根目录，
git clone https://github.com/probberechts/hexo-theme-cactus.git themes/cactus
进入主题将里面的.git文件夹删除，避免与外部git冲突
修改根目录下的_comfig.yml文件
	找theme
主题下的source里面的	_comfig.yml
可以改样式
```

将博客部署到github仓库

```
方式一：
	https://[username].github.io做访问路径 
		打包产物必须部署到master分枝上
		仓库名必须为[username].github.io
		适合用于个人博客
方式二：
	https://[username].github.io/[repo] 	
		可以自定义仓库名
		打包产物 gh-pages
		比较适合用于开源项目
		
操作：
	新建仓库，命名：zoushaozx.giuhub.io
	初始化本地仓库 git init
	添加远程地址 
		git remote add origin https://github.com/Zoushaozx/zoushaozx.giuhub.io.git
	安装依赖，npm install hexo-deployer-git --save 将生成的代码部署到一个具体的分支
	配置博客根目录_comfig.yml 
    deploy:
      type: 'git'
      repo: https://github.com/Zoushaozx/zoushaozx.giuhub.io
      branch: master
	hexo deploy/npm run deploy
这儿有一个大坑，安装hexo-deployer-git，应该使用
cnpm install hexo-deployer-git --save
我折腾一晚上

```

自动化部署前提：前提配置secret

```
git add .
git commit -m "first commit"
新建分支myblog
git push --set-upstream origin myblog

使用gitactions
在项目里面新建文件夹.github/workflows
	在里面新建文件deploy.yml

每次修改代码，推送到myblog即可，会自动化部署
推送代码：
git add .
git commit -m "dscription"
gp(git push)
```

添加博客在线编辑

```
进入主题模版页面，post.ejs
在内容最下方添加跳转标签
<div> 
    <a href="https://github.com/Zoushaozx/zoushaozx.github.io/edit/myblog/source/<%- page.source %>" target="_blank">编辑</a>
</div>
```

更改关于/项目页面

```
hexo new page about/project
就会在source页面产生相应文件夹
编辑里面的index.md就能够达到自己想要的结果

```

