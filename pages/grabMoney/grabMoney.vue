<template>
	<view>
		
		<view class="ordinaryTit pr">
			<image class="bj" src="../../static/images/rob-img.png" mode=""></image>
		</view>
		
		<view class="grabCont">
			<view class="tit center fs17">恭喜您获得0.25元</view>
			<view class="moneyBj">
				<view class="mny center fs16">0.25</view>
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
	.ordinaryTit .bj{width: 100%;height: 140rpx;}
	.moneyBj{width: 520rpx;height: 504rpx;margin: 0 auto;background: url(../../static/images/rob-img1.png) no-repeat 0 0/100% 100%;position: relative;}
	.grabCont{position: fixed;right: 0;bottom: 25%;left: 0;}
	.grabCont .tit{padding-bottom: 60rpx;}
	.moneyBj .mny{position: absolute;bottom: 96rpx;left: 0;right: 0; color: #fcf38d;font-size:56rpx;line-height: 60rpx;}
	
</style>
