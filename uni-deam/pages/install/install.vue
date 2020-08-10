<template>
	<view>
		<uni-popup ref="popup" type="bottom">
			<view @click='cover(img)' class="popup-box  t-center">查看我的头像</view>
			<view @click="uploadAvatar" class="popup-box   t-center">选择上传图片</view>
		</uni-popup>
		<!-- 头像 -->
		<view class="fl box1 mg-t20 j-between">
			<view class=" mg-l20">头像</view>
			<view @click="open" class="colg fl">
				<image class="my mg-r10" :src="img" mode=""></image>
				<view class="font-w mg-r20"> > </view>
			
			</view>
		</view>
		<!-- 昵称 -->
		<view class="fl box1  j-between">
			<view class=" mg-l20">昵称</view>
			<view class="fl colg ">
				<view class="input-box t-center"> <input placeholder="输入您的昵称" class="mg-t20 " type="text" v-model="nickname" />
				</view>
				<view class="font-w mg-r20"> > </view>
			</view>
		</view>
		<!-- 生日 -->
		<view @click="showPop" class="fl box1  j-between">
			<view class=" mg-l20">生日</view>
			<min-pop size="height" :show="show" @close='close'>
				<min-picker :endTime="endTime" :startTime="startTimes" @cancel="cancel" @sure="sure">
				</min-picker>
			</min-pop>
			<view class="colg fl">
				<input disabled class="mg-t20 input-box" type="text" v-model="birthday" />
				<view class="font-w mg-r20 "> > </view>
			</view>

		</view>
		<!-- 性别 -->
		<view class="fl box1  j-between">
			<view class=" mg-l20">性别</view>
			<view class="uni-list-cell-db">

			</view>
			<view class="fl colg ">
				<picker :value="sex" :range="array" @change="getDate">
					<view class=" t-center input-box">{{sex}}</view>
				</picker>
				<view class="font-w mg-r20"> > </view>
			</view>
		</view>
		<view @click="saveChanges" class="mg-t20 saveChanges">
			<button  type="primary">保存修改</button>
		</view>
		<view @click="quitLogin" class="p-r">
			<button class="p-a" type="default">退出登录</button>
		</view>
	</view>
</template>

<script>
	import uniPopup from '../../components/uni-popup/uni-popup.vue'
	import uniPopupMessage from '../../components/uni-popup/uni-popup-message.vue'
	import uniPopupDialog from '../../components/uni-popup/uni-popup-dialog.vue'
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
				startTimes: [2010, 6, 1],
				endTime: 2046,
				array: ['男', '女', '保密'],
				index: 0,
				birthday: '1997-6-1', //生日
				nickname: '', //用户名
				userId: '', //id
				sex: '男', //性别
				show: false,
				img:'../../static/image/xmr.jpg',
				user:'',
				num:0,
			}
		},
		methods: {
			// 保存修改信息
			saveChanges(){
				uni.request({
					url: `${this.$api}/user/modifyUserinfo?qq=434714873`,
					method: 'POST',
					data: {
						birthday:this.birthday,
						nickname:this.nickname,
						sex:this.num,
						userId:this.user.id
					},
					header:{
						"headerUserId":this.user.id,
						"headerUserToken":this.user.userUniqueToken
					},
					success: res => {
						console.log(res);
						let date = res.data.data
						console.log(date);
						uni.setStorageSync('date',date)
						uni.switchTab({
							url:`/pages/my/my`
						})
					},
					fail: () => {},
					complete: () => {}
				});
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
						uni.switchTab({
							url:`/pages/my/my`
						})
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
			// 上传头像
			uploadAvatar(){
					// 上传图片
								
				uni.chooseImage({
				    success: (chooseImageRes) => {
				        const tempFilePaths = chooseImageRes.tempFilePaths;
								// 选取照片点击确定 就把选取的照片赋值给头像
								this.img =  tempFilePaths[0]
				        uni.uploadFile({
				            url: `${this.$api}/user/uploadFace?qq=40699904&userId=${this.user.id}`, //仅为示例，非真实的接口地址
				            filePath: tempFilePaths[0],
				            name: 'file',
				            formData: {
				                'user': 'test'
				            },
										header:{
											"headerUserId":this.user.id,
											"headerUserToken":this.user.userUniqueToken
										},
				            success: (uploadFileRes) => {
				                console.log(uploadFileRes.data);
												// 选取照片点击确定 就把选取的照片赋值给头像
												this.img =  uploadFileRes.data
												// 存起来
												uni.setStorageSync('img',this.img)
				            }
				        });
				    }
				});
			},
			// 底部弹出层
			open() {
				this.$refs.popup.open()
			},
			// 去封面
			cover(photo){
				uni.navigateTo({
					url:`/pages/cover/cover?photo=${photo}`
				})
				},
			
			// 生日取消事件
			cancel() {
				this.close()
			},
			// 生日确认事件
			sure(e) {
				console.log(e)
				let a = e.a
				let b = e.b
				let c = e.c
				this.birthday = `${a}-${b}-${c}`
			},
			//打开  生日 弹出曾 picker
			showPop() {
				this.show = true
			},
			// 关闭   生日   弹出层
			close() {
				this.show = false
			},
			// 选择性别
			getDate: function(e) {
				this.num = e.detail.value
				if (this.num === 0) {
					this.sex = '男'
				}
				if (this.num === 1) {
					this.sex = '女'
				}
				if (this.num === 2) {
					this.sex = '保密'
				}

			},

		},
		mounted() {

		},
		onShow() {
			if (uni.getStorageSync('date')) {
				this.user = uni.getStorageSync('date')
				this.img = this.user.faceImage  //头像
				this.nickname =this.user.nickname  //昵称
				this.birthday =this.user.birthday  //生日
				this.num = this.user.sex
				if (this.num === 0) {
					this.sex = '男'
				}
				if (this.num === 1) {
					this.sex = '女'
				}
				if (this.num === 2) {
					this.sex = '保密'
				}
				console.log(this.user);
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
	.box1 {
		width: 100%;
		height: 120rpx;
		line-height: 120rpx;
	}

	.my {
		width: 120rpx;
		height: 120rpx;
		border-radius: 50%;
	}

	.input-box {
		width: 180rpx;
	}
	.popup-box {
		background: #ffffff;
		height: 100rpx;
		line-height: 100rpx;
	}
	.p-a {
		width: 100%;
		position: absolute;
		top: 440rpx;
	}
	.saveChanges {
		margin: 50rpx auto;
		width: 250rpx;
		}
</style>
