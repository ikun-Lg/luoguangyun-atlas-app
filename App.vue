<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
			const value = uni.getStorageSync("userinfo");
			if (!value) {
				uni.redirectTo({
					url: '/pages/welcome/welcome'
				})
				// } else {
				// 	uni.redirectTo({
				// 		url: '/pages/index/index'
				// 	})
			} else if (value) {
				uni.redirectTo({
					url: '/pages/index/index'
				})

			}
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */

	* {
		padding: 0;
		margin: 0 auto;
		box-sizing: border-box;
	}
</style>