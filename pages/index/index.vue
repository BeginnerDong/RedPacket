<template>
	<view>
		<!-- <view class="bigTit center white pubBj pdlr4">
			
		</view> -->
		
		<view class="">
			<view class="banner-box">
				<view class="banner pdb15">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#ff5e43">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item"  @click="Router.navigateTo({route:{path:'/pages/detail/detail'}})">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="indHome flexRowBetween whiteBj">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/ordinary/ordinary'}})">
				<image src="../../static/images/home-icon.png"></image>
				<view class="tit">普通红包</view>
			</view>
			<view class="item">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">幸运红包</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages//'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">菜单红包</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages//'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">惊喜红包</view>
			</view>
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages//'}})">
				<image src="../../static/images/home-icon.png"></image>
				<view class="tit">普通红包</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages//'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">幸运红包</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages//'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">菜单红包</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages//'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">惊喜红包</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="noticeLis flex">
			<view class="Licon mgr15">
				<image src="../../static/images/home-icon8.png" mode=""></image>
			</view>
			<view class="rr fs13">
				<swiper vertical="true" autoplay="true" circular="true" interval="3000">
					<swiper-item v-for="(item,index) in msg" :key="index">
						<view class="flex">
							<view class="tag">最新</view>
							<view class="avoidOverflow text">{{item}}</view>
						</view>
					</swiper-item>
				</swiper>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="pdlr4 proList">
			<view class="item flexRowBetween boxShaow">
				<view class="ll flex">
					<view class="icon">
						<image src="../../static/images/home-icon9.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="tit">普通红包</view>
						<view class="data">10:00~23:20</view>
					</view>
				</view>
				<view class="rr flexEnd">
					<view class="robBtn"  @click="noOpenshow">抢红包</view>
				</view>
			</view>
			<view class="item flexRowBetween boxShaow">
				<view class="ll flex">
					<view class="icon">
						<image src="../../static/images/home-icon9.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="tit">普通红包</view>
						<view class="data">10:00~23:20</view>
					</view>
				</view>
				<view class="rr flexEnd">
					<view class="robBtn" @click="goLoginShow">抢红包</view>
				</view>
			</view>
			<view class="item flexRowBetween boxShaow">
				<view class="ll flex">
					<view class="icon">
						<image src="../../static/images/home-icon9.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="tit">普通红包</view>
						<view class="data">10:00~23:20</view>
					</view>
				</view>
				<view class="rr flexEnd">
					<view class="robBtn">抢红包</view>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="robAlert" v-show="is_noOpenShow">
			<view class="closebtn" @click="noOpenshow">×</view>
			<view class="noOpen">
				<view class="fs14 center">此红包在这段时间内还未开启，您可以关注其他的红包</view>
				<view class="pdt30">
					<image class="icon" src="../../static/images/home-icon10.png" mode=""></image>
				</view>
			</view>
		</view>
		<view class="robAlert" v-show="is_goLoginShow">
			<view class="closebtn" @click="goLoginShow">×</view>
			<view class="noOpen">
				<view class="fs14 center">您还没有登录，暂时不能开启，您可以登录，享受功能</view>
				<view class="pdt30 mgt20 flexRowBetween goLogin">
					<view class="btn" @click="goLoginShow">否</view>
					<view class="btn on" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">去登录</view>
				</view>
			</view>
		</view>
		
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/Ranking/Ranking'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">红包排名</view>
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
				is_show: false,
				wx_info:{},
				is_show:false,
				is_noOpenShow:false,
				labelData: [
					"../../static/images/banner.png",
					"../../static/images/banner.png"
				],
				msg:[
					'178****2356成功提现50.0元整',
					'158****2388成功提现80.0元整',
					'158****2388成功提现60.0元整'
				]
			}
		},
		onNavigationBarButtonTap() {  
			console.log("点击了自定义按钮");  
			uni.navigateTo({  
				url: '/pages/login/login'  
			}); 
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			noOpenshow(){
				const self = this;
				self.is_noOpenShow = !self.is_noOpenShow
				self.is_show = !self.is_show
			},
			goLoginShow(){
				const self = this;
				self.is_goLoginShow = !self.is_goLoginShow
				self.is_show = !self.is_show
			},
			// closeNoOpen(){
			// 	const self = this;
			// 	self.is_noOpenShow = !self.is_noOpenShow
			// 	self.is_show = false
			// },
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
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 140rpx;}	
	
	.swiper-box {height: 360rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item{width: 100%;}
	.swiper-box image {width: 100%;height: 100%;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 25%;text-align: center;padding-bottom: 36rpx; font-size: 26rpx; color: #222;display: flex; flex-direction: column;}
	.indHome .item image{width:90rpx;height:90rpx; margin: 0 auto 16rpx auto; }
	
	/* 公告滚动 */
	.noticeLis{overflow: hidden;padding: 30rpx 4%;}
	.noticeLis .Licon{width: 50rpx;height: 50rpx;}
	.noticeLis .Licon image{width: 100%;height: 100%;display: block;}
	.noticeLis .rr{width: 88%;height: 60rpx;line-height: 60rpx;}
	.noticeLis .rr swiper{height:60rpx;}
	.noticeLis .rr .tag{height: 36rpx;line-height: 36rpx;border-radius: 6rpx;border:1px solid #f23132;padding: 0 10rpx;font-size: 20rpx;color: #f23132;margin-right: 20rpx;}
	.noticeLis .rr .text{width: 75%;font-size: 24rpx;}
	
	
</style>
