<template>
	<view>
		
		<view class="mglr4 pdtb10" style="border-bottom: 1px solid #ddd;">
			<input type="text" v-model="submitData.address" placeholder="请填写您的地址" placeholder-class="placeholder">
		</view>
		
		<view class="submitbtn" style="margin-top: 500rpx;">
			<view class="btn" @click="Utils.stopMultiClick(userInfoUpdate)">确定</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					address:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.submitData.address = options.name
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			userInfoUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.address==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请填写地址', 'none', 1000)
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
	
	.placeholder{font-size: 26rpx;}
</style>
