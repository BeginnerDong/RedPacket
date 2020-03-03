<template>
	<view>
		
		<view class="pdlr4 paswd">
			<view class="item flexRowBetween">
				<view class="ll fs14">原密码</view>
				<view class="rr">
					<input type="password" value="" placeholder="填写原密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll fs14">新密码</view>
				<view class="rr">
					<input type="password" value="" placeholder="填写新密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll fs14">确认密码</view>
				<view class="rr">
					<input type="password" value="" placeholder="再次填写新密码" placeholder-class="placeholder">
				</view>
			</view>
		</view>
		
		
		
		<view class="submitbtn" style="margin-top: 360rpx;">
			<button class="btn" type="submint" @click="Router.navigateTo({route:{path:'/pages/aboutUs/aboutUs'}})">确定</button>
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
	/* .paswd .item{} */
	.paswd .item .ll{width: 20%;}
	.paswd .item .rr{width: 80%;border-bottom: 1px solid #eee;padding: 30rpx 0;}
	.placeholder{font-size: 26rpx;}
</style>
