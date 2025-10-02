<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-02 
 * @Description: 交易管理组件 - 包含我的订单、我的售后、我的收藏功能入口
-->
<template>
	<view class="transaction-management">
		<view class="section-header">
			<text class="section-title">交易管理</text>
		</view>
		
		<view class="management-grid">
			<view 
				class="management-item" 
				v-for="(item, index) in managementItems" 
				:key="index"
				@click="handleItemClick(item)"
			>
				<view class="item-icon-container">
					<image :src="item.icon" class="item-icon" mode="aspectFit" />
					<!-- 数量角标 -->
					<view v-if="item.badge && item.badge > 0" class="item-badge">
						<text class="badge-text">{{ item.badge > 99 ? '99+' : item.badge }}</text>
					</view>
				</view>
				<text class="item-label">{{ item.label }}</text>
				<text class="item-desc">{{ item.description }}</text>
				
				<!-- 右箭头 -->
				<view class="item-arrow">
					<text class="arrow-icon">›</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'TransactionManagement',
		props: {
			// 订单统计数据
			orderStats: {
				type: Object,
				default: () => ({
					pendingOrders: 0,
					afterSales: 0,
					favorites: 0
				})
			}
		},
		data() {
			return {
				managementItems: [
					{
						icon: '/static/icons/dingdan.png',
						label: '我的订单',
						description: '查看订单状态',
						action: 'orders',
						badge: 0
					},
					{
						icon: '/static/icons/shouhou.png',
						label: '我的售后',
						description: '退换货服务',
						action: 'afterSales',
						badge: 0
					},
					{
						icon: '/static/icons/shoucang.png',
						label: '我的收藏',
						description: '收藏的商品',
						action: 'favorites',
						badge: 0
					}
				]
			}
		},
		watch: {
			// 监听订单统计数据变化，更新角标
			orderStats: {
				handler(newStats) {
					this.updateBadges(newStats)
				},
				immediate: true,
				deep: true
			}
		},
		methods: {
			// 更新角标数量
			updateBadges(stats) {
				this.managementItems[0].badge = stats.pendingOrders || 0
				this.managementItems[1].badge = stats.afterSales || 0
				this.managementItems[2].badge = stats.favorites || 0
			},
			
			// 处理项目点击事件
			handleItemClick(item) {
				this.$emit('itemClick', item)
				
				// 根据不同的操作类型进行页面跳转
				switch(item.action) {
					case 'orders':
						uni.navigateTo({
							url: '/pages/order/orderList'
						})
						break
					case 'afterSales':
						uni.navigateTo({
							url: '/pages/afterSales/afterSalesList'
						})
						break
					case 'favorites':
						uni.navigateTo({
							url: '/pages/favorites/favoritesList'
						})
						break
				}
			}
		}
	}
</script>

<style scoped>
	.transaction-management {
		margin: 20rpx;
		background-color: #fff;
		border-radius: 12rpx;
		padding: 30rpx;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.1);
	}
	
	.section-header {
		margin-bottom: 30rpx;
	}
	
	.section-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #333;
	}
	
	.management-grid {
		display: flex;
		flex-wrap: wrap;
		gap: 20rpx;
		justify-content: space-between;
	}
	
	.management-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 30rpx 20rpx;
		background-color: #fff;
		border-radius: 12rpx;
		border: 1rpx solid #f0f0f0;
		position: relative;
		transition: all 0.3s ease;
		width: 24%;
		box-shadow: 0 2rpx 4rpx rgba(0,0,0,0.05);
	}
	
	.management-item:active {
		background-color: #f8f9fa;
		transform: translateY(-2rpx);
		box-shadow: 0 4rpx 8rpx rgba(0,0,0,0.1);
	}
	
	.item-icon-container {
		position: relative;
		width: 80rpx;
		height: 80rpx;
		background-color: #f8f9fa;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-bottom: 20rpx;
	}
	
	.item-icon {
		width: 40rpx;
		height: 40rpx;
	}
	
	.item-badge {
		position: absolute;
		top: -8rpx;
		right: -8rpx;
		background: linear-gradient(135deg, #ff6b6b, #ee5a52);
		border-radius: 20rpx;
		min-width: 32rpx;
		height: 32rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 2rpx solid #fff;
	}
	
	.badge-text {
		font-size: 20rpx;
		color: #fff;
		font-weight: bold;
		line-height: 1;
	}
	
	.item-label {
		font-size: 28rpx;
		color: #333;
		font-weight: 600;
		margin-bottom: 8rpx;
		text-align: center;
	}
	
	.item-desc {
		font-size: 22rpx;
		color: #999;
		text-align: center;
		line-height: 1.4;
	}
	
	.item-arrow {
		display: none;
	}
	
	/* 不同项目的图标容器颜色 */
	.management-item:nth-child(1) .item-icon-container {
		background: linear-gradient(135deg, #fff9e6, #f8f9fa);
		border: 1rpx solid #d4af37;
	}
	
	.management-item:nth-child(2) .item-icon-container {
		background: linear-gradient(135deg, #fdf2f2, #f8f9fa);
		border: 1rpx solid #e74c3c;
	}
	
	.management-item:nth-child(3) .item-icon-container {
		background: linear-gradient(135deg, #f0f8ff, #f8f9fa);
		border: 1rpx solid #3498db;
	}
</style>
