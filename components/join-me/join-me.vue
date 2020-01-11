<template>
	<!-- 加盟代理 -->
	<view class="agent-box">
		<view class="agent-box-info">
			<text>了解更多有趣好玩的创意请关注</text>
		</view>
		<view class="agent-box-contact">
			<text>公众号</text>
			<view @tap="jump2()">
				<text style="color:#29c1c2;">【BLab大熊实验室】</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'JoinUs',
		props: {
			hotline: {
				type: String,
				default: '4000032400'
			}
		},
		methods: {
			jump2() {
				console.log('123')
			}

		}


	}
</script>

<style>
	.agent-box {
		position: fixed;
		bottom: 8%;
		left: 50%;
		transform: translate(-50%);
		justify-content: center;
		font-size: 25rpx;
		color: #BBBBBB;
	}

	.agent-box-info {
		display: flex;
		justify-content: center;
		text-align: center;

	}

	.agent-box-contact {
		display: flex;
		justify-content: center;
		text-align: center;
		width: 600rpx;
	}
</style>
