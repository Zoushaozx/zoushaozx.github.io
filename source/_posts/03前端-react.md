---
title: react
date: 2021-03-13 13:01:29
tags:
---

# 创建项目-简书

```
npm install -g create-react-app 
create-react-app project
cd project
npm start
```

# 使用styled-components 管理样式

```
安装模块styled-components
在src目录下创建style.js
	引入reset.css样式 使用import { createGlobalStyle } from 'styled-components';
createGlobalStyle``包裹
在index.js引入style.js
```

# Header头部区块

---

## 文件准备

```
src
	common
		header
			index.js
			style.js
			
src创建common/header文件夹
	创建index.js
index.js 为Header 组件
创建style.js 只给Header使用
	import {    HeaderWrapper} from './style' 
	 <HeaderWrapper>
```

## Header组件结构

```
HeaderWrapper
	Logo
	Nav
		NavItem
	NavSearch	
	Addition	
		Button
```

## iconfont实现图标

```
	https://www.iconfont.cn/ 构建个人项目 下载到本地
	利用styled-components 将iconfont.css 全局引用
	在相应位置使用图标
```

## 动画效果

```
将获取焦点样式编写
将失去焦点样式编写
安装react-transition-group
	npm install react-transition-group
引入CSSTransition组件
使用组件，套用在动画区域
	<CSSTransition
    in={this.state.focused} //
    timeout={200}  //
    classNames="slide" //
    >
更改CSSTransition 注入的参数
	.slide-enter {
        width: 160px;
        transition: all .2s ease-out;
    }
    .slide-enter-active {
        width: 240px;
    }
    .slide-exit {
        transition: all .2s ease-out;
    }
    .slide-exit-active {
        width:160px;
    }
```

## react-redex数据管理

```
安装redux react-redux
	 npm install redux 
	 npm install react-redux 
	 
创建数据仓库 store/index.js&reducer.js
	index.js 数据仓库转接口
	reducer.js 数据仓库：定义数据 实现方法
	
连接redux 
	在根组件App引入redux
		import {Provider} from 'react-redux' 	
	在使用数据组件引入redux	
		import {connect} from 'react-redux'; 
		export default connect(mapStateToProps, mapDispatchToProps)(Header); 将redux 与组件绑定
		
改变数据接入点
	this.state.focused => this.props.focused
	
改变方法接入点
	this.state.handleInputFocus => this.props.handleInputFocus
	
去掉constructor		
	不需要绑定this 定义state属性
	
引入	mapStateToProps
	实现数据桥梁
	
引入 mapDispatchToProps	
	实现方法

经过上述操作，Header组件变为了一个无状态组件
	只是使用返回方法形式 能获得比较高的性能
		const Header = (props) => {
    	return ()
    }
    const mapStateToProps = (state) => {
      return {}
		}
    const mapDispatchToProps = (dispatch) => {
        return {}
    }
		export default connect(mapStateToProps, mapDispatchToProps)(Header);
```

## 改善redux 

```
将reducer拆分
	在相应组件文件夹新建store/index.js&reducer.js
	reducer.js 定义数据&方法
	index.js 返回reduc
修改src/store/	index.js&reducer.js
	index.js 
		引入compose 
		const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
		增加createStore入参 composeEnhancers()
	reducer.js
		引入combineReducers
		使用combineReducers 将组件reducer整合
			import {reducer as headerReducer } from '../common/header/store'
			const reducer = combineReducers({
          header: headerReducer
      });
将actionTypes 常量化
将action 定义抽离
```

## 回顾整体redux

```
文件目录
  src
    store
      index.js redux入口
      reducer.js 各个组件reducer整合
    common
      组件名
        store
          index.js 整体返回出口
          reducer.js 数据仓库：数据&方法
          actionTypes.js action类型常量
          actionCreators.js 创建action方法
        
组件使用redux
	App.js 引入 import {Provider} from 'react-redux'
		<Provider store={store}></Provider>
	组件 引入 import {connect} from 'react-redux';
			const Header = (props) => { return ( 
				具体使用 属性props.focused  方法onBlur={props.handleBlur} )}
			const mapStateToProps = (state) => { return {}}
			const mapDispatchToProps = (dispatch) => {    return {}}
			export default connect(mapStateToProps, mapDispatchToProps)(Header);
```

## 使用immutable.js库管理store数据

