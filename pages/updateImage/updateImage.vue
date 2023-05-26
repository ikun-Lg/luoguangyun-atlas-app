<template>
	<view class="updateImage">
		<view class="formBox">
			<uploadFile :url="url" :accept="accept" @upFileChange="upFileChange"></uploadFile>

			</form>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			};
		}
	}
</script>

<style lang="scss" scoped>
	.updateImage {
		.formBox {
			width: 600rpx;
			height: 400rpx;
		}
	}
</style>