<template>
	<view class="tabBarBox">


		<view class="tabBarBottom">
			<view class="tab" @click="gotoIndex()">
				<image
					:src=" tabIndex == 1 ?  '../../static/icons/files_selected.png' :  '../../static/icons/files.png'">
				</image>
			</view>
			<view class="updateFileBox" @click="UpdateImage()">
				+
			</view>
			<view class="tab" @click="gotoUser()">
				<image :src=" tabIndex == 2  ? '../../static/icons/user_selected.png' : '../../static/icons/user.png'">
				</image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "tabBar",
		props: {
			tabIndex: {
				type: Number,
				default: 1
			},
			iduser: {
				type: Number,
				default: 0
			}
		},
		data() {
			return {
				newIduser: 0
			};
		},
		onLoad() {
			this.getUserInfo()
		},
		methods: {
			getUserInfo() {
				let userInfoData = uni.getStorageSync('userinfo')
				this.userInfo.iduser = userInfoData.iduser
			},
			gotoIndex() {
				uni.navigateTo({
					url: '/pages/index/index'
				})
			},
			gotoUser() {
				uni.navigateTo({
					url: '/pages/user/user'
				})
			},
			//上传文件
			UpdateImage() {
				console.log(this.iduser);
				uni.chooseImage({
					success: (chooseImageRes) => {
						const tempFilePaths = chooseImageRes.tempFilePaths;
						uni.showToast({
							title: '文件上传中,请稍后',
							icon: 'loading'
						})

						uni.uploadFile({
							url: `http://cloudapi.luoguang.icu:9000/api/users/portraitfile?iduser=${this.iduser}`,
							filePath: tempFilePaths[0],
							name: 'file',
							// formData: {
							// 	'user': 'test'
							// },
							success: (uploadFileRes) => {
								console.log(uploadFileRes.data);

								uni.showToast({
									title: '上传成功',
									icon: 'success'
								})
								uni.redirectTo({
									url: '/pages/index/index'
								})
							},
							fail: () => {
								uni.showToast({
									title: '上传失败',
									icon: 'error'
								})
							}
						});
					}
				});
			}
		},
	}
</script>

<style lang="scss" scoped>
	.tabBarBox {
		z-index: 999;
		position: fixed;
		bottom: 10rpx;
		left: 0;
		width: 100%;
		height: 150rpx;
		background-color: #262937;
		border-radius: 34rpx;


		.tabBarBottom {
			position: fixed;
			bottom: 0;
			left: 0;
			width: 750rpx;
			height: 182rpx;
			box-shadow: 0px 0px 86px 0px rgba(75, 11, 192, 0.03);
			display: flex;
			justify-content: space-around;
			align-items: center;


			.updateFileBox {
				border-radius: 50%;
				width: 115rpx;
				height: 115rpx;
				background: rgba(60, 68, 192, 1);
				text-align: center;
				line-height: 115rpx;
				color: white;
				font-size: 80rpx;
			}

			.tab {
				width: 50rpx;
				height: 40rpx;
				// background-color: #fff;

				image {
					width: 100%;
					height: 100%;
				}
			}
		}

	}
</style>