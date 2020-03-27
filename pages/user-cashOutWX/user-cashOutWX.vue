<template>
	<view>
		<view class="mglr4 pdt15 color9 fs13 center">您选择微信转账进行红包提现</view>
		<view class="editLine">
			<view class="item flexRowBetween" >
				<view class="ll">您的姓名</view>
				<view class="rr">
					<input type="text" v-model="submitData.name" placeholder="请输入您的真实姓名" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll">您的微信号</view>
				<view class="rr">
					<input type="text" v-model="submitData.trade_info" placeholder="请输入" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll">提现金额</view>
				<view class="rr">
					<input type="text" v-model="submitData.count" placeholder="请输入" placeholder-class="placeholder">
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top:150rpx;">
			<view class="">
				<button class="btn" type="submint" @click="Utils.stopMultiClick(submit)">确认</button>
			</view>
			
			<view class="w80 fs12 color9">您提交成功后，客服将会联系您进行转账</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				userInfoData:{},
				submitData:{
					count:'',
					trade_info:'',
					name:''
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		
		methods: {
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.submitData',self.submitData)
				if (pass) {
					if(parseFloat(self.submitData.count)>parseFloat(self.userInfoData.balance)){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('余额不足', 'none');
						return
					};
					if(parseFloat(self.submitData.count)<=0){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的金额', 'none');
						return
					};
						self.flowLogAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入微信账号或提现金额', 'none')
				};
			},
			
			flowLogAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					count:-self.submitData.count,
					thirdapp_id:2,
					status:1,
					trade_info:self.submitData.trade_info,
					type:2,
					account:1,
					withdraw:1,
					withdraw_status:0,
					phone:uni.getStorageSync('user_info').login_name,
					behavior:2,
					name:self.submitData.name
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功，等待后台审核', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1500)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.logoIcon image{width: 70rpx;height: 70rpx;margin: 0 auto;}
	.seltIcon{width: 36rpx;height: 36rpx;margin: 0 auto;}
	.editLine .item{padding: 30rpx 3%;}
	.item .tit{font-size: 26rpx;margin: 30rpx 0 50rpx 0;}
	.editLine .item input{text-align: right;}
	
	.w80{width: 80%;margin: 20rpx auto;}
</style>
