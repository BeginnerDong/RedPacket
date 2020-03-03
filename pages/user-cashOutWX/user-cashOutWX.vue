<template>
	<view>
		<view class="mglr4 pdt15 color9 fs13 center">您选择微信转账进行红包提现</view>
		<view class="editLine">
			<view class="item flexRowBetween" >
				<view class="ll">您的微信号</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll">提现金额</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder">
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top:150rpx;">
			<view class="">
				<button class="btn" type="submint">确认</button>
			</view>
			
			<view class="w80 fs12 color9">您提交成功后，客服将会联系您进行转账</view>
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
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.logoIcon image{width: 70rpx;height: 70rpx;margin: 0 auto;}
	.seltIcon{width: 36rpx;height: 36rpx;margin: 0 auto;}
	.editLine .item{padding: 30rpx 3%;}
	.item .tit{font-size: 26rpx;margin: 30rpx 0 50rpx 0;}
	.editLine .item input{text-align: right;}
	
	.w80{width: 80%;margin: 20rpx auto;}
</style>
