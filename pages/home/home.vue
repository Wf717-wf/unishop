<template>
  <view>
    <!-- 轮播图区域 -->
    <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
    	<swiper-item v-for="(item,i) in swiperList" :key="i">
    		<navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
    			<image :src="item.image_src" mode=""></image>
    		</navigator>
    	</swiper-item>
    </swiper>
	<!-- 分类导航区域 -->
	<view class="nav-list">
		  <view class="nav-item" v-for="(item,i) in navList" :key="i" @click="navClickHandler(item)">
			  <image :src="item.image_src" class="nav-img"></image>
		  </view>
	</view>
	<view class="floor-list">
		<view class="floor-item" v-for="(item,i) in floorList" :key="i">
			<image class="floor-title" :src="item.floor_title.image_src" mode=""></image>
			<view class="floor-img-box">
			   <!-- 左侧大图片的盒子 -->
			   <navigator class="left-img-box" :url="item.product_list[0].url">
			     <image :src="item.product_list[0].image_src" :style="{width: item.product_list[0].image_width + 'rpx'}" mode="widthFix"></image>
			   </navigator>
			   <!-- 右侧 4 个小图片的盒子 -->
			   <view class="right-img-box">
			     <navigator class="right-img-item" v-for="(item2, i2) in item.product_list" :key="i2" v-if="i2!==0":url="item2.url">
			       <image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}"></image>
			     </navigator>
			   </view>
			 </view>
		</view>
	</view>
  </view>
  
</template>
<script>
	export default {
		data() {
			return {
				//轮播图数据
				swiperList:[],
				//分类导航的数据列表
				navList: [],
				//楼层的数据列表
				floorList: [],
			};
		},	
		onLoad(){
			this.getSwiperList(),
			this.getNavList(),
			this.getFloorList()
		},
		methods:{
			/* 获取轮播图数据 */
		     async getSwiperList() {
			  // 3.1 发起请求
			  const { data: res } = await uni.$http.get('/api/public/v1/home/swiperdata')
			  // 3.2 请求失败
			  if (res.meta.status !== 200) {
				return  uni.$showMsg()
			  }
			  // 3.3 请求成功，为 data 中的数据赋值
			  this.swiperList = res.message
			  console.log(res)
			},
			/* 获取分类导航数据 */
			async getNavList() {
				  const { data: res } = await uni.$http.get('/api/public/v1/home/catitems')
				  if (res.meta.status !== 200) return uni.$showMsg()
				  this.navList = res.message
				  console.log(res)
			},
			navClickHandler(item) {
			  // 判断点击的是哪个 nav
			  if (item.name === '分类') {
			    uni.switchTab({
			      url: '/pages/cate/cate'
			    })
			  }
			},
			async getFloorList() {
			  const { data: res } = await uni.$http.get('/api/public/v1/home/floordata')
			  if (res.meta.status !== 200) return uni.$showMsg()
			  res.message.forEach(floor => {
			      floor.product_list.forEach(prod => {
			        prod.url = '/subpkg/goods_list/goods_list?' + prod.navigator_url.split('?')[1]
			      })
			    })
			  
			    this.floorList = res.message
			  console.log(res)
		}
	},
}
</script>
<style lang="scss">
swiper {
 height: 330rpx;
 .swiper-item,
 image {
   width: 100%;
   height: 100%;
 }
}
.nav-list{
	display: flex;
	justify-content: space-around;
	margin: 15px 0;
	.nav-img{
		width: 128rpx;
		height: 140rpx;
	}
}
.floor-title {
  width: 100%;
  height: 60rpx;
 
}
.right-img-box {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
.floor-img-box {
  display: flex;
  padding-left: 10rpx;
}
</style>
