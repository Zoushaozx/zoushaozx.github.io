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

---

# 导航菜单功能

---

```
1⃣️开启路由模式 
	在Home.vue el-menu 标签里面加上router
2⃣️获取路由数组
	v-for="(item,index) in this.$router.options.routes" 
3⃣️隐藏路由，配置路由时，加上属性hidden
	hidden: true
4⃣️更改路由数组结构
	给路由添加子路由
		children: [
			{
          path: '/userinfo',
          name: '个人中心',
          component: AdminInfo
      }
		]
5⃣️利用vuex存储菜单，实际上存在内存里面
		安装 npm install vuex --save
		配置 vuex 项目根目录新建文件夹store并创建index.js
			index.js内容
				引入Vue、Vuex
				使用vuex Vue.use(Vuex)
				导出 Vuex.Store()
					state: {  //可以理解为是一个全局对象,用来保存所有组件的公共的数据
       		 routes:[]
          },
          mutations: {    //改变state值的方法 同步执行
              initRoutes(state, data) {  //初始化state里面的routes
                  state.routes = data;
              }
          },
          actions: {  //改变state值的方法 异步执行          }
    在main.js引入vuex
    	import store from "./store"
        new Vue({
          router,
          store,
          render: h => h(App)
        }).$mount('#app')
6⃣️获取数据并存入vues&格式化        
	新建工具类menus.js
  	引入 import { getRequest } from "./api"
		----------------------------
    导出 formatRoutes    	
        新建数组用于存放格式化后的路由
        循环传入的对象
				定义路由格式
					利用递归方式循环定义路由格式
						let {   
            path,   
            component,
            name,
            iconCls,
            children
              } = router;
              // 如果有 children存在 并且类型是数组
              if (children && children instanceof Array) {
                  // 递归
                  children = formatRoutes(children)
              }	
          具体格式化
          	let fmRouter = {
              path: path,
              name: name,
              iconCls: iconCls,
              children: children,
              component(resolve) { //进行格式化
                      require(['../views/' + component + '.vue'], resolve);
                  }
              }
		      插入到数组  
          ------------------
          导出 initMenu = (router, store)
            判断路由数据是否完成格式化并存储 存在直接返回
            通过getRequest('/system/cfg/menu').then(data) 获取菜单数据
              格式化好路由	let fmtRoutes = formatRoutes(data)
              添加到 router	router.addRoutes(fmtRoutes) 
              将数据存入 Vuex	store.commit('initRoutes',fmtRoutes)
          ---------------------
          完善新建工具类menus.js
          	在views下新建文件夹 emp per sal sta sys
          	根据数据库t_mune新建vue页面
          	修改	menus.js  单独对某一个路由格式化 component
          		如 判断组件以Home开头，到对应的目录去找
          			if (component.startsWith('Home')) {
                    require(['../views/' + component + '.vue'], resolve);
                }
7⃣️初始化菜单
	路由导航守卫 在main.js注册一个全局前置守卫
		// 使用 router.beforeEach 注册一个全局前置守卫
    router.beforeEach((to, from, next) => {
      // to 要去的路由; from 来自哪里的路由 ; next() 放行
      // 用户登录成功时，把 token 存入 sessionStorage，如果携带 token，初始化菜单，放行
      if (window.sessionStorage.getItem('tokenStr')) { 
        initMenu(router, store)
        next();
      } else {
        next();
        }
    	})
	展开菜单-独立展开
		el-menu 标签 加 unique-opened
		引入了计算属性 this.$router.options.routes 可用routes代替
			computed: { //计算属性
          // 从 vuex 获取 routes
          routes() {
            return this.$store.state.routes
          }
        }
    更改 el-submenu 标签里的 index=“1” :index = "index+''"
    引入font-awesome头像模版
    	安装 npm install  font-awesome
    	引入 import 'font-awesome/css/font-awesome.css'
			使用	:class="item.iconCls" 绑定class  iconCls为数据库字段存储头像编码
			样式  style="color: black;margin-right: 5px"
	特别注意：
  	路由格式化时Home目录为 equire(['../views/' + component + '.vue'], resolve);
```

---

# 获取用户信息功能实现

---

```
1⃣️修改项目标题
	加样式
2⃣️在main.js 路由前置守卫 将用户信息存入sessionStorage
	用户登录成功时，把 token 存入 sessionStorage，如果携带 token，初始化菜单，放行
	判断用户信息是否存在
		不存在
		存入用户信息，转字符串，存入 sessionStorage
3⃣️在Home.vue 数据域 里添加用户信息
4⃣️使用用户信息	 
	在 Home.vue {{ user.name }}<i><img :src="user.userFace"></i>
```

---

# 注销登录功能实现

---

```
1⃣️点击事件 Home.vue   el-dropdown 标签 添加点击事件入口
	@command="commandHandler"
2⃣️具体事件类型  Home.vue  el-dropdown-item 标签 添加时间标注
	command="logout"
3⃣️Home.vue 方法域 添加方法 commandHandler(command) {
	if (command === 'logout') {}	
	}	
	弹框提示用户是否要注销登录 this.$confirm("提醒信息",{}).then(() => {
	注销登录
	清空用户信息
	清空菜单信息  在src/utils/menus.js 中初始化菜单信息
	路由替换到登录页面
	}).catch(() => {
	已取消注销登录
```

