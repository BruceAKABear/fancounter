<script>
	export default {
		onLaunch: function() {
			//应用加载时获取平台的信息
			this.getPlatform()
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		},
		methods: {
			getPlatform() {
				uni.getSystemInfo({
					success: function(res) {
						try {
							//保存系统类型和微信版本
							uni.setStorageSync('platform', res.platform);
							uni.setStorageSync('version', res.version);
						} catch (e) {
							// error
							console.error('保存系统图信息错误')
						}
					}
				})

			}
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import "colorui/main.css";
	@import "colorui/icon.css";
	@import '../../common/uni.css'

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100%;

	}
</style>
