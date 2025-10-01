<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-01 03:12:33
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-01 03:43:34
 * @FilePath: \showcase-frontend-uni\components\index\QuickAccessGrid.vue
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
<template>
	<view class="quick-access-grid">
		<view 
			class="access-item" 
			v-for="(item, index) in accessItems" 
			:key="index"
			@click="handleItemClick(item)"
		>
			<view class="access-icon">
				<image :src="item.icon" class="icon-image" mode="aspectFit" />
			</view>
			<text class="access-label">{{ item.label }}</text>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'QuickEntry',
		data() {
			return {
				accessItems: [
					{ icon: '/static/icons/tansuo.png', label: '精选探索', action: 'explore' },
					{ icon: '/static/icons/rili.png', label: '热销款式', action: 'ranking' },
					{ icon: '/static/icons/shandian.png', label: '限时活动', action: 'activity' },
					{ icon: '/static/icons/shoucang.png', label: '我的收藏', action: 'favorites' }
				]
			}
		},
		methods: {
			handleItemClick(item) {
				this.$emit('itemClick', item)
			}
		}
	}
</script>

<style scoped>
	.quick-access-grid {
		display: flex;
		justify-content: space-around;
		padding: 30rpx 20rpx;
		background-color: #fff;
		margin: 20rpx;
		border-radius: 12rpx;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.1);
	}
	
	.access-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		flex: 1;
	}
	
	.access-icon {
		width: 80rpx;
		height: 80rpx;
		background-color: #f8f9fa;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 15rpx;
	}
	
	.icon-image {
		width: 40rpx;
		height: 40rpx;
	}
	
	.access-label {
		font-size: 24rpx;
		color: #666;
		text-align: center;
	}
</style>
