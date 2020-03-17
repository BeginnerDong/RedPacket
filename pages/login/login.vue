<template>
	<view>
		
		<view class="loginBj pr">
			<image src="../../static/images/login-img.png" mode=""></image>
		</view>
		
		<view class="loginCont radius10 boxShaow">
			<view class="item flex mgb20">
				<view class="icon">
					<image src="../../static/images/the-login-icon.png" mode=""></image>
				</view>
				<view class="input">
					<input type="text" v-model="submitData.login_name" placeholder="请输入手机号" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flex mgb20">
				<view class="icon">
					<image src="../../static/images/the-login-icon1.png" mode=""></image>
				</view>
				<view class="input">
					<input type="password" v-model="submitData.password" placeholder="请输入密码" placeholder-class="placeholder">
				</view>
			</view>
			
			<view class="item loginbtn mgb20 center pubBj white flexCenter" style="margin-top: 140rpx;" @click="submit">
				<button class="btn" type="submint">登录</button>
			</view>
			<view class="item center flexCenter registerBtn"  @click="Router.navigateTo({route:{path:'/pages/register/register'}})">注册</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					login_name:'',
					password:''
				}
			}
		},
		
		onLoad() {
			const self = this;
			uni.hideLoading();
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('user_token', res.token);
							uni.setStorageSync('user_info', res.info);
							setTimeout(function() {
								self.Router.reLaunch({route:{path:'/pages/index/index'}})
							}, 1000);
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.userLogin(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
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
	.loginCont .item.registerBtn{border:1px solid #F23132;color: #F23132;}
</style>
