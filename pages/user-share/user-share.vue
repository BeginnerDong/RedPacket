<template>
	<view>
		<view class="white pdlr4">
			<view class="center fs15 pdt15 pdb10">斯科拉和复健科多少公交卡反倒</view>
			<view class="xqInfor">
				<view class="cont fs13">
					<view>个人个话复健科供货方可绝对是个会奉公守法和国防科技到时候更烦上开个割发代首是割发代首跟客户巨孚达商量价格给家里福利都给突然感觉人</view>
					<view>飞沙关很过分落地生根色温而沃尔夫王鹏放我我杰尔夫集隔热法拉丰富了多少积分供货方点击可四氯化硅发个割发代个发会员特惠聚一块</view>
					<view>
						<image style="width: 100%;height: 400rpx;" src="../../static/images/share-img2.png" mode="widthFix"></image>
					</view>
				</view>
			</view>
			
			<view class="R-fixIcon"><image src="../../static/images/share-icon.png" mode=""></image></view>
			
			<view class="flexRowBetween pdt15" style="width: 85%;margin: 0 auto;">
				<view class="flexColumn">
					<view class="pdb10">苹果安装二维码</view>
					<view class="ewm"><image src="../../static/images/share-img1.png" mode=""></image></view>
				</view>
				<view class="flexColumn">
					<view class="pdb10">安卓安装二维码</view>
					<view class="ewm"><image src="../../static/images/share-img1.png" mode=""></image></view>
				</view>
			</view>
			
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
							title: ['in', ['关于我们']],
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
