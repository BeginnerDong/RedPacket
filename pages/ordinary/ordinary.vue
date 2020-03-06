<template>
	<view>
		
		<view class="ordinaryTit pr">
			<image class="bj" src="../../static/images/rob-img2.png" mode=""></image>
			<view class="text white center pdlr4">- 无套路现金红包整点开枪 -</view>
		</view>
		
		<view class="pdlr4 proList pdt10">
			<view class="item flexRowBetween boxShaow" v-for="(item,index) in mainData">
				<view class="ll flex">
					<view class="icon">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="tit">{{item.title}}</view>
						<view class="data">{{item.start_hour}}:{{item.start_min}}~{{item.end_hour}}:{{item.end_min}}</view>
					</view>
				</view>
				<view class="rr flexEnd">
					<view class="robBtn"  @click="grab(index)">抢红包</view>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="robAlert" v-show="is_noOpenShow">
			<view class="closebtn" @click="noOpenshow">×</view>
			<view class="noOpen">
				<view class="fs14 center">此红包在这段时间内还未开启，您可以关注其他的红包</view>
				<view class="pdt30">
					<image class="icon" src="../../static/images/home-icon10.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- <view class="robAlert" v-show="is_goLoginShow">
			<view class="closebtn" @click="goLoginShow">×</view>
			<view class="noOpen">
				<view class="fs14 center">您还没有登录，暂时不能开启，您可以登录，享受功能</view>
				<view class="pdt30 mgt20 flexRowBetween goLogin">
					<view class="btn" @click="goLoginShow">否</view>
					<view class="btn on" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">去登录</view>
				</view>
			</view>
		</view> -->
		
		
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{},
				is_noOpenShow:false,
				mainData:[]
			}
		},
		onLoad(options) {
			const self = this;
			self.id  = options.id;
			uni.setNavigationBarTitle({
			    title: options.name
			});
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			if (uni.getStorageSync('user_token')) {
				self.isLogin = true
			} else {
				self.isLogin = false
			}
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2,
					category_id:self.id
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data)
						for (var i = 0; i < self.mainData.length; i++) {
							if (self.mainData[i].start_hour == 0) {
								self.mainData[i].start_hour = '00'
							};
							if (self.mainData[i].start_min == 0) {
								self.mainData[i].start_min = '00'
							};
							if (self.mainData[i].end_hour == 0) {
								self.mainData[i].end_hour = '00'
							};
							if (self.mainData[i].end_min == 0) {
								self.mainData[i].end_min = '00'
							};
						}
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			noOpenshow(){
				const self = this;
				self.is_noOpenShow = !self.is_noOpenShow
				self.is_show = !self.is_show
			},
			
			grab(index) {
				var self = this;
				uni.setStorageSync('canClick', false);
				if (!self.isLogin) {
					uni.showModal({
						title: '提示',
						content: '您还没有登录，暂时不能开启，您可以登录享受功能,是否立即登录？',
						confirmColor: "#f23132",
						success: function(res) {
							if (res.confirm) {
								uni.navigateTo({
									url: '/pages/login/login'
								});
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
					return
				};
				var ymd = new Date().getFullYear() +  "-" +(new Date().getMonth() + 1).toString().padStart(2, "0") +  "-" + new Date().getDate().toString().padStart(2, "0")
				var beginDateStr = ymd+' '+self.mainData[index].start_hour+':'+self.mainData[index].start_min;
				var endDateStr = ymd+' '+self.mainData[index].end_hour+':'+self.mainData[index].end_min;
				if(!self.isDuringDate(beginDateStr,endDateStr)){
					self.noOpenshow();
					return
				};
				var postData = {};
				postData.tokenFuncName = 'getUserToken'
				postData.data = {
					id: self.mainData[index].id
				}
				var callback = function(res) {
					if(res.solely_code==100000){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none', 1000)
					}else{
						//uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg, 'none', 1000)
					}
				};
				self.$apis.grab(postData, callback);
			},
			
			isDuringDate(beginDateStr, endDateStr) {
				console.log('beginDateStr',beginDateStr)
				console.log('endDateStr',endDateStr)
				var curDate = new Date(),
					beginDate = new Date(beginDateStr),
					endDate = new Date(endDateStr);
				if (curDate >= beginDate && curDate <= endDate) {
					return true;
				}
				return false;
			},
		}
	};
</script>

<style>
	@import "../../assets/style/proList.css";
	
	page{padding-bottom: 60rpx;}	
	.ordinaryTit .bj{width: 100%;height: 140rpx;}
	.ordinaryTit .text{position: absolute;top: 0;right: 0;bottom: 0;left: 0;padding-top: 20rpx;}
</style>
