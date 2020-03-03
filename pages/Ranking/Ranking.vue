<template>
	<view>
		
		<view class="rankingNum">
			<view class="title pdb20 center fs15 flexCenter"><image class="icon" src="../../static/images/list-icon.png" mode=""></image>我的红包排行榜</view>
			<view class="flexRowBetween center fs12">
				<view class="item">
					<view class="num">5322</view>
					<view class="color9">累计总排名</view>
				</view>
				<view class="item">
					<view class="num">532+</view>
					<view class="color9">今日排名</view>
				</view>
				<view class="item">
					<view class="num pubColor">5</view>
					<view class="color9">获得红包</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		
		<view class="rankList fs13 mglr4">
			<view class="pdb10 center fs15 flexCenter"><image src="../../static/images/list-icon1.png" mode="" style="width: 36rpx;height: 30rpx;margin-right: 20rpx;"></image>累计奖励排行榜</view>
			<view class="item flexRowBetween" v-for="rankData in 6">
				<view class="ll flex">
					<view class="num" >{{rankData}}</view>
					<view class="fs13">156****2356</view>
				</view>
				<view class="rr flexEnd">
					<view class="flexColumn">
						<view class="pubColor">665</view>
						<view class="fs10 color6">获得红包</view>
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
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 130rpx;}
	.rankingNum{padding: 40rpx 0;}
	.title .icon{width: 27rpx;height: 36rpx;margin-right: 20rpx;}
	.rankingNum .item{width: 33.33%;}
	.rankingNum .item .num{font-size: 40rpx;line-height: 44rpx;margin-bottom: 10rpx;}
	
	.rankList{padding-top: 40rpx;}
	.rankList .item{padding: 20rpx 0;}
	.rankList .item .ll{width: 60%;}
	.rankList .item .rr{width: 40%;}
	.rankList .num{width:45rpx;height: 50rpx;margin-right: 20rpx;color: #999;font-size: 40rpx;text-align: center;}
	.rankList .item:nth-of-type(1) .num{background: url(../../static/images/list-icon2.png) no-repeat 0 0/100% 100%;color:transparent; }
	.rankList .item:nth-of-type(2) .num{background: url(../../static/images/list-icon3.png) no-repeat 0 0/100% 100%;color:transparent;}
	.rankList .item:nth-of-type(3) .num{background: url(../../static/images/list-icon4.png) no-repeat 0 0/100% 100%;color:transparent;}
	
</style>
