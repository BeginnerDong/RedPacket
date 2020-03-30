<template>
	<view>
		
		<view class="myRowBetween pdlr4 whiteBj">
			<!-- <view class="item flexRowBetween">
				<view class="ll">头像</view>
				<view class="rr flexEnd">
					<view class="userPhoto" @click="upLoadImg('mainImg')" v-if="submitData.mainImg.length>0">
						<image :src="submitData.mainImg[0].url" mode=""></image>
					</view>
					<view class="userPhoto" @click="upLoadImg('mainImg')" v-else>
						<image  src="../../static/images/set-up-the-img.png" mode=""></image>
					</view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image>
				</view>
			</view> -->
			<view class="item flexRowBetween">
				<view class="ll">手机号</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">{{userInfoData.phone}}</view>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-sex/user-setUp-sex?name='+userInfoData.gender}})" >
				<view class="ll">性别</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">{{userInfoData.gender!=0?(userInfoData.gender==1?'男':'女'):'请选择性别'}}</view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-Birthday/user-setUp-Birthday?name='+userInfoData.birthday}})" >
				<view class="ll">生日</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">{{userInfoData.birthday!=''?userInfoData.birthday:'请选择生日'}}</view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-adrs/user-setUp-adrs?name='+userInfoData.address+'&passage1='+userInfoData.passage1}})" >
				<view class="ll">地区</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">{{userInfoData.address!=''?userInfoData.address:'请选择地区'}}</view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-password/user-setUp-password'}})" >
				<view class="ll">修改密码</view>
				<view class="rr flexEnd">
					<!-- <view class="color9 fs13">{{password}}</view> -->
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
		</view>
		
		<view class="quitBtn boxShaow center fs15" @click="loginOff">退出登录</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				submitData:{
					mainImg:[]
				},
				userInfoData:{},
				password:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.password = uni.getStorageSync('user_info').password;
			//self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('userInfo').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						self.submitData.mainImg = self.userInfoData.mainImg;
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						self.userInfoUpdate()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						var file = res.tempFiles[0];
						var obj = res.tempFiles[0].path.lastIndexOf(".");
						var ext = res.tempFiles[0].path.substr(obj+1);
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getUserToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'headImg'
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('userInfo').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.getUserInfoData()
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			loginOff(){
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确定要退出登陆吗？',
					confirmColor:"#f23132",
					success: function(res) {
						if (res.confirm) {
							uni.removeStorageSync('user_info');
							uni.removeStorageSync('user_token');
							self.Router.reLaunch({route:{path:'/pages/index/index'}})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
				
			},

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.userPhoto{width: 80rpx;height: 80rpx;border-radius: 50%;border: 1px solid #eee;box-sizing: border-box;overflow: hidden;}
	.userPhoto image{width: 100%;height: 100%; display: block;}
	.quitBtn{width: 80%;margin: 120rpx auto 0 auto;background-color: #fff;border-radius: 10rpx;line-height: 80rpx;}
</style>
