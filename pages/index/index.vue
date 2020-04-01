<template>
	<view>
		<!-- <view class="bigTit center white pubBj pdlr4">
			
		</view> -->
		
		


		<view class="">
			<view class="banner-box">
				<view class="banner pdb15">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000"
					 indicator-active-color="#ff5e43">
						<block v-for="(item,index) in sliderData" :key="index">
							<swiper-item class="swiper-item" @click="labelClick(item.behavior,'slider',item.id,item.url)">
								<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>

		<view class="indHome flex whiteBj">
			<view class="item" v-for="(item,index) in labelData" :key="index" @click="labelClick(item.behavior,'label',item.id,item.url,item.title)">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				<view class="tit">{{item.title}}</view>
			</view>
		</view>
		<view class="f5H10"></view>
		<view class="noticeLis flex">
			<view class="Licon mgr15">
				<image src="../../static/images/home-icon8.png" mode=""></image>
			</view>
			<view class="rr fs13">
				<swiper vertical="true" autoplay="true" circular="true" interval="3000">
					<swiper-item v-for="(item,index) in msg" :key="index">
						<view class="flex">
							<view class="tag">最新</view>
							<view class="avoidOverflow text">{{item}}</view>
						</view>
					</swiper-item>
				</swiper>
			</view>
		</view>
		<view class="f5H10"></view>

		<view class="pdlr4 proList">
			<view class="item flexRowBetween boxShaow" v-for="(item,index) in mainData" :key="index">
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
					<view class="robBtn" v-if="(!isLogin&&item.isStart)||(isLogin&&item.order&&item.order.length==0&&item.isStart)" @click="grab(index)">抢红包</view>
					<view class="robBtn" v-if="isLogin&&item.order&&item.order.length>0&&item.isStart">已参加</view>
					<view class="robBtn" v-if="!item.isStart" style="color: #666;background-color: #f5f5f5;box-shadow: 0 2px 0 #000;">未开启</view>
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



		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="tabClick('/pages/Ranking/Ranking')">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">红包排名</view>
			</view>
			<view class="navbar_item" @click="tabClick('/pages/user/user')">
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->

	</view>

