<template>
	<view>
		
		<view class="ordinaryTit pr">
			<image class="bj" src="../../static/images/rob-img2.png" mode=""></image>
			<view class="text white center pdlr4">- 无套路现金红包整点开枪 -</view>
		</view>
		
		<view class="pdlr4 proList pdt10">
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
					<view class="robBtn" @click="Router.navigateTo({route:{path:'/pages/grabMoney/grabMoney'}})">抢红包</view>
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
				is_noOpenShow:false
			}
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
	.ordinaryTit .bj{width: 100%;height: 140rpx;}
	.ordinaryTit .text{position: absolute;top: 0;right: 0;bottom: 0;left: 0;padding-top: 20rpx;}
</style>
