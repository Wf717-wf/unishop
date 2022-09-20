<template>
	<view class="my-search-container" @click="searchBoxHandler()">
		<view class="my-search-box"  >
			<uni-icons type="search" size="17"></uni-icons>
			<text class="placeholder">搜索</text>
		</view>
	</view>
</template>
<script>
	export default {
		name:"my-search",
		data() {
			return {
				
			};
		},
		methods:{
			searchBoxHandler(){
				this.$emit('click')
			}
		}
	}
</script>

<style lang="scss">
	.my-search-container{
		display: flex;
		justify-content: center;
		align-items: center;
		padding: 0 10px;
		width: 100%;
		height:55px ;
		background-color: aqua;
		.my-search-box{
			width:80%;
			height:36px;
			position:sticky;
			top:0;
			z-index:999;
			background-color: #ffffff;
			border-radius: 15px;
			border: 5px solid #ffffff;
			display: flex;
			justify-content: center;
			align-items: center;
			.placeholder{
				font-size: 15px;
				margin-left: 5px;
			}
		}
		
	}
	 
</style>