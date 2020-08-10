<template>
	<view>

		<view class="bodyer">
			<image class="fengmiantu" :src="photo" mode="" @click="open"></image>
		</view>
		<uni-popup ref="popup" type="bottom">
			<view class="bt-1" @click="keepPhoto">保存图片到本地</view>
			<view class="bt-2" @click="close">取消</view>
		</uni-popup>

	</view>
</template>

<script>
	import uniPopup from '../../components/uni-ui/uni-popup/uni-popup.vue'
	import uniPopupMessage from '../../components/uni-ui/uni-popup/uni-popup-message.vue'
	import uniPopupDialog from '../../components/uni-ui/uni-popup/uni-popup-dialog.vue'
	export default {
		name: "",
		components: {
			uniPopup,
			uniPopupMessage,
			uniPopupDialog
		},
		props: {},
		data() {
			return {

				photo:''
			}
		},
		methods: {
			close(){
			this.$refs.popup.close()	
			},
			open() {
				this.$refs.popup.open()
			},
			// 保存照片至本地
						keepPhoto() {
							let that = this
							uni.showActionSheet({
								itemList: ['您是否要下载该照片   确定？'],
								success: function(res) {
									if (res.tapIndex === 0) {
										let ua = uni.getSystemInfoSync().platform;
										// 如果不是ios(h5)
										if (ua !== "ios") {
											uni.showLoading({
												title:"下载中...",
												icon:'none'
											})
											uni.downloadFile({
												url: that.photo,
												success: res => {
													uni.hideLoading()
													uni.showToast({
														title:'下载成功',
														icon: 'success',
														duration: 2000
													})
													console.log(res);
													let photo = res.tempFilePath;
													uni.saveImageToPhotosAlbum({
														filePath: photo,
														success(res) {
															uni.showToast({
																title: '保存成功',
																icon: 'success',
																duration: 2000
															})
														}
													})
												},
											})
										} else {
											uni.showToast({
												title: 'H5不支持下载',
												icon:'none'
											})
										}
									}
								},
								fail: function(res) {
									console.log(res.errMsg);
								}
							});
						},
		},
		mounted() {

		},
		onLoad(options) {
			console.log(options.photo)
			this.photo = options.photo
		},
		filters: {

		},
		computed: {

		},
		watch: {

		},
		directives: {

		}
	}
</script>

<style scoped lang="scss">
	.bodyer {
		width: 100%;
		height: 100%;
		background-color: black;
		position: absolute;
		justify-content: center;
		align-items: center;
		display: flex;
	}

	.fengmiantu {
		width: 90%;

	}

	.bt-1,
	.bt-2 {
		margin-top: 10rpx;
		height: 70rpx;
		line-height: 70rpx;
		text-align: center;

		background-color: white;
	}
</style>
