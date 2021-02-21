---
title: 云E办前端
date: 2021-02-21 09:36:40
tags:
---

# 技术栈

---

```
vue
element UI
vue_cli 项目搭建
vuex 状态管理
vuerouter 路由管理
axios 通讯框架
es6 前端语法
webpack 打包
websocket 在线聊天
vue_chat 在线聊天
font_awesome 字体
js-file-download 文件下载
```

----

# 环境准备

---

```
1⃣️Node.js安装
	http://nodejs.cn/download/
	安装淘宝镜像加速器
		npm install cnpm -g
	更改node源
  	npm install --registry=https://registy.npm.taobao.org
  npm源管理
  	nrm
  	npm install -g nrm
  		nrm ls展示源
```

```
2⃣️安装vue-cli
	安装 npm install -g @vue/cli
	验证 vue --version
```

```
3⃣️创建项目
	准备存放项目地址，进入该地址
	vue create 项目名
		选择Manually select features 回车
		选择 Babel Router 回车
		选择 vue 2.x 
    选择 不使用history模式
    选择 将配置信息存入package.json
    选择 不创建为模版
```



---

# Vue项目介绍

---

```
1⃣️目录分析
 node_modules #项目依赖文件 使用 npm-install 可安装依赖
  public #公用文件 
    favicon.ico #是vue图标
    index.html #是vue模版页面开发时用不到 
  src #项目源码目录
    assets #放资源的
    component #组件目录
    router #路由目录
    views #页面目录
    main.js #整个程序的入口	
  .gitignore #git忽略的配置文件
  babel.config.json #babel 将es6语法转换为es5 适配ie 
  package.json #整个项目的配置文件
  -----------
2⃣️关于 main.js疑问点
  	import Vue from 'vue'  import es6语法 babel打包时会转换成es5语法 require vue提供的模块加载器
    import App from './App.vue'
    import router from './router'
    Vue.config.productionTip = false 关闭浏览器控制台关于环境的相关的提示
    new Vue({ 实例化vue
      router,	router组件
      render: h => h(App) es6语法 渲染app组件，app是一开始通过import导入进来的
    }).$mount('#app') 手动挂载 等价 el: '#app',
			
		难点	render: h => h(App)	
		-------------------------------
		ES6的写法，其实就是如下内容的简写
      render: function (createElement) {  
       	return createElement(App);
  		}
  		createElement 是 Vue.js 里面的 函数
  		这个函数的作用就是生成一个 VNode节点，render 函数得到这个 VNode 节点之后返回给 Vue.js 的 mount 函数，渲染成真实 DOM 节点，并挂载到根节点上。
      render: function (createElement) {
        return createElement(
          'h' + this.level,   // tag name 标签名称
          this.$slots.default // 子组件中的阵列
        )
      }		
  	然后ES6写法,
  		render: createElement => createElement(App)
  	然后用h代替createElement，使用箭头函数来写：
  		render: h => h(App)
  	h的涵义，尤雨溪在一个回复中提到
  	createElement 函数是用来生成 HTML DOM 元素的，而上文中的 Hyperscript也是用来创建HTML结构的脚本，这样作者才把 createElement 简写成 h。
  	而 createElement(也就是h)是vuejs里的一个函数。这个函数的作用就是生成一个 VNode节点，render 函数得到这个 VNode 节点之后，返回给 Vue.js 的 mount 函数，渲染成真实 DOM 节点，并挂载到根节点上。
  -------------------------------
  
	难点 	.$mount('#app')
	这里创建的vue实例没有el属性，而是在实例后面添加了一个$mount(’#app’)方法
	$mount(’#app’) ：手动挂载到id为app的dom中的意思
	当Vue实例没有el属性时，则该实例尚没有挂载到某个dom中；
	假如需要延迟挂载，可以在之后手动调用vm.$mount()方法来挂载
	该方法是直接挂载到入口文件index.html 的 id=app 的dom 元素上的
	----------
3⃣️关于 App.vue疑问点
	<template>  vue模版标签 会替换 main.js 引入app的内容
  <router-link to=""> 类似html a标签 to类似 href
  <router-view/> 用于渲染路由匹配到的组件
  --------------
4⃣️router/index.js解析
	mport Vue from 'vue' 导入node_modules里面的vue
	import VueRouter from 'vue-router'导入node_modules里面的vue-router
	Vue.use(VueRouter) 安装路由
	const routes = [   路由数组
    { 配置路由 
    	path: '/',  跳转路径
      name: 'Home', 路由名称
      component: Home 路由要跳转的一个组件
      }
  ]     
  component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
		es6语法 lebda表的式
			功能为，动态的导入，在使用时才导入组件，类似懒加载
-------------- --
    const router = new VueRouter({  router实例化
     routes
		})  
		export default router 导出router 可供其他地方导入路由
5⃣️vue单页面讲解
	<template>  页面内容
	</template>
    <script> 可定义数据、方法、可导入组件、可自定义组件等
      import HelloWorld from '@/components/HelloWorld.vue' 可导入组件
      export default { 可自定义组件等 可形成组件树
        name: 'Home',
        components: {
          HelloWorld
          props: { 参数
              msg: String  使用msg时 在template区域 {{ msg }}
            }
          }
    	}
		</script>
		<style scoped></style> 网页样式，scope但页面使用
6⃣️main.js程序入口	
```

---

