<template>
	<view>
		
		<view class="myRowBetween mglr4">
			<view class="item flexRowBetween" v-for="item in rankData">
				<view class="ll">
					<view class="fs14">普通红包</view>
					<view class="fs12 color6">2018.02.23</view>
				</view>
				<view class="rr flexEnd pubColor">+0.26</view>
			</view>
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
				rankData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.myRowBetween .item .rr{font-size: 30rpx;}
</style>
