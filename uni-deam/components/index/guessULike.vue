<template>
	<view class="">
		<view v-for="(item) in guessLike" :key='item.id' class=" fl box ">
			<image @click="goDetail(item)" class="item-img" :src="item.cover" mode=""></image>
			<view class="center">
				<view class=""> {{item.name}} </view>
				<uni-rate class="rate colg" :readonly="true" :is-fill="false" :size="12" :value="item.score/2" />
				<view class="fz14 colg">{{item.basicInfo}} </view>
				<view class="fz14 colg">{{item.releaseDate}} </view>
			</view>
			<view class="fl right">
				<image class="xx-img" src="../../static/image/xx.png" mode=""></image>
				<view @click="click" class="">
					<image class="rm-img" src="../../static/image/praise.png" mode=""></image>
					<view class="fz14 zyx ">赞一赞</view>
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
			guessLike: {
				type: Array
			}
		},
		data() {
			return {}
		},
		methods: {
			click(){
				uni.showToast({
					title:'已赞',
					icon:'none'
				})
			},
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?item=${JSON.stringify(item)}`
				})
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
	.box {}

	.item-img {
		width: 200rpx;
		height: 280rpx;
		margin: 0 15rpx 40rpx 15rpx;

	}

	.center {
		width: 350rpx;
		height: 280rpx;
		margin-bottom: 40rpx;
	}

	.rate {
		margin: 10rpx 0 10rpx 0;
	}

	.xx-img {
		margin: 0 15rpx 0 15rpx;
		width: 20rpx;
		height: 280rpx;
	}

	.right {
		width: 140rpx;
		height: 280rpx;
		margin-bottom: 40rpx;
	}

	.rm-img {
		width: 45rpx;
		height: 45rpx;
		margin-top: 120rpx;
		margin-left: 20rpx;
	}

	.zyx {
		color: #FAD982;
	}
</style>
