<template>
	<view>
		
		<view class="flexRowBetween pdlr4 pdt15 pdb10 white">
			<view  @click="prev()"><image class="arrowR" style=" width: 18rpx;height: 32rpx;margin-left: 0;" src="../../static/images/arrowL.png"></image></view>
			<view class="fs12 flexEnd" @click="chooseImage">
				<view class="dian"></view>
				<view class="dian"></view>
				<view class="dian"></view>
			</view>
		</view>
		
		<view class="bigImg">
			<block v-if="imageSrc">
				<image :src="imageSrc" mode="widthFix"></image>
			</block>
			<block v-else>
				<view class="addfile">
					<image src="../../static/images/image-img.png" mode="widthFix"></image>
				</view>
			</block>
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
				imageSrc: ''
			}
		},
		onUnload() {
			const self = this;
			self.imageSrc = '';
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		onNavigationBarButtonTap(e){
			console.log(e);
			uni.navigateTo({
				url:'pages/aboutUs/aboutUs'
			})
		},
		methods: {
			
			prev(){
				const self = this;
				self.$router.go(-1)
			},
			chooseImage(){
				uni.chooseImage({
					count: 1,
					sizeType: ['compressed'],
					sourceType: ['album'],
					success: (res) => {
						console.log('chooseImage success, temp path is', res.tempFilePaths[0])
						var imageSrc = res.tempFilePaths[0]
						uni.uploadFile({
							url: 'https://unidemo.dcloud.net.cn/upload',
							filePath: imageSrc,
							fileType: 'image',
							name: 'data',
							success: (res) => {
								console.log('uploadImage success, res is:', res)
								uni.showToast({
									title: '上传成功',
									icon: 'success',
									duration: 1000
								})
								this.imageSrc = imageSrc
							},
							fail: (err) => {
								console.log('uploadImage fail', err);
								uni.showModal({
									content: err.errMsg,
									showCancel: false
								});
							}
						});
					},
					fail: (err) => {
						console.log('chooseImage fail', err)
					}
				})
			}
		}
	};
</script>

<style>
	
	page{background: #222;}
	.dian{width: 10rpx;height: 10rpx;border-radius: 50%;background: #FFF;margin-left: 10rpx;}
	.bigImg{width: 100%;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 22;}
	.bigImg .addfile{width: 100%;}
	.bigImg image{width: 100%;height: auto; display: block;}
</style>
