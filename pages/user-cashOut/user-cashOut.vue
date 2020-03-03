<template>
	<view>
		
		<view class="mglr4 center flexRowAround cont">
			<view class="item" @click="changeCurr('1')">
				<view class="logoIcon">
					<image src="../../static/images/withdrawal-icon.png" mode=""></image>
				</view>
				<view class="tit">微信转账</view>
				<view class="">
					<image class="seltIcon"  :src="curr==1?'../../static/images/withdrawal-icon3.png':'../../static/images/withdrawal-icon4.png'"  mode=""></image>
				</view>
			</view>
			<view class="item" @click="changeCurr('2')">
				<view class="logoIcon">
					<image src="../../static/images/withdrawal-icon1.png" mode=""></image>
				</view>
				<view class="tit">支付宝转账</view>
				<view class="">
					<image class="seltIcon" :src="curr==2?'../../static/images/withdrawal-icon3.png':'../../static/images/withdrawal-icon4.png'" mode=""></image>
				</view>
			</view>
			<view class="item" @click="changeCurr('3')">
				<view class="logoIcon">
					<image src="../../static/images/withdrawal-icon2.png" mode=""></image>
				</view>
				<view class="tit">话费充值</view>
				<view class="">
					<image class="seltIcon" :src="curr==3?'../../static/images/withdrawal-icon3.png':'../../static/images/withdrawal-icon4.png'" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top:150rpx;">
			<button class="btn" type="submint" @click="Router.navigateTo({route:{path:'/pages/user-cashOutWX/user-cashOutWX'}})">提交表单</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	page{background-color: #F5F5F5;}
	.logoIcon image{width: 70rpx;height: 70rpx;margin: 0 auto;}
	.seltIcon{width: 36rpx;height: 36rpx;margin: 0 auto;}
	.cont{flex-wrap: wrap;}
	.item{width: 300rpx;height: 310rpx;background-color: #fff;padding: 40rpx 5%;box-sizing: border-box;border-radius: 10rpx;margin-top: 50rpx;}
	.item .tit{font-size: 26rpx;margin: 30rpx 0 50rpx 0;}
</style>
