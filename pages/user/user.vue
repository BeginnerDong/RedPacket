<template>
	<view>
		
		<view class="userHead pdlr4 pubBj white">
			<view class="infor flex pdb20">
				<image class="photo" @click="upLoadImg('mainImg')" v-if="submitData.mainImg.length>0" 
				:src="userInfoData.mainImg[0].url" mode=""></image>
				<image class="photo" src="../../static/images/about-img.png"  mode="" @click="upLoadImg('mainImg')" v-else></image>
				<view style="width: 70%;">
					<view class="fs16 pdb5">{{userInfoData.phone}}</view>
				</view>
			</view>
			<view class="flexRowBetween center headNum">
				<view class="item">
					<view class="mny ftw">{{userInfoData.balance}}</view>
					<view>账户余额</view>
				</view>
				<view class="item">
					<view class="mny ftw">{{userInfoData.yesterday?userInfoData.yesterday.count:''}}</view>
					<view>昨天收益(元)</view>
				</view>
			</view>
		</view>
		
		
		<view class="myRowBetween fs13 mglr4">
			<view class="item flexRowBetween">
				<view class="flex">
					<view class="fs12 color9 mgr15">可提现金额</view>
					<view class="fs16">{{userInfoData.balance}}</view>
				</view>
				<view class="txBtn" @click="Router.navigateTo({route:{path:'/pages/user-cashOut/user-cashOut'}})">提现</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/record/record'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon.png" mode=""></image>
					<view class="">抢红包记录</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp/user-setUp'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon1.png" mode=""></image>
					<view class="">设置</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/aboutUs/aboutUs'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
					<view class="">关于我们</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-share/user-share'}})">
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
					<view class="">去分享</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/Ranking/Ranking'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">红包排名</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				userInfoData:{},
				submitData:{
					mainImg:[]
				},
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData','getLabelData'], self);
		},
		
		methods: {
			
			getLabelData() {
				const self = this;
				self.bannerData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'分享'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0]
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			copy(){
				const self = this;
				uni.setClipboardData({
				    data: self.labelData.url,
				    success: function () {
				        self.$Utils.showToast('分享链接已复制', 'none')
				    }
				});
			},

			getUserInfoData() {
				const self = this;
				var yesterday = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000-86400;
				var today = new Date(new Date().setHours(0, 0, 0, 0)).getTime() / 1000;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('userInfo').user_no
				};
				postData.getAfter = {
					 yesterday: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							type:2,
							withdraw:0,
							create_time:['between',[yesterday,today]]
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'count',
						    {
						      status:1,
						      behavior:0,
						      type:2,
							  create_time:['between',[yesterday,today]]
						    }
						  ]
						},
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						self.userInfoData.phone = self.userInfoData.phone.replace(/^(\d{3})\d{4}(\d+)/,"$1****$2")
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
					if(res){
						if (res.solely_code == 100000) {
							self.submitData[type] = [];
							self.submitData[type].push({url:res.info.url,type:'image'})
							console.log(self.submitData)
							self.userInfoUpdate()
						} else {
							self.$Utils.showToast('网络故障', 'none')
						}
					}else{
						self.$Utils.showToast('上传失败', 'none')
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
			
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 130rpx;}
	.userHead{box-sizing: border-box;padding: 50rpx 4%;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;box-sizing: border-box;}
	.headNum .item{width: 50%;font-size: 26rpx;color: #eee;}
	.headNum .item .mny{font-size: 40rpx;line-height: 44rpx;margin-bottom: 14rpx;color: #fff;}
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
	.txBtn{width: 110rpx;height: 48rpx;line-height: 48rpx;text-align: center;border-radius: 36rpx;color: #fff;background-color: #F23132;box-shadow: 0 4rpx 0 #bd3f2a;font-size: 24rpx;}
</style>
