---
title: uniapp
date: 2021-04-08 10:09:17
tags:
---

## 安装HBuilderX

## 创建项目



## 微信登录app端

```
前期准备：
	微信开放平台：
    appid
    appsecrut
		ios端：额外需要 Universal Links 需要在微信开放平台填写 然后配置在manifest.json
		
uniapp登录页面
<!-- 第三方登录H5不支持，所以隐藏掉 -->
		<!-- #ifndef H5 -->
		<view class="third-wapper">
			<view class="third-line">
				<view class="single-line"><view class="line"></view></view>

				<view class="third-words">第三方账号登录</view>

				<view class="single-line"><view class="line"></view></view>
			</view>

			<view class="third-icos-wapper">
				<!-- 5+app 用qq/微信/微博 登录 小程序用微信小程序登录 h5不支持 -->
				<!-- #ifdef APP-PLUS -->
				<image src="../../static/icos/weixin.png" data-logintype="weixin" @click="appOAuthLogin" class="third-ico"></image>
				<image src="../../static/icos/QQ.png" data-logintype="qq" @click="appOAuthLogin" class="third-ico" style="margin-left: 80upx;"></image>
				<image src="../../static/icos/weibo.png" data-logintype="sinaweibo" @click="appOAuthLogin" class="third-ico" style="margin-left: 80upx;"></image>
				<!-- #endif -->
			</view>
			<!-- #ifdef MP-WEIXIN -->
			<button open-type="getUserInfo" @getuserinfo="wxLogin" class="third-btn-ico"></button>
			<!-- #endif -->
		</view>
		<!-- #endif -->
		
uniapp处理登陆事件
	appOAuthLogin(e) {
			var _this = this;
			//获取用户登陆类型
			var loginType = e.currentTarget.dataset.logintype;
			//授权登录
			uni.login({
				provider: loginType,
				success: res => {
					// 授权登陆成功之后 获取 用户信息
					uni.getUserInfo({
						provider: loginType,
						success(loginRes) {
							
						}

访问微信端
https://api.weixin.qq.com/sns/jscode2session?
appid=&
secret=&
js_code=JSCODE&
grant_type=authorization_code

通过Map 拼接出上述字符串
String url = "https://api.weixin.qq.com/sns/jscode2session";
Map<String,String> param = new HashMap<>();
param.put("appid",appid);
param.put("secret",secret);
param.put("js_code",js_code);
param.put("grant_type",grant_type);

//链接微信端 返回字符串形式数据
String wxResult = httpclientUtil.doGet(url,param);

//转换数据格式
session_key
openid 主要关注openid 需要保存到数据库

//登陆时查询用户 判断openid是否存在 这个openid是唯一的
存在就进行登陆 通过openid 查询
不存在就进行注册（openid，用户对象）
```



