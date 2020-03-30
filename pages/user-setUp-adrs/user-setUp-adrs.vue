<template>
	<view>

		<view class="mglr4 pdtb10 flexRowBetween" style="border-bottom: 1px solid #ddd;" @tap="openAddres">
			<view>地区</view>
			<input type="text"  style="text-align: right;"  v-model="submitData.address" disabled="true" placeholder="请选择您的地区" placeholder-class="placeholder">
		</view>
		<view class="mglr4 pdtb10 flexRowBetween" style="border-bottom: 1px solid #ddd;">
			<view>详细地址</view>
			<input type="text" style="text-align: right;" v-model="submitData.passage1"  placeholder="请填写" placeholder-class="placeholder">
		</view>
		<view class="submitbtn" style="margin-top: 500rpx;">
			<view class="btn" @click="Utils.stopMultiClick(userInfoUpdate)">确定</view>
		</view>
		<simple-address ref="simpleAddress" :pickerValueDefault="cityPickerValueDefault" @onConfirm="onConfirm" themeColor="#007AFF">

		</simple-address>
	</view>
</template>

<script>
	import simpleAddress from '@/components/simple-address/simple-address.nvue';
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,
				submitData: {
					address: '',
					passage1:''
				},
				cityPickerValueDefault: [0, 0, 1],
				pickerText: ''
			}
		},
		components: {
			simpleAddress
		},
		onLoad(options) {
			const self = this;
			self.submitData.address = options.name;
			self.submitData.passage1 = options.passage1;
			if (self.submitData.address == '') {
				self.submitData.address = '北京市-市辖区-东城区'
			}
			//self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {

			onConfirm(e) {
				const self = this;
				self.submitData.address = e.label;
				//this.pickerText = JSON.stringify(e);
			},

			openAddres() {
				const self = this;

				var array = self.submitData.address.split('-');
				var index = this.$refs.simpleAddress.queryIndex([array[0], array[1], array[2]], 'label');
				console.log(index);
				this.cityPickerValueDefault = index.index;
				this.$refs.simpleAddress.open();
			},

			userInfoUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.submitData.address == '') {
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
							delta: 1
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
	.placeholder {
		font-size: 26rpx;
	}
</style>
