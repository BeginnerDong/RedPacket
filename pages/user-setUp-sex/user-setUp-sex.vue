<template>
	<view>
		
		<view class="whiteBj">
			<view class="item flexRowBetween" @click="changeCurr('1')" style="border-bottom: 1px solid #eee;">
				<view class="ll fs14">男</view>
				<view class="rr flexEnd">
					<image class="icon" :src="curr==1?'../../static/images/set-up-the-icon.png':''" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="changeCurr('2')">
				<view class="ll fs14">女</view>
				<view class="rr flexEnd">
					<image class="icon" :src="curr==2?'../../static/images/set-up-the-icon.png':''" mode=""></image>
				</view>
			</view>
		</view>
		
		
		
		<view class="submitbtn" style="margin-top: 360rpx;">
			<button class="btn" type="submint">确定</button>
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
	page{background-color: #F5F5F5;}
	.item{padding: 30rpx 4%;}
	.icon{width: 30rpx;height: 22rpx;}
</style>
