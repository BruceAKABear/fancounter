<template>
	<view class="content">
		<image class="logo" src="../../static/logo.jpg" mode="scaleToFill"></image>
		<view class="text-area">
			<text class="title">{{ title }}</text>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			title: 'keep creative'
		};
	},
	onLoad() {
		setTimeout(function(){
			uni.reLaunch({
				url:'../setting/setting'
			})
		},2000)
		
	},
	methods: {}
};
</script>

<style lang="scss">
.content {
	position: absolute;
	width: 400rpx;
	height: 400rpx;
	
	top: 50%;
	left: 50%;
	transform: translate(-50%,-50%);	
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	text-align:center;
	.logo {
		height: 300rpx;
		width: 400rpx;
	}
	.text-area {
		display: flex;
		justify-content: center;
		.title {
			font-size: 40rpx;
			color: #8f8f94;
			text-align: center;
		}
	}
}
</style>
