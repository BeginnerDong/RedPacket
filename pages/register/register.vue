<template>
	<view>
		
		<view class="loginBj pr">
			<image src="../../static/images/rob-img.png" mode=""></image>
		</view>
		
		<view class="loginCont radius10 boxShaow">
			<view class="item flex mgb20">
				<view class="icon">
					<image src="../../static/images/the-login-icon.png" mode=""></image>
				</view>
				<view class="input">
					<input type="text" value="" placeholder="请输入手机号" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flex mgb20">
				<view class="icon">
					<image src="../../static/images/the-login-icon1.png" mode=""></image>
				</view>
				<view class="input">
					<input type="password" value="" placeholder="请输入密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flex">
				<view class="icon">
					<image src="../../static/images/the-login-icon2.png" mode=""></image>
				</view>
				<view class="input">
					<input type="text" value="" placeholder="请输入验证码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="flexEnd mgt10">
				<view class="pubColor">获取验证码</view>
			</view>
			
			<view class="item loginbtn center flexCenter" style="margin-top: 80rpx;">
				<button class="btn" type="submint" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">注册</button>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 60rpx;}	
	.loginBj image{width: 100%;height: 344rpx;}
	
	.loginCont{width: 80%;margin: 0 auto;position: relative;z-index: 2;background: #fff;padding: 50rpx 60rpx 80rpx 60rpx;box-sizing: border-box;margin-top: -120rpx;}
	.loginCont .item{border-radius: 40rpx;height: 80rpx;box-sizing: border-box;padding: 0 30rpx;border: 1px solid #eee;}
	.loginCont .item .icon image{width: 36rpx;height: 36rpx;margin-right: 30rpx;}
	.loginCont .item .input{width: 360rpx;}
	.loginCont .item .input input{font-size: 26rpx;}
	.loginCont .item.loginbtn{border:1px solid #F23132;overflow: hidden;background-color: #F23132;}
	.loginCont .item.loginbtn .btn{background-color:transparent;color: #fff;border: none;width: 100%;display: block;font-size: 26rpx;height: 100%;line-height: 78rpx;}
</style>
