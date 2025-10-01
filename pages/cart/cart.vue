<template>
	<view class="content">
		<text class="title">购物车</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '购物车'
			}
		},
		onLoad() {
			console.log('购物车页加载')
		},
		methods: {
			
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100vh;
	}

	.title {
		font-size: 36rpx;
		color: #333;
	}
</style>
