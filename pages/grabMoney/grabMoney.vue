<template>
	<view>
		
		<view class="ordinaryTit pr">
			<image class="bj" src="../../static/images/rob-img.png" mode=""></image>
		</view>
		
		<view class="grabCont"  style="margin-top: 80rpx;">
			<view class="tit center fs17">恭喜您获得{{money}}元</view>
			<view class="moneyBj">
				<view class="pic">
					<image src="../../static/images/rob-img1.png" mode=""></image>
				</view>
				<view class="mny center fs16">{{money}}</view>
			</view>
		</view>
		
		<view class="item loginbtn center flexCenter" style="margin: 80rpx auto;width: 80%;">
			<button class="btn" type="submint" style="border: none;width: 80%;" @click="Router.back()">返回</button>
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
				money:0
			}
		},
		onLoad(options) {
			const self = this;
			self.money = options.money/100;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 60rpx;}	
	.ordinaryTit .bj{width: 100%;height: 140rpx;}
	.moneyBj{width: 520rpx;height: 504rpx;margin: 0 auto;position: relative;}
	.grabCont{right: 0;bottom: 25%;left: 0;}
	.grabCont .tit{padding-bottom: 60rpx;}
	.moneyBj .mny{position: absolute;bottom: 96rpx;left: 0;right: 0; color: #fcf38d;font-size:56rpx;line-height: 60rpx;z-index: 2;}
	
	.moneyBj .pic,.moneyBj .pic image{width: 100%;height: 100%;}
	.loginbtn{border:1px solid #F23132;overflow: hidden;background-color: #F23132;}
	.loginbtn .btn{background-color:transparent;color: #fff;border: none;width: 80%;margin: 0 auto;display: block;font-size: 26rpx;height: 100%;line-height: 78rpx;}
	
</style>
