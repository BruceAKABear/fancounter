<template>
	<view class="content">
		<!-- 蓝牙连接信息 -->
		<view class="content-header">
			<!-- 蓝牙图标容器 -->
			<view class="content-header-connectbox">
				<view class="btinfobox" v-if="btconnected">
					<image class="bticon" src="../../static/lanya-on.png" mode="scaleToFill"></image>
					<text>已连接</text>
				</view>
				<view class="btinfobox" v-else>
					<image class="bticon" src="../../static/lanya-off.png" mode="scaleToFill"></image>
					<text>未连接</text>
				</view>
			</view>
			<!-- 蓝牙设备电量容器 -->
			<view class="content-header-batterybox" v-if="btconnected">
				<image class="battery" src="../../static/lanya-off.png" mode="scaleToFill"></image>
				<text>200%</text>
			</view>
		</view>
		<view class="content-body"></view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			btconnected: true
		};
	}
};
</script>

<style lang="scss">
.content {
	display: flex;
	flex-direction: column;
	.content-header {
		display: flex;
		position: absolute;
		top: 30rpx;
		right: 30rpx;
		width:25%;
		.content-header-connectbox {
			width:50%;
			.bticon {
				width: 40rpx;
				height: 40rpx;
			}
			.btinfobox {
				display: flex;
				flex-direction: column;
				align-items: center;
				font-size: 25rpx;
			}
		}
		.content-header-batterybox {
			display: flex;
			font-size: 25rpx;
			margin-left: 25rpx;
			width: 50%;
			align-items: center;
			justify-content: center;
			flex-direction: column;
			.battery {
				width: 40rpx;
				height: 40rpx;
			}
		}
	}
}
</style>
