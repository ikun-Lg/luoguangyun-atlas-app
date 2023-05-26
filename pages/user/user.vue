<template>
	<view class="user">
		<view class="avatorBox">
			<image :src="userInfo.avatorUrl ? userInfo.avatorUrl : '../../static/icons/default.jpg'" mode="aspectFill"
				@click="updateAvator()"></image>
			<h3>{{userInfo.username}}</h3>
			<button class="out" @click="out()">
				注销
			</button>
		</view>

		<view class="echartBox">
			<qiun-data-charts type="arcbar" :opts="opts" :chartData="chartData" />
		</view>
		<tabBar :tabIndex="2" :iduser="userInfo.iduser"></tabBar>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				countImages: 0,
				userInfo: {
					avatorUrl: '../../static/icons/default.jpg',
					username: 'ikun',
					iduser: 1
				},
				chartData: {},
				opts: {
					color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4",
						"#ea7ccc"
					],
					padding: undefined,
					title: {
						name: 0 + '%',
						fontSize: 35,
						color: "white"
					},
					subtitle: {
						name: "已使用",
						fontSize: 25,
						color: "#666666"
					},
					extra: {
						arcbar: {
							type: "default",
							width: 12,
							backgroundColor: "#E9E9E9",
							startAngle: 0.75,
							endAngle: 0.25,
							gap: 2,
							linearType: "custom"
						}
					}
				}
			};
		},
		onLoad() {
			this.getUserInfo()
			this.getCount()
			this.showEchart()
		},
		methods: {
			//分享功能
			share() {
				uni.share({
					provider: "weixin",
					scene: "WXSceneSession",
					type: 0,
					href: "https://gitee.com//luoguangguang/luoguang-weather/releases/download/2.4.1/__UNI__EF86916__20230423133442.apk",
					title: "落光云图册",
					summary: "我正在使用HBuilderX开发uni-app，赶紧跟我一起来体验！",
					imageUrl: "https://luoguang.icu/static/img/SmallPic.9509f92.jpg",
					success: function(res) {
						console.log("success:" + JSON.stringify(res));
					},
					fail: function(err) {
						console.log("fail:" + JSON.stringify(err));
					}
				});
			},
			//注销
			out() {
				uni.removeStorageSync('userinfo')
				uni.redirectTo({
					url: '/pages/welcome/welcome'
				})
			},
			getUserInfo() {
				let user = uni.getStorageSync('userinfo')
				let {
					avatorurl,
					username,
					iduser
				} = user

				this.userInfo.avatorUrl = avatorurl
				this.userInfo.username = username
				this.userInfo.iduser = iduser
			},
			//统计上传数量
			getCount() {
				uni.request({
					url: `http://cloudapi.luoguang.icu:9000/api/users/getallimages?iduser=${this.userInfo.iduser}`,
					success: (res) => {
						this.opts.title.name = res.data.imagesurl.length + '%'
						this.countImages = res.data.imagesurl.length
					}
				})
			},
			//展示echart
			showEchart() {
				setTimeout(() => {
					let res = {
						series: [{
							name: "已使用",
							color: "#3c44c0",
							data: this.countImages / 100
						}]
					};
					this.chartData = JSON.parse(JSON.stringify(res));
				}, 500)
			},
			//更换头像
			updateAvator() {
				uni.chooseImage({
					success: (chooseImageRes) => {
						const tempFilePaths = chooseImageRes.tempFilePaths;
						uni.showToast({
							title: '文件上传中,请稍后',
							icon: 'loading'
						})
						uni.uploadFile({
							url: `http://cloudapi.luoguang.icu:9000/api/users/avatorportraitfile?iduser=${this.userInfo.iduser}`,
							filePath: tempFilePaths[0],
							name: 'file',
							formData: {
								'user': 'test'
							},
							success: (uploadFileRes) => {
								let newUserInfo = uni.getStorageSync('userinfo')
								newUserInfo.avatorurl = JSON.parse(uploadFileRes.data).avatorurl
								uni.setStorageSync('userinfo', newUserInfo)
								uni.showToast({
									title: '头像更新成功',
									icon: 'success'
								})

								uni.redirectTo({
									url: '/pages/index/index'
								})
							},
							fail: () => {
								uni.showToast({
									title: '头像更新失败',
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
	* {
		padding: 0;
		margin: 0 auto;
		box-sizing: border-box;
	}

	.user {
		width: 100vw;
		height: 100vh;
		background: rgba(25, 26, 35, 1);

		.avatorBox {
			position: fixed;
			top: 7%;
			left: 50%;
			transform: translateX(-50%);
			width: 200rpx;
			height: 220rpx;

			.out {
				background-color: #fff;
				font-size: 34rpx;
				font-weight: bold;
				color: #3c43c1;
				background-color: #262937;
			}

			image {
				margin-top: 20rpx;
				height: 200rpx;
				width: 200rpx;
				border-radius: 17rpx;
			}

			h3 {
				text-align: center;
				color: white;
				font-size: 40rpx;
			}
		}

		.echartBox {
			position: fixed;
			top: 500rpx;
			left: 0;
			width: 100%;
			height: 600rpx;
		}
	}
</style>