<template>
	<view>
		<view class="content">
			<!-- logo和标语 -->
			<view class="logo-slogan">
				<view class="logo-pic">
					<image class="logo" src="../../static/logo.jpg" mode="scaleToFill"></image>
				</view>
				<view class="logo-slogan">
					<text class="slogan">{{ title }}</text>
				</view>
			</view>
		</view>
		<!-- 加入我 -->
		<join-me></join-me>
	</view>
</template>

<script>
	import JoinMe from '../../components/join-me/join-me.vue'

	export default {
		data() {
			return {
				title: 'keep creative'
			};
		},
		onLoad() {
			setTimeout(function() {
				uni.reLaunch({
					url: '../setting/setting'
				})
			}, 2000)

		},
		methods: {},
		components: {
			JoinMe
		}
	};
</script>

<style lang="scss">
	.content {
		position: absolute;
		top: 40%;
		left: 50%;
		transform: translate(-50%, -50%);

		.logo-slogan {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			.logo-pic {
				.logo {
					height: 400rpx;
					width: 400rpx;
				}
			}

			.logo-slogan {
				text-align: center;

				.slogan {
					font-size: 60rpx;
					color: #8f8f94;
				}
			}
		}
	}
</style>
