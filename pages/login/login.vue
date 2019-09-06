<template>
	<view class="container">
	  <!-- #ifdef MP-WEIXIN -->
	  <button type="primary" open-type="getUserInfo" @getuserinfo="getuserinfo" withCredentials="true">微信登录</button>
	  <!-- #endif -->
	  <!-- #ifdef APP-PLUS -->
	  <button type="primary" open-type="getUserInfo" @click="getuserinfo" withCredentials="true">微信登录</button>
	  <!-- #endif -->
	  <button style="margin-top:50px;">手机号码登录</button>
	</view>
</template>

<script>
	export default {
		methods:{
			getuserinfo(res1){
				wx.login({
					success(res2) {
						wx.request({
							url: 'https:///hoa.hcoder.net/xcxencode/?c=sk&appid=wxbc7692f7c6706fd3&secret=739b970b832f0df158f54c494a08e440&code='+res2.code,
							success(res3){
								console.log(res3);
								try{
									uni.setStorageSync('sk', res3.data.session_key);
									uni.setStorageSync('openid', res3.data.openid);
								}catch(e){
									//TODO handle the exception
								}
								uni.hideLoading();
								//以下步骤可以获取加密信息，需要授权
							   //获取加密信息
							   if(!res1.detail.iv){
								uni.showToast({
								 title:"您取消了授权,登录失败",
								 icon:"none"
								});
								return false;
							   }
							   try{
								var sessionKey = uni.getStorageSync('sk');
								console.log(sessionKey);
							   }catch(e){
								//TODO handle the exception
							   }
							   uni.request({
								/**
								* $appid         = $_POST['appid'];
								 $sessionKey    = $_POST['sessionKey'];
								 $encryptedData = $_POST['encryptedData'];
								 $iv            = $_POST['iv'];
								*/
								method : "POST",
								url : 'https:///hoa.hcoder.net/xcxencode/',
								header : {'content-type':'application/x-www-form-urlencoded'},
								data : {
								 appid : "wxbb7f9f1f2c6f4f33",
								 sessionKey : sessionKey,
								 iv : res1.detail.iv,
								 encryptedData : res1.detail.encryptedData
								},
								success:function(res4){
								 //"﻿﻿﻿{"openId":"oS6of0V0rdp9nY_BuvCnQUasOHYc","nickName":"深海",
								 //"gender":1,"language":"zh_CN","city":"Xi'an","province":"Shaanxi",
								 //"country":"China","avatarUrl":"https://wx.qlogo.cn/mmopen/vi_32/7iags6YD4enyU"
								 console.log(res4);
								 //至此登录完成
								}
							   });
							}
						})
					}
				})
			}
		}
	}
</script>

<style>
</style>
