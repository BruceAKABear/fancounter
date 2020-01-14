<template>
	<view class="content">
		<!-- 蓝牙连接信息 -->
		<view class="content-header">
			<w-loading text="加载中.." mask="true" click="true" ref="loading"></w-loading>
			<!-- 蓝牙图标容器 -->
			<view class="content-header-connectbox">
				<view class="btinfobox" v-if="bluetoothConnected">
					<image class="bticon" src="../../static/lanya-on.png" mode="scaleToFill"></image>
					<text style="color: #007AFF;">已连接</text>
				</view>
				<view class="btinfobox" v-else>
					<image class="bticon" src="../../static/lanya-off.png" mode="scaleToFill"></image>
					<text>未连接</text>
				</view>
			</view>
			<!-- 蓝牙设备电量容器 -->
			<view class="content-header-batterybox" v-if="bluetoothConnected">
				<image class="battery" v-if="battery>0&&battery<=10" src="../../static/1.png" mode="scaleToFill"></image>
				<image class="battery" v-if="battery>10&&battery<=50" src="../../static/10.png" mode="scaleToFill"></image>
				<image class="battery" v-if="battery>50&&battery<=90" src="../../static/60.png" mode="scaleToFill"></image>
				<image class="battery" v-if="battery>90&&battery<=100" src="../../static/90.png" mode="scaleToFill"></image>
				<text>{{battery}}%</text>
			</view>
		</view>
		<!-- 体信息 -->
		<view class="content-body">
			<!-- 输入参数 -->
			<view class="content-body-setting-sys padding radius shadow-warp bg-white">
				<view class="content-body-setting-sys-name">
					<text>系统参数设置</text>
				</view>
				<view class="content-body-setting-sys-content ">
					<view class="content-body-setting-sys-content-col">
						<view class="title">工作模式</view>
						<radio-group @change="workTypeChange" class="content-body-setting-sys-content-col-radioinline">
							<label class="content-body-setting-sys-content-col-radio" v-for="(item, index) in workTypes" :key="item.value">
								<view>
									<radio :value="item.value" :checked="item.value == workType" />
								</view>
								<view>{{item.name}}</view>
							</label>
						</radio-group>
					</view>
					<view class="content-body-setting-sys-content-col">
						<view class="title">WIFI SSID</view>
						<input v-model="wifiSSID" class="uni-input" focus placeholder="请输入WiFi SSID" />
					</view>
					<view class="content-body-setting-sys-content-col">
						<view class="title">WIFI 密码</view>
						<input v-model="wifiPWD" class="uni-input" focus placeholder="请输入WiFi密码" />
					</view>
					<!-- 如果是粉丝计数器模式的话，需要输入up主相关信息 -->
					<view class="content-body-setting-sys-content-col" v-if="workType==1">
						<view class="title">bilibili用户id</view>
						<input v-model="bilibiliId" class="uni-input" focus placeholder="请输入bilibili用户ID" />
					</view>
					<view class="content-body-setting-sys-content-col" v-if="workType==1">
						<view class="title">youtube-key</view>
						<input v-model="youtubeKey" class="uni-input" focus placeholder="请输入youtubeKey" />
					</view>
					<view class="content-body-setting-sys-content-col" v-if="workType==1">
						<view class="title">头像设置</view>
						<input v-model="headPic" class="uni-input" focus placeholder="123" />
					</view>
				</view>
			</view>
			<view class="content-body-setting-firameware radius shadow-warp bg-white">
				<view class="content-body-setting-firameware-name">
					<text>固件管理</text>
				</view>
				<view class="content-body-setting-firameware-content ">

				</view>
			</view>
		</view>
		<view class="content-buttom">
			<button class="cu-btn bg-green round lg shadow" @tap="save" :loading="!bluetoothConnected">{{bluetoothConnected?'保存':'蓝牙连接中'}}</button>
		</view>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				//电池电量
				battery: 50,
				deviceId: '',
				mainUUID: '',
				writeUUID: '',
				notifyUUID: '',
				readUUID: '',
				bluetoothConnected: true,
				//wifissid
				wifiSSID: '',
				//wifi密码
				wifiPWD: '',
				//bilibili用户id
				bilibiliId: '',
				headPic: '',
				//youtube的key
				youtubeKey: '',
				workTypes: [{
						value: 1,
						name: '粉丝计数器'
					}, {
						value: 2,
						name: '天气预报'
					},

				],
				//计数器工作模式，1为粉丝计数器，2为天气预报
				workType: 1,
				//是否有新版本固件
				haveNewVersion: true
			}
		},
		onLoad() {
			//加载时，获取手机已经连接的wifi
			var that = this
			// #ifdef MP-WEIXIN
			wx.startWifi({
				success(res) {
					console.info('开启wifi模块成功')
					wx.getConnectedWifi({
						complete(res) {
							console.log(res)
							//如果wifi连接上
							if (res.errCode == 0) {
								that.wifiSSID = res.wifi.SSID
							}
						}
					})
				}
			})

			// #endif


		},
		onShow() {
			//显示加载中
			// uni.showLoading({
			// 	title: '连接中',
			// 	mask: true
			// })
			//每次页面显示时连接蓝牙
			//this.connectBluetooth()

		},
		onHide() {
			//每次页面隐藏时关闭蓝牙
			//this.closeBluetooth(this.deviceId)
		},
		methods: {
			connectBle() {
				var that = this
				setTimeout(function() {
					that.btconnected = true
					uni.hideLoading()
				}, 4000)
			},
			/**
			 * 关闭蓝牙连接
			 * @param {Object} deviceId 设备id
			 */
			closeBluetooth(deviceId) {
				uni.closeBLEConnection({
					deviceId,
					success(res) {
						console.info('页面隐藏，断开蓝牙成功')
						uni.closeBluetoothAdapter({
							success(res) {
								console.info('页面隐藏关闭蓝牙适配器成功')
							}
						})
					}
				})

			},
			/**
			 * 连接蓝牙
			 */
			connectBluetooth() {
				var that = this
				try {
					const platform = uni.getStorageSync('platform');
					const version = uni.getStorageSync('version');
					if (platform && version) {
						uni.openBluetoothAdapter({
							success(res) {
								//蓝牙开关打开，直接连接蓝牙
								that.doConnect(platform, version)
							},
							fail(res) {
								uni.hideLoading()
								//蓝牙未打开，提示用户开启手机蓝牙
								uni.showModal({
									title: '提示',
									content: '使用小程序请开启手机蓝牙',
									showCancel: false,
									mask: true,
									success: function(res) {
										if (res.confirm) {
											console.log('用户点击确定');
										}
									}
								})

							}
						})
					}
				} catch (e) {
					console.error('连接蓝牙时获取系统信息失败')
					uni.showModal({
						title: '错误',
						content: '获取小程序系统信息失败，请重试',
						showCancel: false,
						success: function(res) {
							if (res.confirm) {
								uni.reLaunch({
									url: '../index/index'
								})
							}
						}
					})

				}
			},
			/**
			 * 真实连接蓝牙方法
			 */
			doConnect(platform, version) {
				var that = this
				//1. 开启搜索服务
				var searchTimeoutId = setTimeout(function() {
					uni.hideLoading()
					uni.stopBluetoothDevicesDiscovery({
						success(res) {
							console.info('蓝牙搜索超时，关闭搜索服务成功')
						}
					})
					if (that.deviceId == '') {
						uni.showModal({
							title: '提示',
							content: '未搜索到设备请靠近设备后重试',
							showCancel: false,
							success: function(res) {
								if (res.confirm) {
									uni.reLaunch({
										url: '../index/index'
									})
								}
							}
						})
					}
				}, 5000)

				uni.startBluetoothDevicesDiscovery({
					services: [that.mainUUID],
					success(res) {
						var deviceId = res.deviceId
						//搜索到设备,关闭搜索服务
						uni.stopBluetoothDevicesDiscovery({
							success(res) {
								console.info('搜索到设备，关闭搜索服务成功')
							}
						})
						if ('android' == platform) {
							//安卓设备直接连接
							uni.createBLEConnection({
								deviceId,
								success(res) {
									console.info('创建蓝牙连接成功')
									that.bluetoothConnected = true
								}
							})

						} else {
							//ios设备要搜索一下特征才能使用特征
							uni.createBLEConnection({
								deviceId,
								success(res) {
									that.bluetoothConnected = true
									console.info('创建蓝牙连接成功')
									uni.getBLEDeviceServices({
										deviceId,
										success(res) {
											console.info('ios设备搜索蓝牙服务成功', res.services)
											//搜索特征
											uni.getBLEDeviceCharacteristics({
												deviceId,
												serviceId: that.mainUUID,
												success(res) {
													console.info('ios设备搜索特征成功', res)
												}
											})
										}
									})
								}
							})
						}

					}
				})


			},
			workTypeChange(e) {
				console.log('工作模式改变')
				this.workType = e.detail.value

			},
			save() {
				//保存下发配置到粉丝计数器
				if (this.workType == 1) {
					//粉丝计数器模式
					if (this.wifiSSID != '' && this.wifiPWD != '' && this.bilibiliId != '') {

					} else {
						console.error('请将数据填写完整')

					}

				} else {

				}


			}



		},
		computed: {
			btnmessage() {
				return this.btconnected ? '蓝牙连接中' : '保存'
			}
		}
	}
</script>

<style lang="scss">
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

	.content-body {
		width: auto;
		margin-top: 120rpx;
		padding: 30rpx;
		color: #B2B2B2;

		.content-body-setting-sys {
			.content-body-setting-sys-content {
				margin-top: 20rpx;

				.content-body-setting-sys-content-col {
					display: flex;
					padding-bottom: 20rpx;
					align-items: center;

					input {
						margin-left: 30rpx;
						border-bottom: solid 1rpx #BBBBBB;
					}

					radio-group {
						margin-left: 30rpx;
					}

					.content-body-setting-sys-content-col-radioinline {
						display: flex;

						.content-body-setting-sys-content-col-radio {
							text-align: center;
							display: flex;
							flex-direction: column;
							justify-content: space-between;
						}
					}
				}

			}
		}

		.content-body-setting-firameware {
			margin-top: 30rpx;

			.content-body-setting-firameware-name {
				margin-bottom: 20rpx;
			}

			.content-body-setting-firameware-content {
				height: 200rpx;
			}
		}

	}

	.content-buttom {
		position: absolute;
		bottom: 80rpx;
		left: 50%;
		transform: translate(-50%);

		button {
			width: 600rpx;
		}
	}
</style>
