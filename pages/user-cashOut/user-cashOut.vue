<template>
	<view>
		
		<view class="mglr4 center flexRowAround cont">
			<view class="item" @click="changeCurr('1')" v-show="thirdAppData.wx==1">
				<view class="logoIcon">
					<image src="../../static/images/withdrawal-icon.png" mode=""></image>
				</view>
				<view class="tit">微信转账</view>
				<view class="">
					<image class="seltIcon"  :src="curr==1?'../../static/images/withdrawal-icon3.png':'../../static/images/withdrawal-icon4.png'"  mode=""></image>
				</view>
			</view>
			<view class="item" @click="changeCurr('2')" v-show="thirdAppData.zfb==1">
				<view class="logoIcon">
					<image src="../../static/images/withdrawal-icon1.png" mode=""></image>
				</view>
				<view class="tit">支付宝转账</view>
				<view class="">
					<image class="seltIcon" :src="curr==2?'../../static/images/withdrawal-icon3.png':'../../static/images/withdrawal-icon4.png'" mode=""></image>
				</view>
			</view>
			<view class="item" @click="changeCurr('3')" v-show="thirdAppData.hf==1">
				<view class="logoIcon">
					<image src="../../static/images/withdrawal-icon2.png" mode=""></image>
				</view>
				<view class="tit">话费充值</view>
				<view class="">
					<image class="seltIcon" :src="curr==3?'../../static/images/withdrawal-icon3.png':'../../static/images/withdrawal-icon4.png'" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top:150rpx;">
			<button class="btn" type="submint" @click="toDetail">提交表单</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				curr:1,
				thirdAppData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getThirdAppData'], self);
		},
		
		methods: {
			
			toDetail(){
				const self = this;
				if(self.curr==1){
					self.Router.navigateTo({route:{path:'/pages/user-cashOutWX/user-cashOutWX'}})
				}else if(self.curr==2){
					self.Router.navigateTo({route:{path:'/pages/user-cashOut-Alipay/user-cashOut-Alipay'}})
				}else if(self.curr==3){
					self.Router.navigateTo({route:{path:'/pages/user-cashOut-PhoneBill/user-cashOut-PhoneBill'}})
				}
			},
			
			getThirdAppData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					id:2
				}
				const callback = (res) => {
					if (res.solely_code==100000) {
						self.thirdAppData = res.info.data[0];
						/* if(self.thirdAppData.zfb==1){
							self.zfbShow = true
						};
						if(self.thirdAppData.wx==1){
							self.wxShow = true
						};
						if(self.thirdAppData.hf==1){
							self.hfShow = true
						}; */
					};
					self.$Utils.finishFunc('getThirdAppData');
				};
				self.$apis.thirdAppGet(postData, callback);
			},
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	page{background-color: #F5F5F5;}
	.logoIcon image{width: 70rpx;height: 70rpx;margin: 0 auto;}
	.seltIcon{width: 36rpx;height: 36rpx;margin: 0 auto;}
	.cont{flex-wrap: wrap;}
	.item{width: 300rpx;height: 310rpx;background-color: #fff;padding: 40rpx 5%;box-sizing: border-box;border-radius: 10rpx;margin-top: 50rpx;}
	.item .tit{font-size: 26rpx;margin: 30rpx 0 50rpx 0;}
</style>
