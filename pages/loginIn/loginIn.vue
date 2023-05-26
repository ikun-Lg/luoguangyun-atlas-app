<template>
	<view class="loginIn">
		<view class="image">
			<image src="../../static/images/Cloud Illustration.png" mode=""></image>
		</view>
		<view class="formBox">
			<h3>用户名</h3>
			<input type="text" v-model="username" placeholder="请输入用户名" id="username"><br>
			<h3>密码</h3>
			<input type="password" v-model="password" placeholder="请输入密码" id="password">
			<button @click="login()">登录</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: "",
				password: ""
			};
		},
		methods: {
			login() {
				uni.request({
					url: `http://cloudapi.luoguang.icu:9000/api/users/login?username=${this.username}&password=${this.password}`,
					success: (res) => {
						console.log(res);
						if (res.data.code == 200) {
							uni.redirectTo({
								url: '/pages/index/index'
							})
							uni.setStorageSync('userinfo', res.data)
						} else if (res.data.code == 500) {
							uni.showToast({
								title: '账号或密码错误',
								icon: 'error'
							})
						}
					}
				})
			}
		},
	}
</script>

<style lang="scss" scoped>
	.loginIn {
		width: 750rpx;
		height: 100vh;
		background: rgba(25, 26, 35, 1);
		position: relative;

		.image {
			position: absolute;
			top: 20%;
			width: 100%;
		}

		.formBox {
			position: absolute;
			top: 60%;
			left: 50%;
			transform: translate(-50%, -50%);
			width: 600rpx;
			height: 500rpx;
			background: rgba(34, 35, 56, 0.85);
			box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
			backdrop-filter: blur(4px);
			-webkit-backdrop-filter: blur(4px);
			border-radius: 20rpx;
			border: 1px solid rgba(255, 255, 255, 0.18);
			display: flex;
			flex-direction: column;
			justify-content: space-around;
			align-items: center;


			h3 {
				color: rgba(60, 68, 192, 1);

			}

			#username {
				text-align: center;
				color: rgba(60, 68, 192, 1);
				border-bottom: 2rpx solid #515151;
			}

			#password {
				text-align: center;
				color: rgba(60, 68, 192, 1);
				border-bottom: 2rpx solid #515151;
			}

			button {
				color: white;
				appearance: none;
				background: rgba(60, 68, 192, 1);
				border: 4rpx solid #1A1A1A;
				width: 200rpx;
				height: 80rpx;
				line-height: 80rpx;
				border-radius: 0.9375em;
			}
		}

	}
</style>