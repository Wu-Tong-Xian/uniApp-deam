<template>
	<view>
		<view class="fl mg-tb20 mg-l10">
			<image class="rm mg-r10" src="../../static/image/rm.png" mode=""></image>
			<view class=""> 热门超英 </view>
		</view>
		<scroll-view class="scroll-view_H" scroll-x="true" scroll-left="120">
			<view class="fl box">
				<view v-for="(item) in hot1" :key='item.id'>
					<view class=" hot1-box">
						<image  @click="goDetail(item)" class="hot1-img " :src="item.cover" mode=""></image>
						<view class="t-none t-center fz12"> {{item.name}} </view>
						<view class="fl fz12">
							<uni-rate class="rate" :readonly="true" :is-fill="false" :size="12" :value="item.score/2" />
							<view> {{item.score}} </view>
						</view>
					</view>
				</view>
			</view>
		</scroll-view>

		<view class="fl mg-tb20 mg-l10">
			<image class="rm mg-r10" src="../../static/image/yg.png" mode=""></image>
			<view class=""> 热门预告 </view>
		</view>
		<view class="fl trailer-box fl-wrap">
			<view v-for="(item) in hot2 " :key='item.id'>
				<view class="">
					<video @play='play(item.id)'  show-fullscreen-btn=true :poster="item.cover" :id="item.id" class="trailer "
					 vslide-gesture-in-fullscreen=true :src="item.trailer" @error="videoErrorCallback" :danmu-list="danmuList"
					 enable-danmu controls></video>
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
		props: {
			hot1: {
				type: Array
			},
			hot2: {
				type: Array
			}
		},
		data() {
			return {
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
			}
		},
		methods: {
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?item=${JSON.stringify(item)}`
				})
			},
			videoErrorCallback: function(e) {
				uni.showModal({
					content: e.target.errMsg,
					showCancel: false
				})
			},
			
			// 播放视频触发
			play(id) {
				console.log(this.hot2);
				
				this.videoContext = uni.createVideoContext(id)
				for (let i = 0; i < this.hot2.length; i++) {
					let tempid = this.hot2[i].id
					if (tempid != id) {
						uni.createVideoContext(tempid).pause()
					}
				}
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
	.rm {
		width: 50rpx;
		height: 50rpx;
	}

	.hot1-img {
		width: 164rpx;
		height: 164rpx;
	}

	.hot1-box {
		width: 164rpx;
		height: 300rpx;
		margin-left: 20rpx;
		margin-right: 20rpx;
	}

	.rate {
		margin-top: 5rpx;
	}

	.scroll-view_H {
		height: 250rpx;
	}

	.trailer-box {
		width: 100%;
		height: 800rpx;
	}

	.trailer {
		width: 345rpx;
		height: 345rpx;
		margin: 0 15rpx 0 15rpx;
	}
</style>
