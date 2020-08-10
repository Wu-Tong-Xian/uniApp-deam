<template>
	<view>
		<!-- 轮播图 -->
		<swiper1 :swiper1="swiper1"></swiper1>
		<!-- 热门超英 热门预告 -->
		<hot :hot1="hot1" :hot2="hot2"></hot>
		<!-- 猜你喜欢 -->
		<guessULike :guessLike='guessLike'></guessULike>
	</view>
</template>

<script>
	import swiper1 from "../../components/index/swiper1.vue"
	import hot from "../../components/index/hot.vue"
	import guessULike from "../../components/index/guessULike.vue"
	export default {
		name: "",
		components: {
			swiper1,
			hot,
			guessULike,
		},
		props: {},
		data() {
			return {
				swiper1: [],
				hot1: [],
				hot2: [],
				guessLike: [],

			}
		},
		methods: {
			// 轮播图
			banner() {
				uni.showLoading({
					title: '加载中',
					icon: 'none'
				})
				uni.request({
					url: `${this.$api}/index/carousel/list?qq=434714873`,
					method: 'POST',
					data: {},
					success: res => {
						uni.hideLoading()
						this.swiper1 = res.data.data
						console.log(this.swiper1);
					},
					fail: (err) => {
						uni.hideLoading()
						console.log(err);
					},
					complete: () => {}
				});
			},
			// 热门超英
			hotSuperhero() {
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=434714873&type=superhero`,
					method: 'POST',
					data: {},
					success: res => {
						// console.log(res);
						this.hot1 = res.data.data
						console.log(this.hot1);
					},
					fail: (err) => {
						console.log(err);
					},
					complete: () => {}
				});
			},
			// 热门预告
			hotTrailer() {
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=434714873&type=trailer`,
					method: 'POST',
					data: {},
					success: res => {
						// console.log(res);
						this.hot2 = res.data.data
						console.log(this.hot2);
					},
					fail: (err) => {
						console.log(err);
					},
					complete: () => {}
				});
			},
			// 猜你喜欢
			guessULike() {
				uni.showLoading({
					title: '加载中',
					icon: 'none'
				})
				uni.request({
					url: `${this.$api}/index/guessULike?qq=434714873`,
					method: 'POST',
					data: {},
					success: res => {
						uni.hideLoading()
						this.guessLike = res.data.data,
							console.log(this.guessLike);
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
			// 下拉触底 在调用猜你喜欢的方法
			onReachBottom(e) {
				this.guessULike()
			},
			// 点击右上角分享按钮
			
	},
	mounted() {

		},
		onLoad(options) {
			this.banner() //轮播图
			this.hotSuperhero() //热门超英
			this.hotTrailer() //热门预告
			this.guessULike() //猜你喜欢
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

</style>
