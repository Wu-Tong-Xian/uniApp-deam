<template>
	<view class="">
		<!-- 没有登录的状态 -->
		<view v-if="!flag" class="bg">
			<view class="fl bg-box">
				<view class="xmr-box">
					<!-- 头像 -->
					<image class="xmr" :src="date.faceImage" mode=""></image>
				</view>
				<view @click="registLogin" class="l-h">登录 / 注册 </view>
			</view>
			<view class="fz18 mg mg-l20 col6">
				欢迎来到最好的热门电影，我们热诚打造您满意的影院，
			</view>
			<view class="fz14 mg-t20 t-center  col6">
				温馨提示：登录之后食用效果更佳~~~
			</view>
		</view>
		<view v-else class="bg">
			<view class="fl bg-box">
				<view class="p-r">
					<!-- 设置个人信息 -->
					<image @click="install" class="sz" src="../../static/image/sz.png" mode=""></image>
				</view>
				<view class="xmr-box">
					<!-- 头像 -->
					<image class="xmr" :src="date.faceImage" mode=""></image>
				</view>
				<view class="l-h">
					<!-- 用户名 -->
					<view @click="" class="colr fz18">{{date.nickname}} </view>
					<view @click="quitLogin" class="">退出登录 </view>
				</view>
			</view>
			<view class="fz18 mg mg-l20 col6">
				温馨提示：充值会员享有更多特权~~~
			</view>
			<view class="fz14 mg-t20 t-center  col6">
				氪金使我快乐~~~
			</view>
		</view>
	</view>
</template>

<script>
	
	export default {
		name: "",
		components: {
			
		},
		props: {},
		data() {
			return {
				date: '',
				name: '',
				flag: false
			}
		},
		methods: {
			// 设置修改个人信息
			install() {
				uni.navigateTo({
					url: `/pages/install/install`
				})
			},
			// 登录注册
			registLogin() {
				uni.navigateTo({
					url: `/pages/registLogin/registLogin`
				})
			},
			// 退出登录
			quitLogin() {
				uni.showLoading({
					title: '加载中'
				})
				uni.request({
					url: `${this.$api}/user/logout?qq=434714873&userId=this.date.id`,
					method: 'POST',
					data: {},
					success: res => {
						uni.hideLoading()
						uni.removeStorageSync('date')
						this.flag = false
						uni.showToast({
							title: '已退出登录',
							icon: 'none'
						})
					},
					fail: () => {
						uni.hideLoading()
					},
					complete: () => {}
				});
			},
			// 上拉刷新
			onPullDownRefresh() {
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000);
			
			},
		},
		mounted() {},

		onShow() {
			if (uni.getStorageSync('date')) {
				this.date = uni.getStorageSync('date')
				console.log(this.date);
				this.flag = true
			}
			
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
	.bg {
		background: url(../../static/image/mv.jpg);
		width: 100%;
		height: 1220rpx;
	}

	.sz {
		width: 50rpx;
		height: 50rpx;
		position: absolute;
		top: 30rpx;
		left: 650rpx;

	}

	.xmr-box {
		width: 150rpx;
		height: 150rpx;
		border-radius: 50%;
	}

	.bg-box {
		width: 100%;
		height: 400rpx;
		background: rgba(155, 215, 12, 0.4);
	}

	.xmr {
		width: 150rpx;
		height: 150rpx;
		border-radius: 50%;
		margin-top: 120rpx;
		margin-left: 100rpx;
	}

	.l-h {
		line-height: 50rpx;
		color: #00ffff;
		margin-top: 140rpx;
		margin-left: 150rpx;
	}

	.mg {
		margin-top: 150rpx;
	}
</style>
