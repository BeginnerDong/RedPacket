<template>
	<view>
		
		<view class="rankingNum">
			<view class="title pdb20 center fs15 flexCenter"><image class="icon" src="../../static/images/list-icon.png" mode=""></image>我的红包排行榜</view>
			<view class="flexRowBetween center fs12">
				<view class="item">
					<view class="num">{{rankData.total_rank}}</view>
					<view class="color9">累计总排名</view>
				</view>
				<view class="item">
					<view class="num">{{rankData.today_rank}}</view>
					<view class="color9">今日排名</view>
				</view>
				<view class="item">
					<view class="num pubColor">{{rankData.today_count}}</view>
					<view class="color9">获得红包</view>
				</view>
			</view>
		</view>
		
		<view class="rankingNumH"></view>
		<view class="f5H10"></view>
		
		
		<view class="rankList fs13 mglr4">
			<view class="pdb10 center fs15 flexCenter">
				<image src="../../static/images/list-icon1.png" mode="" style="width: 36rpx;height: 30rpx;margin-right: 20rpx;">
					
				</image>累计奖励排行榜</view>
			<view class="realList">
				<view class="item flexRowBetween" v-for="(item,index) in mainData">
					<view class="ll flex">
						<view class="num" >{{index+1}}</view>
						<view class="fs13">{{item.userInfo&&item.userInfo.phone?item.userInfo.phone:''}}</view>
					</view>
					<view class="rr flexEnd">
						<view class="flexColumn">
							<view class="pubColor">{{item.count}}</view>
							<view class="fs10 color6">获得红包</view>
						</view>
					</view>
				</view>
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
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">红包排名</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
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
				rankData:{},
				mainData:[]
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getRankData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getRankData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					if (res.solely_code==100000) {
						self.rankData = res.info;
					};
					self.$Utils.finishFunc('getRankData');
				};
				self.$apis.getRank(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:1,
					user_type:0
				};
				postData.tokenFuncName = 'getUserToken';
				postData.order = {
					count:'desc'
				};
				postData.getAfter = {
					userInfo:{
						tableName:'UserInfo',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['phone']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							
							self.mainData[i].userInfo.phone = self.mainData[i].userInfo.phone.replace(/^(\d{3})\d{4}(\d+)/,"$1****$2")
						}
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.rankGet(postData, callback);
			},
			
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 130rpx;}
	.rankingNum{padding: 40rpx 0;box-sizing: border-box;height: 274rpx;position: fixed;left: 0;top:88rpx;right: 0; z-index: 2;background: #fff;border-bottom: 20rpx solid #F5F5F5;}
	.rankingNumH{width: 100%;height: 254RPX;}
	.title .icon{width: 27rpx;height: 36rpx;margin-right: 20rpx;}
	.rankingNum .item{width: 33.33%;}
	.rankingNum .item .num{font-size: 40rpx;line-height: 44rpx;margin-bottom: 10rpx;}
	
	.rankList{padding-top: 40rpx;}
	.rankList .item{padding: 20rpx 0;}
	.rankList .item .ll{width: 60%;}
	.rankList .item .rr{width: 40%;}
	.rankList .num{width:45rpx;height: 50rpx;margin-right: 20rpx;color: #999;font-size: 40rpx;text-align: center;}
	.realList .item:nth-of-type(1) .num{background: url(../../static/images/list-icon2.png) no-repeat 0 0/100% 100%;color:transparent; }
	.realList .item:nth-of-type(2) .num{background: url(../../static/images/list-icon3.png) no-repeat 0 0/100% 100%;color:transparent;}
	.realList .item:nth-of-type(3) .num{background: url(../../static/images/list-icon4.png) no-repeat 0 0/100% 100%;color:transparent;}
	
</style>
