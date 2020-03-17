<template>
	<view>
		
		<view class="userHead pdlr4 pubBj white">
			<view class="infor flex pdb20">
				<image class="photo" :src="userInfoData.mainImg&&userInfoData.mainImg[0]?userInfoData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
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
				userInfoData:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {

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
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
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
