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

# 登录页面&前后端联调

---

```
1⃣️引入element ui
	安装 npm i element-ui -S  -S 安装到项目目录里面，将依赖加入到正式环境依赖，
	引入 进入main.js  
		import ElementUI from 'element-ui';
		import 'element-ui/lib/theme-chalk/index.css';
2⃣️清理App.vue 删除 页面About Home 组件Hellworld
3⃣️views新建页面Login.vue
4⃣️配置路由 router/index.js
	导入 import Login from '../views/Login.vue'
	配置路由详情
  	{ path: '/', 默认渲染Login
      name: 'Login',
      component: Login
      }
  使用 Vue.use(ElementUI);    
5⃣️登录页面完善 
6⃣️校验数据
	规定 在el-form加入:rules="rules"规则绑定
	绑定 具体检验的数据绑定 el-form-item加入prop="username"
	校验 数据域
		rules: {username: [{required: true, message: '请输入用户名', trigger: 'blur'}]}
7⃣️登陆事件
	入口 登录按钮 绑定点击事件 @click="submitLogin"
	方法域 submitLogin() { 
	利用ref反射定位到loginForm
	使用validate方法进行校验
    this.$refs.loginForm.validate((valid) => {
      if (valid) {} 判断valid false校验不通过
    });
    }
```

```
8⃣️前后端联调
	安装axios 
		npm install axios
	新建文件夹utils
  	新建文件api.js
  api.js内容		
  	引入element-ui-Message import {Message} from "element-ui";
  	响应拦截器 - 统一处理消息提示，调用axios的interceptors.request.use
    	业务逻辑错误
    		后端：500 业务逻辑错误，401 未登录，403 无权访问；
    	成功访问	
    		成功访问可打印信息
    		成功访问可返回数据
     	没访问到后端接口
    		判断具体错误
    传送json格式的请求 post get put delete
    	格式
    		// 预备前置路径 大型项目用于区分资源
				let base = '';
    		export const postRequest = (url, params) => {
            return axios({
                method: 'post',
                url: `${base}${url}`,
                data: params
            })
        }
```

```
9⃣️跨域问题解决
	项目根目录新建vue.config.js
		导出对象devServer
			module.exports = { 
          devServer: {
              host: 'localhost',
              port: 8080,
              proxy: proxyObj // 代理对象
          }
      }
    代理对象
    	let proxyObj = {} // 代理对象
      proxyObj['/'] = {  //所有/都进行代理
          // websocket
          ws: false,
          // 代理目标地址
          target: 'http://localhost:8081',
          // 发送请求头 host 会被设置 target
          changeOrigin: true,  //target参数是域名
          // 不重写请求地址
          pathRewrite: {
              '^/': '/'
          }
      }
```

```
验证码前后端联调测试       
 	入口	<img :src="captchaUrl"  @click="updateCaptcha" >
 	数据	captchaUrl:'/captcha?time=' + new Date(),
  方法	updateCaptcha() {
          this.captchaUrl = '/captcha?time=' + new Date()
        }
```

```
登陆前后端联调测试
	加入加载动画 Login.vue
  	在el-form标签里面加入
  		v-loading="loading"
      element-loading-text="正在登录......"
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.8)"
    在数据域 加入  loading: false, //加载动画
    在方法域 登陆成功前 加this.loading=true 判断登陆成功后 this.loading=false
    
  封装请求和响应 utils/api.js
  	请求拦截器-将请求进行加工 axios.interceptors.request.use
  		登陆请求 将token存入sessionStorage
  			if (window.sessionStorage.getItem("tokenStr")) {
            // token 的key : Authorization ; value: tokenStr
            config.headers['Authorization'] = window.sessionStorage.getItem('tokenStr')
    			}
  	响应拦截器-统一处理消息提示 axios.interceptors.response.use
  	
  插件形式使用请求
		在main.js里面
			import {postRequest} from "./utils/api"
			Vue.prototype.postRequest = postRequest;  		
  处理登陆请求 Login.vue 方法域 submitLogin
  	登陆请求联调后端
  		使用utils/api.js/封装的postRequest
  			this.postRequest('/login',this.loginForm).then(resp=>{})
  				参数一，后端接口
  				参数二，前端登陆表单
  				then表示使用通讯框架方法进行接口访问
  				resp自定义的回调结果 用于判断登陆成功与否
      存储用户 token 到 sessionStorage
        const tokenStr = resp.obj.tokenHead + resp.obj.token
        window.sessionStorage.setItem('tokenStr', tokenStr)
```

