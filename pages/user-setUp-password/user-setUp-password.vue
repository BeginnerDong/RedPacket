<template>
	<view>
		
		<view class="pdlr4 paswd">
			<view class="item flexRowBetween">
				<view class="ll fs14">原密码</view>
				<view class="rr">
					<input type="password" v-model="submitData.o_password" placeholder="填写原密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll fs14">新密码</view>
				<view class="rr">
					<input type="password" v-model="submitData.n_password" placeholder="填写新密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll fs14">确认密码</view>
				<view class="rr">
					<input type="password" v-model="submitData.passwordCopy" placeholder="再次填写新密码" placeholder-class="placeholder">
				</view>
			</view>
		</view>
		
		
		
		<view class="submitbtn" style="margin-top: 360rpx;">
			<button class="btn" type="submint" @click="Utils.stopMultiClick(submit)">确定</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,
				index: 0,
				is_show: false,
				type: '',
				mode: '',
				submitData: {
					o_password: '',
					n_password: '',
					passwordCopy: ''
				}
			}
		},
		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
		},

		methods: {



			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);

				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					if (self.submitData.o_password != uni.getStorageSync('user_info').password) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('原密码错误', 'none');
						return
					};
					if (self.submitData.n_password != self.submitData.passwordCopy) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('两次输入密码不一致', 'none');
						return
					};
					self.passwordUpdate();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},

			passwordUpdate() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					password:self.submitData.n_password
				};
				const callback = (data) => {
					if (data.solely_code == 100000) {
						self.$Utils.showToast('修改成功,请重新登陆', 'none');
						setTimeout(function() {
							uni.removeStorageSync('user_token');
							uni.removeStorageSync('user_info');
							uni.reLaunch({
								url: '/pages/index/index'
							});
						}, 800);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
				};
				self.$apis.userUpdate(postData, callback);
			},

		},
	};
</script>


<style>
	/* .paswd .item{} */
	.paswd .item .ll{width: 20%;}
	.paswd .item .rr{width: 80%;border-bottom: 1px solid #eee;padding: 30rpx 0;}
	.placeholder{font-size: 26rpx;}
</style>
