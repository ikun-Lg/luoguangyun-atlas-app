<template>
	<view class="detail">
		<image :src="imageInfo.imageurl" mode="widthFix"></image>
		<view class="bottom">
			<view class="buttons">
				<image src="../../static/icons/download.png" @click="downloadImage()"></image>
			</view>
			<view class="buttons">
				<image src="../../static/icons/delete.png" @click="deleteImage()"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imageInfo: {
					imageurl: '',
					idimages: null
				}
			};
		},
		onLoad(e) {
			let {
				imageurl,
				idimages
			} = e

			this.imageInfo.idimages = idimages
			this.imageInfo.imageurl = imageurl
			console.log(this.imageInfo.idimages);
		},
		methods: {
			//下载图片
			downloadImage() {
				uni.downloadFile({
					url: this.imageInfo.imageurl,
					success: (res) => {
						uni.showToast({
							title: '图片保存成功',
							icon: 'success'
						})
					},
					fail: () => {
						uni.showToast({
							title: '图片保存失败',
							icon: 'error'
						})
					}
				})
			},
			//删除图片
			deleteImage() {
				uni.request({
					method: 'DELETE',
					url: `http://cloudapi.luoguang.icu:9000/api/users/deleteimage?idimages=${this.imageInfo.idimages}`,
					success: () => {
						uni.showToast({
							title: '图片删除成功',
							icon: 'success'
						})
						uni.redirectTo({
							url: '/pages/index/index'
						})
					},
					fail: () => {
						uni.showToast({
							title: '删除失败',
							icon: 'error'
						})
					}
				})
			}
		},
	}
</script>

<style lang="scss">
	.detail {
		width: 100vw;
		height: 100vh;
		background: rgba(25, 26, 35, 1);

		image {
			width: 100%;
			margin-top: 20%;
		}

		.bottom {
			position: fixed;
			bottom: 0;
			left: 0;
			width: 100%;
			height: 100rpx;
			display: flex;
			justify-content: space-around;
			align-items: center;
			background-color: #3c43c0;
			border-radius: 10rpx;

			.buttons {
				width: 70rpx;
				height: 80rpx;
			}

			image {
				width: 100%;
				height: 100%;
			}
		}
	}
</style>