<template>
	<view class="content">
		<text class="title">会员中心</text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '会员中心'
			}
		},
		onLoad() {
			console.log('会员中心页加载')
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
