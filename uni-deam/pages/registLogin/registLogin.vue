<template>
	<view class="bg">
		<view class="xmr-box">
			<image class="xmr" src="../../static/image/xmr.jpg" mode=""></image>
		</view>
		<view class="fl mg20 col6 bor-b">
			<view class="mg-r10">账户:</view>
			<input type="text" placeholder="请输入用户名" v-model="username" />
		</view>
		<view class="fl mg20 col6 bor-b">
			<view class="mg-r10"> 密码:</view>
			<input password=true placeholder="请输入密码" v-model="password" type="text" value="" />
		</view>
		<view class="mg-tb20">
			<button class="mg20" @click="registLogin" type="primary">注册 / 登录</button>
		</view>
		<!-- <view class="col6 t-center mg-tb20 fz16">
			第三方账号登录
		</view>
		<view class="fl  mg-t20">
			<image class="img m0" src="../../static/image/wechat.png" mode=""></image>
			<image class="img m0 " src="../../static/image/weibo.png" mode=""></image>
			<image class="img m0" src="../../static/image/qq.png" mode=""></image>
		</view> -->

		<!-- #ifndef H5 -->
		<view class="col6 t-center mg-tb20 fz16">第三方账号登录</view>
		<view class="fl  mg-t20">
			<image src="../../static/image/wechat.png" mode="" class="img m0" @click="weixinLogin" data-loginType="weixin"></image>
			<image src="../../static/image/qq.png" mode="" class="img m0" @click="qqLogin" data-loginType="qq"></image>
			<image src="../../static/image/weibo.png" mode="" class="img m0" @click="sinaweiboLogin" data-loginType="sinaweibo"></image>
		</view>
		<!-- 	#endif -->

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
				username: '',
				password: '',
	
			}
		},


		methods: {
			// 第三方微信的登录
			weixinLogin(e) {
				console.log(e);
				let loginType = e.currentTarget.dataset.logintype
				// 获取服务供应商。
				uni.getProvider({
					service: 'oauth',
					success: res => {
						if (~res.provider.indexOf('weixin')) {
							//uni 第三方（微信） 登录的方法
							uni.login({
								provider: 'weixin',
								success: res => {
									console.log(JSON.stringify(res));
									// 获取用户信息
									uni.getUserInfo({
										provider: 'weixin',
										success: res => {
											console.log(res);
											console.log(res.userInfo.nickName);
											let nickName = res.userInfo.nickName
											console.log(nickName);
											let openid = res.signature
											console.log(res.openid);
											console.log(res.userInfo.avatarUrl);
											let figureurl = res.userInfo.avatarUrl
											// 拿到数据在发请求
											uni.request({
												url: `${this.$api}/appUnionLogin/${loginType}?qq=434714873`,
												method: 'POST',
												data: {
													face: figureurl,
													nickname: nickName,
													openIdOrUid: openid
												},
												success: res => {
													console.log(res);
													uni.switchTab({
														url: '/pages/index/index'
													})
													
													uni.setStorageSync('date', res.data.data)
													console.log(res);
												},
												fail: err => {
													console.log(err);
												},
												complete: () => {}
											});
										}
									});
								}
							});
						}
					}
				});
			},
			sinaweiboLogin(e) {
				console.log(e);
				let loginType = e.currentTarget.dataset.logintype
				uni.getProvider({
					service: 'oauth',
					success: res => {
						if (~res.provider.indexOf('sinaweibo')) {
							uni.login({
								provider: 'sinaweibo',
								success: res => {
									console.log(JSON.stringify(res));
									uni.getUserInfo({
										provider: 'sinaweibo',
										success: res => {
											console.log(res);
											console.log(res.userInfo.nickName);
											let nickName = res.userInfo.nickName
											console.log(nickName);
											console.log(res.signature);
											let openid = res.signature

											console.log(res.userInfo.avatarUrl);
											let figureurl = res.userInfo.avatarUrl
											uni.request({
												url: `${this.$api}/appUnionLogin/${loginType}?qq=434714873`,
												method: 'POST',
												data: {
													face: figureurl,
													nickname: nickName,
													openIdOrUid: openid

												},
												success: res => {
													uni.switchTab({
														url: '/pages/index/index'
													})
													
													uni.setStorageSync('date', res.data.data)
													console.log(res);
												},
												fail: err => {
													console.log(err);
												},
												complete: () => {}
											});
										}
									});
								}
							});
						}
					}
				});
			},
			qqLogin(e) {
				console.log(e);
				let loginType = e.currentTarget.dataset.logintype
				uni.getProvider({
					service: 'oauth',
					success: res => {

						if (~res.provider.indexOf('qq')) {
							uni.login({
								provider: 'qq',
								success: res => {
									console.log(JSON.stringify(res));
									uni.getUserInfo({
										provider: 'qq',
										success: res => {
											console.log(res.userInfo.nickName);
											let nickName = res.userInfo.nickName
											console.log(nickName);
											console.log(res.signature);
											let openid = res.signature

											console.log(res.userInfo.figureurl);
											let figureurl = res.userInfo.figureurl
											uni.request({
												url: `${this.$api}/appUnionLogin/${loginType}?qq=434714873`,
												method: 'POST',
												data: {
													face: figureurl,
													nickname: nickName,
													openIdOrUid: openid

												},
												success: res => {
													uni.switchTab({
														url: '/pages/index/index'
													})
												
													uni.setStorageSync('date', res.data.data)
													
													console.log(res);
												},
												fail: err => {
													console.log(err);
												},
												complete: () => {}
											});
										}
									});


								}
							});
						}
					}
				});

			},
			//账号密码的 登录 注册
			registLogin() {
				uni.showLoading({
					title: "加载中~~"
				})
				uni.request({
					url: `${this.$api}/user/registOrLogin?qq=434714873`,
					method: 'POST',
					data: {
						username: this.username,
						password: this.password
					},
					success: res => {
						uni.hideLoading()
						// console.log(res);
						if (res.data.status === 200) {
							
							uni.setStorageSync('date', res.data.data)
							uni.showToast({
								title: res.data.msg
							})

							uni.switchTab({
								url: `/pages/my/my`
							})
						} else {
							uni.showToast({
								title: res.data.msg
							})
						}
					},
					fail: (err) => {
						uni.hideLoading()
						console.log(err);
					},
					complete: () => {}
				});
			},
		},
		mounted() {

		},
		onLoad() {

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


	.xmr {
		width: 200rpx;
		height: 200rpx;
		border-radius: 50%;
		margin-top: 200rpx;
		margin-left: 290rpx;
	}

	.img {
		width: 80rpx;
		height: 80rpx;
	}
</style>
