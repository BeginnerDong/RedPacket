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
			<button class="btn" type="submint" @click="Utils.stopMultiClick(userInfoUpdate)">确定</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				curr:1,
				submitData:{
					gender:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.curr = options.name;
			self.submitData.gender = options.name
			console.log('22',self.submitData.gender)
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr;
					self.submitData.gender = self.curr
				}
			},
			
			userInfoUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.gender==0){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择性别', 'none', 1000)
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('userInfo').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						uni.navigateBack({
							delta:1
						})
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
		}
	};
</script>

<style>
	page{background-color: #F5F5F5;}
	.item{padding: 30rpx 4%;}
	.icon{width: 30rpx;height: 22rpx;}
</style>
