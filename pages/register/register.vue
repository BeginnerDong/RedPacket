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
					<input type="text" v-model="submitData.phone" placeholder="请输入手机号" placeholder-class="placeholder">
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
			<view class="item flex mgb20">
				<view class="icon">
					<image src="../../static/images/the-login-icon1.png" mode=""></image>
				</view>
				<view class="input">
					<input type="password" v-model="submitData.passwordCopy" placeholder="请再次输入密码" placeholder-class="placeholder">
				</view>
			</view>
			<!-- <view class="item flex">
				<view class="icon">
					<image src="../../static/images/the-login-icon2.png" mode=""></image>
				</view>
				<view class="input">
					<input type="text" value="" placeholder="请输入验证码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="flexEnd mgt10">
				<view class="pubColor">获取验证码</view>
			</view> -->
			
			<view class="item loginbtn center flexCenter" style="margin-top: 80rpx;">
				<button class="btn" type="submint" @click="Utils.stopMultiClick(submit)">注册</button>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				wx_info:{},
				is_show:false,
				submitData:{
					phone:'',
					password:'',
					passwordCopy:''
				},
				
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
			uni.setStorageSync('canClick', true);
		},
		
		methods: {
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				
				if (pass) {	
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('手机格式不正确', 'none')	
						return
					};
					if(self.submitData.password != self.submitData.passwordCopy){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('两次输入密码不一致', 'none')
						return
					}
					self.register();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全注册信息', 'none')
				};
			},
			
			register() {
				const self = this;
				const postData = {};
				postData.data = {
					phone:self.submitData.phone,
					password:self.submitData.password
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('注册成功', 'none', 1000);
						uni.setStorageSync('user_token', data.token);
						uni.setStorageSync('user_info', data.info);
						setTimeout(function() {
							self.Router.reLaunch({route:{path:'/pages/index/index'}})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.register(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 60rpx;}	
	.loginBj image{width: 100%;height: 344rpx;}
	
	.loginCont{width: 80%;margin: 0 auto;position: relative;z-index: 2;background: #fff;padding: 60rpx 60rpx 80rpx 60rpx;box-sizing: border-box;margin-top: -120rpx;}
	.loginCont .item{border-radius: 40rpx;height: 80rpx;box-sizing: border-box;padding: 0 30rpx;border: 1px solid #eee;}
	.loginCont .item .icon image{width: 36rpx;height: 36rpx;margin-right: 30rpx;}
	.loginCont .item .input{width: 360rpx;}
	.loginCont .item .input input{font-size: 26rpx;}
	.loginCont .item.loginbtn{border:1px solid #F23132;overflow: hidden;background-color: #F23132;}
	.loginCont .item.loginbtn .btn{background-color:transparent;color: #fff;border: none;width: 100%;display: block;font-size: 26rpx;height: 100%;line-height: 78rpx;}
</style>