</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,
				is_show: false,
				is_noOpenShow: false,

				msg: [
					
				],
				sliderData: [],
				isLogin: false,
				labelData: [],
				searchItem: {
					thirdapp_id: 2
				},
				mainData: [],
				flowLogData:[]
			}
		},

		onNavigationBarButtonTap() {
			console.log("点击了自定义按钮");
			const self = this;
			if (self.isLogin) {
				uni.redirectTo({
					url: '/pages/user/user'
				})
			} else {
				uni.navigateTo({
					url: '/pages/login/login'
				});
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getSliderData', 'getLabelData', 'getFlowGet'], self);
		},

		onShow() {
			const self = this;
			if (uni.getStorageSync('user_token')) {
				self.isLogin = true
			} else {
				self.isLogin = false
			}
			self.getMainData(true)
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
			
			getFlowGet() {
				const self = this;
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					user_type:0,
					withdraw:1,
					withdraw_status:1
				};
				/* postData.tokenFuncName = 'getUserToken';
				postData.getAfter = {
					userInfo:{
						tableName:'UserInfo',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['phone']
					}
				}; */
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.flowLogData.push.apply(self.flowLogData, res.info.data);
						for (var i = 0; i < self.flowLogData.length; i++) {
							self.flowLogData[i].count = -(parseFloat(self.flowLogData[i].count).toFixed(2))
							self.flowLogData[i].phone = self.flowLogData[i].phone.replace(/^(\d{3})\d{4}(\d+)/,"$1****$2");
							self.msg.push(self.flowLogData[i].phone+'成功提现'+self.flowLogData[i].count+'元整')
						}
					}
					self.$Utils.finishFunc('getFlowGet');
				};
				self.$apis.flowLogGet(postData, callback);
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
				var beginDateStr = ymd.replace(/\.|\-/g, '/')+' '+self.mainData[index].start_hour+':'+self.mainData[index].start_min;
				var endDateStr = ymd.replace(/\.|\-/g, '/')+' '+self.mainData[index].end_hour+':'+self.mainData[index].end_min;
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
						//self.$Utils.showToast(res.msg, 'none', 1000);
						self.Router.navigateTo({
							route: {
								path: '/pages/grabMoney/grabMoney?money=' + res.info.reward
							}
						})
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
				var curDate = new Date();
				var	beginDate = new Date(beginDateStr);
				var	endDate = new Date(endDateStr);
				if (curDate >= beginDate && curDate <= endDate) {
					return true;
				}
				return false;
			},

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
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					listorder: 'desc'
				};
				if(self.isLogin){
					postData.getAfter = {
						order:{
							token:uni.getStorageSync('user_token'),
							tableName:'FlowLog',
							middleKey:'product_no',
							key:'product_no',
							searchItem:{
								status:1,
								user_no:uni.getStorageSync('user_info').user_no
							},
							condition:'='
						}
					};
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
							var ymd = new Date().getFullYear() +  "-" +(new Date().getMonth() + 1).toString().padStart(2, "0") +  "-" + new Date().getDate().toString().padStart(2, "0")
							var beginDateStr = ymd.replace(/\.|\-/g, '/')+' '+self.mainData[i].start_hour+':'+self.mainData[i].start_min;
							var endDateStr = ymd.replace(/\.|\-/g, '/')+' '+self.mainData[i].end_hour+':'+self.mainData[i].end_min;
						
							self.mainData[i].isStart = self.isDuringDate(beginDateStr,endDateStr)
						
						}
					};
					console.log('self.mainData',self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},


			getLabelData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 3
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.labelData.push.apply(self.labelData, res.info.data)
					};
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},


			getSliderData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					child: {
						tableName: 'Label',
						middleKey: 'parentid',
						key: 'id',
						searchItem: {
							status: ['in', [1]],
							title: ['in', ['首页轮播']]
						},
						condition: 'in',
					}
				};
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.sliderData.push.apply(self.sliderData, res.info.data)
					};
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
			},

			labelClick(behavior, type, id, url,name) {
				const self = this;
				if (behavior == 1) {
					if (type == 'slider') {
						self.Router.navigateTo({
							route: {
								path: '/pages/detail/detail?id=' + id
							}
						})
					} else if (type == 'label') {
						self.Router.navigateTo({
							route: {
								path: '/pages/ordinary/ordinary?id=' + id+'&name='+name
							}
						})
					}
				} else if (behavior == 2) {
					plus.runtime.openURL(url);
				}
			},

			tabClick(path) {
				const self = this;
				if (self.isLogin) {
					self.Router.redirectTo({
						route: {
							path: path
						}
					})
				} else {
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
				}
			},

			noOpenshow() {
				const self = this;
				self.is_noOpenShow = !self.is_noOpenShow;
				self.is_show = !self.is_show;
			},

		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	
	page {
		padding-bottom: 140rpx;
	}
	.fixTitle{position: fixed;left: 0;right: 0;right: 0; z-index: 1;height: 89px;}
	.fixTitleH{height: 89px;}
	button{
		background: none;
		line-height: 1.5;
	}
	button::after{
		border: none;
	}
	.dian{width: 10rpx;height: 10rpx;border-radius: 50%;background: #FFF;margin-left: 10rpx;}
	.button-hover{
		color: #000000;
		background: none;
	}
	.swiper-box {
		height: 360rpx;
		box-sizing: border-box;
		overflow: hidden;
	}

	.swiper-box swiper-item {
		width: 100%;
	}

	.swiper-box image {
		width: 100%;
		height: 100%;
	}

	/* 分类导航 */
	.indHome {
		flex-wrap: wrap;
	}

	.indHome .item {
		width: 25%;
		text-align: center;
		padding-bottom: 36rpx;
		font-size: 26rpx;
		color: #222;
		display: flex;
		flex-direction: column;
	}

	.indHome .item image {
		width: 90rpx;
		height: 90rpx;
		margin: 0 auto 16rpx auto;
	}

	/* 公告滚动 */
	.noticeLis {
		overflow: hidden;
		padding: 30rpx 4%;
	}

	.noticeLis .Licon {
		width: 50rpx;
		height: 50rpx;
	}

	.noticeLis .Licon image {
		width: 100%;
		height: 100%;
		display: block;
	}

	.noticeLis .rr {
		width: 88%;
		height: 60rpx;
		line-height: 60rpx;
	}

	.noticeLis .rr swiper {
		height: 60rpx;
	}

	.noticeLis .rr .tag {
		height: 36rpx;
		line-height: 36rpx;
		border-radius: 6rpx;
		border: 1px solid #f23132;
		padding: 0 10rpx;
		font-size: 20rpx;
		color: #f23132;
		margin-right: 20rpx;
	}

	.noticeLis .rr .text {
		width: 75%;
		font-size: 24rpx;
	}
</style>
