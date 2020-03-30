<template>
	<view>
		<view class="white pdlr4">
			<!-- <view class="center fs15 pdt15 pdb10">斯科拉和复健科多少公交卡反倒</view> -->
			<view class="xqInfor">
				<view class="cont fs13">
					<view class="content ql-editor" style="padding:0;"
					v-html="mainData.content">
					</view>
				</view>
			</view>
			
			<view class="R-fixIcon" @click="copy()"><image src="../../static/images/share-icon.png" mode=""></image></view>
			
			
			
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['分享']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			copy(){
				const self = this;
				uni.setClipboardData({
				    data: self.mainData.passage1,
				    success: function () {
				        self.$Utils.showToast('分享链接已复制', 'none')
				    }
				});
			},
		}
	};
</script>

<style>
	
	page{background: #f23132;padding-bottom:40rpx;}
	
	.xqInfor .cont view{padding-bottom: 16rpx;line-height: 46rpx;}
	.ewm{width: 200rpx;height: 200rpx;border-radius: 10rpx;overflow: hidden;}
	.ewm image{width: 100%;height: 100%;}
	
	.R-fixIcon{width: 100rpx;height: 100rpx;position: fixed;bottom:45%;right: 0rpx;z-index: 66;}
	.R-fixIcon image{width: 100%;height: 100%;}
</style>
