<template>
	<view class="content">
		<!-- 蓝牙连接信息 -->
		<view class="content-header">
			<w-loading text="加载中.." mask="true" click="true" ref="loading"></w-loading>
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
				<image class="battery" v-if="battery>0&&battery<=10" src="../../static/1.png" mode="scaleToFill"></image>
				<image class="battery" v-if="battery>10&&battery<=50" src="../../static/10.png" mode="scaleToFill"></image>
				<image class="battery" v-if="battery>50&&battery<=90" src="../../static/60.png" mode="scaleToFill"></image>
				<image class="battery" v-if="battery>90&&battery<=100" src="../../static/90.png" mode="scaleToFill"></image>
				<text>{{battery}}%</text>
			</view>
		</view>
		<view class="content-body">
			<!-- 输入参数 -->
			<!-- 选择模式 -->
			<!-- 按钮 -->
			<input type="text" value="123" />
		</view>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				btconnected: false,
				battery: 50
			}
		},
		onLoad() {
			uni.showLoading({
				title: '连接中',
				mask: true
			})
			//连接蓝牙
			this.connectBle()
		},
		methods: {
			connectBle() {
				var that = this
				setTimeout(function() {
					that.btconnected = true
					uni.hideLoading()
				}, 4000)
			}
		}
	}
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
			width: 25%;

			.content-header-connectbox {
				width: 50%;

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
					width: 28rpx;
					height: 40rpx;
				}
			}
		}
		.content-body{
			position: absolute;
			top: 10%;
			background-color: #000000;
			width: 100%;
		}
	}
</style>
