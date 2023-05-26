<template>
	<view class="index">
		<view class="top">
			<image :src="userInfo.avator ? userInfo.avator : '../../static/icons/default.jpg' "></image>
		</view>

		<scroll-view scroll-y="true" class="scroll-Y">
			<image :src="item.imageurl" v-for="item in imagesUrls" mode="aspectFill" :key="item.idimages"
				@longpress="previewImage(item.imageurl)" @click="gotoDetail(item)"></image>
		</scroll-view>

		<tabBar :tabIndex="1" :iduser="userInfo.iduser"></tabBar>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					avator: '',
					username: '',
					password: '',
					iduser: 1
				},
				imagesUrls: []
			}
		},
		onLoad() {
			this.getUserInfo()
			this.getAllimages()
		},
		onPullDownRefresh() {
			this.getAllimages()
		},
		methods: {
			//读取本地用户信息
			getUserInfo() {
				let userInfoData = uni.getStorageSync('userinfo')
				this.userInfo.avator = userInfoData.avatorurl
				this.userInfo.username = userInfoData.username
				this.userInfo.password = userInfoData.password
				this.userInfo.iduser = userInfoData.iduser

				uni.request({
					url: `http://cloudapi.luoguang.icu:9000/api/users/login?username=${this.userInfo.username}&password=${this.userInfo.password}`,
					success: (res) => {
						this.userInfo.avator = res.data.avatorurl
						uni.setStorageSync('userinfo', res.data)
					}

				})
			},
			//用户获取所有图片
			getAllimages() {
				uni.request({
					url: `http://cloudapi.luoguang.icu:9000/api/users/getallimages?iduser=${this.userInfo.iduser}`,
					success: (res) => {
						this.imagesUrls = res.data.imagesurl
						if (this.imagesUrls != []) {
							uni.stopPullDownRefresh()
						}
						uni.stopPullDownRefresh()
					}
				})
			},
			//点击图片预览
			previewImage(imageUrl) {
				uni.previewImage({
					urls: [imageUrl],
					current: 0
				})
			},
			gotoDetail(image) {
				uni.redirectTo({
					url: `/pages/imageDetail/imageDetail?imageurl=${image.imageurl}&idimages=${image.idimages}`,
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.index {
		width: 100vw;
		height: 100vh;
		background: rgba(25, 26, 35, 1);

		.top {
			position: fixed;
			left: 44rpx;
			top: 82rpx;
			width: 86rpx;
			height: 86rpx;
			// background-color: #fff;

			image {
				width: 100%;
				height: 100%;
				border-radius: 16rpx;
			}
		}

		.scroll-Y {
			position: fixed;
			top: 220rpx;
			left: 0;
			height: 1000rpx;
			width: 100%;
			display: flex;
			flex-wrap: wrap;
			align-items: center;
			margin: 0 auto;
			justify-content: space-around;


			image {
				margin: 20rpx;
				width: 317rpx;
				height: 317rpx;
				border-radius: 16rpx;
			}

		}
	}
</style>