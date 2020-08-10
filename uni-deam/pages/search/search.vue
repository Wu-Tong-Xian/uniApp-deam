<template>
	<view>
		<view class="uni-form-item uni-column fl">
			<view class="title fl">
				<image class="search" src="../../static/image/search.png" mode=""></image>
				<input class="uni-input" v-model="value" focus @confirm="changevalue" placeholder="请输入电影信息" />
			</view>
			<view class="search1" @click='search'>搜索 </view>
		</view>
		<view v-if="obtainList.length >=1 ">
			<view class="fl fl-wrap">
				<view v-for="(item) in obtainList" :key='item.id' @click="goDetail(item)" class="mg-lr10 box">
					<image class="item-img" :src="item.poster" mode=""></image>
					<view class="fz14"> {{item.name}}</view>
				</view>
			</view>
		</view>
		<view v-else class="t-center">
			<view class=""> 您的搜索不存在 </view>
			<view class=""> 请重新搜索 </view>
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
				value: '',
				page: 1,
				pageSize: 10,
				obtainList: [],
				length:10
			}
		},
		methods: {
			search() {
				this.changevalue()
			},
			changevalue() {
				console.log(this.value);
				uni.showLoading({
					title: '加载中...',
				})
				uni.request({
					url: `${this.$api}/search/list?qq=434714873&keywords=${this.value}&page=${this.page}&pageSize=${this.pageSize}`,
					method: 'POST',
					data: {},
					success: res => {
						uni.hideLoading()
						this.obtainList = res.data.data.rows
						// this.length =this.obtainList.length
						console.log(this.obtainList);
					},
					fail: (err) => {
						uni.hideLoading()
						console.log(err);
					},
					complete: () => {}
				});
			},
			// 上拉刷新
			onPullDownRefresh() {
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 500);

			},
			// 下拉触底 在调用猜你喜欢的方法
			onReachBottom(e) {
					this.pageSize	= (this.pageSize * 1 + this.length * 1)
					this.changevalue()
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
			this.changevalue()
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
	.title {
		width: 600rpx;
		height: 50rpx;
		border-radius: 30rpx;
		border: 1px solid #2C405A;
		margin: 20rpx;
	}

	.search {
		width: 40rpx;
		height: 40rpx;
		margin: 5rpx 20rpx 0 10rpx;
	}

	.search1 {
		margin-top: 18rpx;
	}

	.item-img {
		width: 210rpx;
		height: 300rpx;
		margin-top: 10rpx;
	}

	.box {
		width: 210rpx;
		height: 400rpx;
	}
</style>
