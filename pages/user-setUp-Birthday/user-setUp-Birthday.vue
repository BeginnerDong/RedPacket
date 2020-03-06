<template>
	<view>
		
		<view class="mglr4 pdtb10" style="border-bottom: 1px solid #ddd;">
			<picker mode="date" @change="change">
				{{submitData.birthday!=''?submitData.birthday:'请选择您的生日'}}
			</picker>
		</view>
		
		<view class="submitbtn" style="margin-top: 500rpx;">
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
				submitData:{
					birthday:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.submitData.birthday = options.name
			console.log('22',self.submitData.gender)
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			change(e){
				const self = this;
				console.log(e)
				self.submitData.birthday = e.detail.value
			},
			
			userInfoUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.birthday==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择生日', 'none', 1000)
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
