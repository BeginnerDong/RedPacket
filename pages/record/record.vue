<template>
	<view>
		
		<view class="myRowBetween mglr4">
			<view class="item flexRowBetween" v-for="item in mainData" :key="index">
				<view class="ll">
					<view class="fs14">{{item.trade_info}}</view>
					<view class="fs12 color6">{{item.create_time}}</view>
				</view>
				<view class="rr flexEnd pubColor">{{item.count}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[],
				searchItem:{
					type:2,
					thirdapp_id:2,
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
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
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myRowBetween .item .rr{font-size: 30rpx;}
</style>