```
react 不允许改变state属性 使用immutable操作state
安装immutable
 npm install immutable  
引入immut 
	import {fromJS} from 'immutable';
将数据转化为immutable对象
  const defaultState = fromJS({
      focused: false
  }) 
通过set 改变数据
  //set 结合之前immutable对象 当前值 返回一个新对象
  return state.set('focused',true);  
通过get方法获得数据
   focused:state.header.get('focused')	     
```

## 使用redux-immuta统一数据格式

```
安装 redux-immutable
	npm install redux-immutable
更改 state初始化 方式
	src/reducer.js 
	引入 import {combineReducers} from 'redux-immutable'
	定义 reducer 
    const reducer = combineReducers({
    	  header: headerReducer
  	});
state调用方法
	const mapStateToProps = (state) => {
    return {
				focused:state.getIn(['header','focused'])
						}
					}
```

## 热门搜索样式布局

```
index.js
<SearchInfo>
    <SearchInfoTitle>
      热门搜索
    	<SearchInfoSwitch>换一批</SearchInfoSwitch>
    </SearchInfoTitle>
    <SearchList>
      <SearchInfoItem>教育</SearchInfoItem>
      <SearchInfoItem>简书</SearchInfoItem>
      <SearchInfoItem>生活</SearchInfoItem>
      <SearchInfoItem>投稿</SearchInfoItem>
      <SearchInfoItem>历史</SearchInfoItem>
      <SearchInfoItem>PHP</SearchInfoItem>
    </SearchList>
 </SearchInfo>
{this.showSearchInfo(this.props.focused)}  
style.js
	export const SearchList = styled.div``;	
```

## Ajax获取推荐数据

```
安装redux-thunk
	npm install redux-thunk   作用 actionCreators 可以返回一个函数
安装 axios	
	npm install axios 实现axios 请求
	
在src/store/index.js 
	引入 import thunk from "redux-thunk";
	引入 applyMiddleware import {createStore, compose,applyMiddleware} from 'redux';
	增加 createStore 参数 const store = createStore(reducer, composeEnhancers(applyMiddleware(thunk)));

在hander/index.js
	mapDispatchToProps 添加获取数据方法    dispatch(actionCreators.getList());
	mapStateToProps 添加数据	 list:state.getIn(['header','list'])
	
操作header/store/*
	actionTypes.js 
    添加action类型 
      export const CHANGE_LIST = 'header/CHANGE_LIST';
      
  actionCreators.js 
    将list 数据转换成immutable类型 
     	data: fromJS(data)
    使用axis实现数据请求
    	export const getList = () => {
    		return (dispatch) => {
    			axios.get('/api/headerList.json').then((resp) => {
            const data = resp.data;
            dispatch(changeList(data.data));
          }).catch(() => {
              console.log("error");
          })
    		}
    	}
	reducer.js
		定义数据 list  
    定义方法 
     	if (action.type === actionTypes.CHANGE_LIST) {
          return state.set('list',action.data);
        }   
虚拟数据
	在public/api文件夹下创建headerList.json
		返回json格式数据
```

## 代码优化

```
const {focused, list} = this.props;
	简化this.props.xxx调用
if语句可以使用switch代替	
```

## 换页功能

```
点击事件
	onClick={() => handleChangePage(page, totalPage)}
	
mapDispatchToProps点击事件响应
	handleChangePage(page, totalPAge) {
     if (page < totalPAge) dispatch(actionCreators.changePage(page + 1));
     else dispatch(actionCreators.changePage(1));
    }
    
actionTypes.js	
	export const CHANGE_PAGE = 'header/CHANGE_PAGE';
	
actionCreators.js	
	export const changePage = (page)=>({
    type: actionTypes.CHANGE_PAGE,
    page
	});
reduce.js
	case actionTypes.CHANGE_PAGE:
        return state.set('page', action.page);
```

## css动画效果

```
利用 ref 获取dom元素
	ref={(icon) => {this.spinIcon = icon}}
改变点击事件参数 onClick={() => handleChangePage(page, totalPage, this.spinIcon)}
更改方法参数 handleChangePage(page, totalPAge, spin)
获取dom元素角度
   let originAngle = spin.style.transform.replace(/[^0-9]/ig, '');
动画实现
	if (originAngle) {
     originAngle = parseInt(originAngle, 10);
     } else {
     originAngle = 0;
   }
  spin.style.transform = 'rotate(' + (originAngle + 360) + 'deg)';
样式
	spin {
        display:block;
        float:left;
        font-size:12px;
        margin-right:2px;
        transition:all .2s ease-in;
        transform-origin: center center;
    }
```

## 解决无必要的Ajax请求

```

```

