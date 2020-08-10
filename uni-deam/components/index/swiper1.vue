<template>
    <view>
        <!-- 轮播图 -->
        <swiper class="swiper1" :circular='true' :indicator-dots="true" indicator-color="rgba(191, 163, 79, .3)"
         indicator-active-color="#E80000" :autoplay="true" :interval="3000" :duration="300">
        	<swiper-item v-for="(item) in swiper1 " :key='item.id'>
        		<image @click="cover(item.image)" class="img" :src="item.image" mode=""></image>
        	</swiper-item>
        </swiper>
        
    </view>
</template>

<script>
export default {
name: "",
components: {

},
props: {
	swiper1:{
		type:Array
	}
},
data () {
  return {
    }
  },
  methods: {
// 去封面
			cover(photo){
				uni.navigateTo({
					url:`/pages/cover/cover?photo=${photo}`
				})
				},
  },
  mounted () {

  },
  onLoad () {
	console.log(swiper1);
	console.log(111);
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
.swiper1 {
		width: 100%;
		height: 450rpx;
	}
	.img {
		width: 100%;
		height: 450rpx;
	}
	
</style>
