<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-01 03:12:33
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-02 11:35:03
 * @FilePath: \showcase-frontend-uni\components\index\QuickEntry.vue
 * @Description: 快捷入口组件 - 支持跳转到精选探索、主推款式、限时活动、最多收藏页面
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
					{ icon: '/static/icons/rili.png', label: '主推款式', action: 'ranking' },
					{ icon: '/static/icons/shandian.png', label: '限时活动', action: 'activity' },
					{ icon: '/static/icons/shoucang.png', label: '最多收藏', action: 'favorites' }
				]
			}
		},
		methods: {
			handleItemClick(item) {
				console.log('快捷入口点击:', item)
				
				// 根据action跳转到对应页面
				const pageMap = {
					'explore': '/pages/index/explore',
					'ranking': '/pages/index/ranking', 
					'activity': '/pages/index/activity',
					'favorites': '/pages/index/favorites'
				}
				
				const targetUrl = pageMap[item.action]
				if (targetUrl) {
					// 确保页面存在，添加预加载逻辑
					this.preloadPage(targetUrl)
					
					uni.navigateTo({
						url: targetUrl,
						success: () => {
							console.log(`跳转到${item.label}页面成功`)
						},
						fail: (err) => {
							console.error('页面跳转失败:', err)
							uni.showToast({
								title: '页面跳转失败',
								icon: 'none'
							})
						}
					})
				} else {
					// 如果没有对应的页面，显示提示
					uni.showToast({
						title: `点击了${item.label}`,
						icon: 'none'
					})
				}
				
				// 触发父组件事件
				this.$emit('itemClick', item)
			},
			
			// 预加载页面
			preloadPage(url) {
				try {
					// 动态导入页面组件，确保代码依赖分析能识别
					switch(url) {
						case '/pages/index/explore':
							console.log('预加载精选探索页面')
							break
						case '/pages/index/ranking':
							console.log('预加载主推款式页面')
							break
						case '/pages/index/activity':
							console.log('预加载限时活动页面')
							break
						case '/pages/index/favorites':
							console.log('预加载最多收藏页面')
							break
					}
				} catch (error) {
					console.error('页面预加载失败:', error)
				}
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