---

# 面包屑导航栏效果

---

```
1⃣️在 Home.vue 标签el-main 添加
<el-breadcrumb separator-class="el-icon-arrow-right" v-if="this.$router.currentRoute.path!=='/home'">
   <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
   <el-breadcrumb-item>{{this.$router.currentRoute.name}}</el-breadcrumb-item>
 </el-breadcrumb>
 <div class="homeWelcome" v-if="this.$router.currentRoute.path==='/home'">
   欢迎来到云E办系统！
 </div>             
```

---

# 未登陆bug

---

```
1⃣️main.js
	判断携带 token 为 未携带
	更改访问路径
	if (to.path === '/') {
        next()
    } else {
        next('/?redirect=' + to.path)
    }
2⃣️在Login.vue submitLogin 中 
	页面跳转时
	拿到用户要跳转的路径
		let path = this.$route.query.redirect;
	用户可能输入首页地址或错误地址，让他跳到首页，否则跳转到他输入的地址
  	this.$router.replace((path === '/' || path === undefined) ? '/home' : path)

```

---

# 基础信息设置

---

```
1⃣️ 在SysBasic.vue   elementui 标签页 引入
		<template>
     <el-tabs v-model="activeName" type="card">
      <el-tab-pane label="职位管理" name="PosMenu">
      	<PosMenu></PosMenu>
      </el-tab-pane>
     </el-tabs>
    </template>	
    引入组件DepMenu
    	import DepMenu from '../../components/sys/basic/DepMenu'
      组件域  components: {DepMenu}	
2⃣️在src/components下新建文件夹sys 在sys下新建文件 PosMenu.vue
```

---

# 职位管理页面设计

---

```
1⃣️添加职位
  引入elementui 输入框
  引入elementui 按钮
  方法入口 点击/键盘entry
  数据域 定义json对象，用于访问后端接口
  方法域 访问后端接口 
2⃣️信息展示区域
	引入elementui 表格
	引入多选框 el-table-column的 type=selection
	引入操作 删除 编辑
	数据域 定义json对象，用于访问后端接口数据存贮
  方法域 访问后端接口 
  方法入口 vue生命周期 mounted 页面初始化就要展示的数据放里面
3⃣️删除操作
	确认提示
	访问后端节后
	刷新数据
4⃣️编辑操作
	引入对话框 
5⃣️批量删除操作
	引入按钮
```

---

# 职称管理界面设计

---

```
1⃣️添加职称
	
```

---

# 部门管理界面设计

---

```
1⃣️部门展示&搜索
	引入 树形控件-节点过滤 
	加入 图标prefix-icon="el-icon-search"
	数据 
		filterText 搜索字段
		deps[] 展示数据
		defaultProps 默认数据
	方法域 
  	initDeps 获取数据 getRequest  /system/basic/department/
  	filterNode 
  mounted 加载initDeps
  watch 监控filterText 调用 filterNode
2⃣️部门添加&删除  
	引入 自定义节点按钮 添加&删除
	样式 更改样式 按钮
	属性 :expand-on-click-node="false" 点击按钮不会展开树
	按钮 添加事件 添加/删除
	添加 删除添加部门方法
	引入 弹出框
	数据 dialogVisible
	添加 新方法showAddDep 里面开启弹出框
	数据 定义添加部门结构体
	引入 表格 用于展示添加数据 
	数据 定义上级部门名称
	更改 showAddDep获取上级部门名字和id
	更改 弹出框确定按钮 触发事件 为doAddDep具体添加操作
	方法 新方法doAddDep
	更改 doAddDep里请求后端接口 刷新部门数据 关闭弹出框
	方法 initDep 重置dep pname
	更改 doAddDep刷新部门数据调用方法为initDep
	方法 insertDepWithDeps 手动插入添加的部门信息到部门展示信息列表
	更改 insertDepWithDeps 循环部门信息数组 如果添加的部门的parentId与某个部门相同 将数据追加到他的children
  方法 deleteDep 确认删除 判断不为父部门 进行删除 更新部门数据 removeDepFromDeps
  方法 removeDepFromDeps 将删除的数据从部门展示信息拿走
  
```

---

# 操作员页面设计

---

```
1⃣️搜索
引入 输入框 按钮
2⃣️内容展示
引入 卡片	
数据 admin:[]
```

---

# 员工资料

---

```
1⃣️员工资料工具栏
2⃣️员工展示
3⃣️分页&普通搜索
4⃣️添加功能
5⃣️更新
6⃣️删除
7⃣️导入
8⃣️导出
9⃣️高级搜索
```

---

# 工资账套管理

---

```
1⃣️数据展示
2⃣️添加工资帐套
3⃣️更新工资帐套
```

---

# 员工帐套功能

---

```
1⃣️展示工资帐套
2⃣️修改工资帐套/添加工资帐套
```

---

# 在线聊天

---

```
1⃣️界面 开源项目vue-chat  https://github.com/Zoushaozx/vue-Chat-demo
2⃣️安装sass-loader
3⃣️web-socket
	安装 npm install sockjs-client
			npm install stompjs  
```

---

# 用户中心

---

```
1⃣️更新密码
	
```

