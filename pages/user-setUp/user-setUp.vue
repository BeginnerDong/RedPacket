<template>
	<view>
		
		<view class="myRowBetween pdlr4 whiteBj">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-uploadPhoto/user-uploadPhoto'}})" >
				<view class="ll">图像</view>
				<view class="rr flexEnd">
					<view class="userPhoto"><image src="../../static/images/set-up-the-img.png" mode=""></image></view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-nickname/user-setUp-nickname'}})" >
				<view class="ll">手机号</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">15689232560</view>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-sex/user-setUp-sex'}})" >
				<view class="ll">性别</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">请选择</view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-Birthday/user-setUp-Birthday'}})" >
				<view class="ll">生日</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">请选择</view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-adrs/user-setUp-adrs'}})" >
				<view class="ll">地区</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">请填写</view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-password/user-setUp-password'}})" >
				<view class="ll">登录密码</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">13352323dfdff</view>
					<image class="arrowR" src="../../static/images/about-icon3.png" mode=""></image></view>
			</view>
		</view>
		
		<view class="quitBtn boxShaow center fs15">退出登录</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{}
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
	page{padding-bottom: 60rpx;}
	.userPhoto{width: 80rpx;height: 80rpx;border-radius: 50%;border: 1px solid #eee;box-sizing: border-box;overflow: hidden;}
	.userPhoto image{width: 100%;height: 100%; display: block;}
	.quitBtn{width: 80%;margin: 120rpx auto 0 auto;background-color: #fff;border-radius: 10rpx;line-height: 80rpx;}
</style>
