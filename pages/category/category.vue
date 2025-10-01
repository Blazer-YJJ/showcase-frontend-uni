<template>
	<view class="content">
		<text class="title">分类</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '分类'
			}
		},
		onLoad() {
			console.log('分类页加载')
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
