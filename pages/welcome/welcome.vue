<template>
	<view class="welcome">
		<view class="image">
			<image src="../../static/images/Cloud Illustration.png" mode=""></image>
		</view>
		<view class="text">
			<text class="text_1">Welcome to</text>
			<text class="text_2">落光云图册</text>
			<text class="text_3">Best cloud storage platform for ikun.</text>
		</view>
		<view class="buttons">
			<view class="signin" @click="gotoSignin()">注册</view>
			<view class="loginin" @click="gotoLoginin()">登录</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {};
		},
		onLoad() {

		},
		methods: {
			gotoSignin() {
				uni.navigateTo({
					url: '/pages/signIn/signIn'
				})
			},
			gotoLoginin() {
				uni.navigateTo({
					url: '/pages/loginIn/loginIn'
				})
			}
		},
	}
</script>

<style lang="scss" scoped>
	* {
		padding: 0;
		margin: 0 auto;
		box-sizing: border-box;
	}

	.welcome {
		width: 750rpx;
		height: 100vh;
		background: rgba(25, 26, 35, 1);

		.image {
			position: absolute;
			top: 20%;
			width: 100%;
		}

		.text {
			position: absolute;
			left: 60rpx;
			top: 858rpx;
			width: 464rpx;
			height: 184rpx;
			display: flex;
			flex-direction: column;
			justify-content: space-around;

			/** 文本1 */
			.text_1 {
				opacity: 0.4;
				font-size: 34rpx;
				font-weight: 500;
				letter-spacing: 0px;
				line-height: 40rpx;
				color: rgba(255, 255, 255, 1);
				text-align: left;
				vertical-align: top;
			}

			.text_2 {
				font-size: 80rpx;
				font-weight: 500;
				letter-spacing: 0px;
				line-height: 94rpx;
				color: rgba(255, 255, 255, 1);
				text-align: left;
				vertical-align: top;
			}

			.text_3 {
				opacity: 0.3;
				font-size: 30rpx;
				font-weight: 500;
				letter-spacing: 0px;
				line-height: 36rpx;
				color: rgba(255, 255, 255, 1);
				text-align: left;
				vertical-align: top;
			}

		}

		.buttons {
			position: absolute;
			left: 60rpx;
			top: 1194rpx;
			width: 606rpx;
			height: 192rpx;
			// background-color: pink;
			display: flex;
			justify-content: space-between;


			.signin {
				left: 0px;
				top: 74rpx;
				width: 290rpx;
				height: 118rpx;
				border-radius: 17px;
				background: rgba(97, 104, 210, 0.12);
				font-size: 30rpx;
				font-weight: 600;
				letter-spacing: 0px;
				line-height: 118rpx;
				color: rgba(60, 68, 192, 1);
				text-align: center;
				vertical-align: top;
			}

			.loginin {
				left: 0px;
				top: 74rpx;
				width: 290rpx;
				height: 118rpx;
				border-radius: 17px;
				background: rgba(60, 68, 192, 1);
				font-size: 30rpx;
				font-weight: 600;
				letter-spacing: 0px;
				line-height: 118rpx;
				color: rgba(255, 255, 255, 1);
				text-align: center;
				vertical-align: top;
			}
		}
	}
</style>