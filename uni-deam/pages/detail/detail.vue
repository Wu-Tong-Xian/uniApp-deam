<template>
	<view>
		<view class="uni-padding-wrap uni-common-mt">
			<view>
				<video id="myVideo" class="video" :src="item.trailer" :poster='item.poster' @error="videoErrorCallback" :danmu-list="danmuList"
				 enable-danmu danmu-btn controls></video>
			</view>
			<!-- #ifndef MP-ALIPAY -->
			<view class="uni-list uni-common-mt">
				<view class="uni-list-cell">
					<view class="fl">
						<view class="uni-label fz14">请输入您要吐槽的话：</view>
						<view class="uni-list-cell-db">
							<input v-model="danmuValue" class="uni-input" type="text" placeholder="在此处输入弹幕内容" />
						</view>
					</view>

				</view>
			</view>
			<view class="uni-btn-v">
				<button @click="sendDanmu" class="page-body-button">发送弹幕</button>
			</view>
			<!-- #endif -->
		</view>
		<view class="fl mg-tb10">
			<image @click="cover(item.poster)" class="trailer-img" :src="item.poster" mode=""></image>
			<view class=" mg-l20 ">
				<view class="font-w mg-t10">{{item.name}} </view>
				<view class="colg fz14">{{item.basicInfo}} </view>
				<view class="colg fz14">{{item.originalName}} </view>
				<view class="colg fz14">{{item.releaseDate}} </view>
				<view class="zhpf j-around fl">
					<view class="">
						<view class="">综合评分:</view>
						<view class="t-center col6">{{item.score}}</view>
					</view>
					<view class="">
						<uni-rate class="mg-t10 " allow-half :size="16" :is-fill="false" v-model="value" @change="onChange" />
						<view class="t-center fz14">{{item.prisedCounts}}人点赞</view>
					</view>
				</view>
			</view>
		</view>
		<view class="mg-t20">
			<view class="mg-lr20 mg-tb20 bor-b"> </view>
			<view class="fz14 mg-l20 mg-t10 colg">剧情介绍: </view>
			<view  class="mg-lr20 mg-b20 fz16">{{item.plotDesc}} </view>
			<view class="mg20 bor-b"> </view>
			<view class="fz14 mg-l20 mg-t10 colg">演职人员: </view>

			<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120">
				<view class="fl">
					<view v-for="(item) in Actors1" :key='item.id' class="">
						<!-- 导演 -->
						<view class="Actors-box fl-nowrap">
							<image @click="cover(item.photo)" class="img mg-lr10" :src="item.photo" mode=""></image>
							<view class="colb t-none t-center fz18">{{item.name}} </view>
							<view class="colg t-none t-center fz18">{{item.actName}} </view>
						</view>
					</view>
					<!-- 演员 -->
					<view v-for="(item) in Actors2" :key='item.id' class="">
						<view class="Actors-box fl-nowrap">
							<image @click="cover(item.photo)" class="img mg-lr10" :src="item.photo" mode=""></image>
							<view class="colb t-none t-center fz18">{{item.name}} </view>
							<view class="colg t-none t-center fz18">饰演{{item.actName}} </view>
						</view>
					</view>
				</view>
			</scroll-view>
			<view class="mg-lr20 mg-tb20 bor-b"> </view>
			<view class="fz14 mg-l20 mg-t10 colg">剧照: </view>
			<view class="fl fl-wrap">
				<view v-for='(item,index) in arr' :key='index' class=" fl  mg-tb20 ">
					<image @click="cover(item)" class="arr-img mg-lr10 fl-wrap" :src="item" mode=""></image>
				</view>
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
				arr:[],
				Actors1: [],
				Actors2: [],
				value: 3.5,
				item: {},
				src: '',
				danmuList: [{
						text: '第 1s 出现的弹幕',
						color: '#ff0000',
						time: 1
					},
					{
						text: '第 3s 出现的弹幕',
						color: '#ff00ff',
						time: 3
					}
				],
				danmuValue: ''

			}
		},
		onReady: function(res) {
			// #ifndef MP-ALIPAY
			this.videoContext = uni.createVideoContext('myVideo')
			// #endif
		},
		methods: {
			// 去封面
			cover(photo){
				uni.navigateTo({
					url:`/pages/cover/cover?photo=${photo}`
				})
				},
			sendDanmu() {
				this.videoContext.sendDanmu({
					text: this.danmuValue,
					color: this.getRandomColor()
				});
				this.danmuValue = '';
			},
			videoErrorCallback(e) {
				uni.showModal({
					content: e.target.errMsg,
					showCancel: false
				})
			},
			getRandomColor() {
				const rgb = []
				for (let i = 0; i < 3; ++i) {
					let color = Math.floor(Math.random() * 256).toString(16)
					color = color.length == 1 ? '0' + color : color
					rgb.push(color)
				}
				return '#' + rgb.join('')
			},
			onChange(e) {
				console.log('rate发生改变:' + JSON.stringify(e))
			},
			// 导演 
			theActors1() {
				uni.showLoading({
					title: "加载中。。。"
				})
				uni.request({
					url: `${this.$api}/search/staff/${this.item.id}/1?qq=${434714873}`,
					method: 'POST',
					data: {},
					success: res => {
						wx.hideLoading()
						this.Actors1 = res.data.data
						console.log(res);
					},
					fail: (err) => {
						wx.hideLoading()
					},
					complete: () => {}
				});
			},
			// 职员
			theActors2() {
				uni.showLoading({
					title: "加载中。。。"
				})
				uni.request({
					url: `${this.$api}/search/staff/${this.item.id}/2?qq=${434714873}`,
					method: 'POST',
					data: {},
					success: res => {
						wx.hideLoading()
						this.Actors2 = res.data.data
						console.log(res);
					},
					fail: (err) => {
						wx.hideLoading()
					},
					complete: () => {}
				});
			},
		},
		mounted() {

		},
		onNavigationBarButtonTap(e) {
				console.log(e);
		},
		onLoad(options) {
			if(options.item){
				this.item = JSON.parse(options.item)
			}
			if(this.item.plotPics){
				this.arr =JSON.parse(this.item.plotPics)
			}
			
			this.theActors1()
			this.theActors2()
			console.log(this.item);
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
	.video {
		width: 100%;
	}

	.trailer-img {
		width: 250rpx;
		height: 300rpx;
		margin-left: 15rpx;
	}

	.zhpf {
		width: 400rpx;
		height: 100rpx;
		background: #E6E6E6;
		margin-top: 10rpx;
	}

	.img {
		width: 170rpx;
		height: 220rpx;
	}

	.Actors-box {
		width: 170rpx;
		height: 320rpx;
		margin: 25rpx 15rpx 20rpx 15rpx;
	}
	.arr-img {
		width: 210rpx;
		height: 230rpx;
	}
</style>
