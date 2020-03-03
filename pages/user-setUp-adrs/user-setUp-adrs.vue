<template>
	<view>
		
		<view class="mglr4 pdtb10" style="border-bottom: 1px solid #ddd;">
			<input type="text" value="" placeholder="请输入" placeholder-class="placeholder">
		</view>
		
		<view class="submitbtn" style="margin-top: 500rpx;">
			<view class="btn">确定</view>
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
	
	.placeholder{font-size: 26rpx;}
</style>
