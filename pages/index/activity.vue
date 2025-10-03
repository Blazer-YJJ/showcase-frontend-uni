<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-01-27 10:00:00
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-03 11:29:52
 * @FilePath: \showcase-frontend-uni\pages\index\activity.vue
 * @Description: 限时活动页面 - 展示限时优惠的黄金饰品活动
-->
<template>
	<view class="activity-container">
		<!-- 活动信息卡片 -->
		<view class="activity-info-section">
			<view class="activity-info-card">
				<view class="info-header">
					<text class="info-title">{{ activityInfo.title }}</text>
					<text class="info-subtitle">{{ activityInfo.subtitle }}</text>
				</view>
				<view class="info-content">
					<text class="info-description">{{ activityInfo.description }}</text>
				</view>
				<view class="info-time">
					<view class="time-item">
						<text class="time-label">开始时间：</text>
						<text class="time-value">{{ activityInfo.startTime }}</text>
					</view>
					<view class="time-item">
						<text class="time-label">结束时间：</text>
						<text class="time-value">{{ activityInfo.endTime }}</text>
					</view>
				</view>
			</view>
		</view>

		<!-- 活动分类 -->
		<view class="activity-tabs">
			<scroll-view class="tabs-scroll" scroll-x="true">
				<view class="tabs-container">
					<view 
						class="activity-tab" 
						:class="{ active: activeTab === tab.key }"
						v-for="tab in activityTabs" 
						:key="tab.key"
						@click="switchTab(tab.key)"
					>
						<text class="tab-text">{{ tab.label }}</text>
					</view>
				</view>
			</scroll-view>
		</view>

		<!-- 活动商品列表 -->
		<view class="activity-products">
			<view 
				class="product-card" 
				v-for="item in currentActivityProducts" 
				:key="item.id"
				@click="viewProductDetail(item)"
			>
				<!-- 商品图片区域 -->
				<view class="product-image-section">
					<image :src="item.image" class="product-image" mode="aspectFit" />
					<!-- 活动标签 -->
					<view class="activity-badge">
						<text class="badge-text">{{ item.activityType }}</text>
					</view>
					<!-- 折扣标签 -->
					<view class="discount-badge">
						<text class="discount-text">{{ item.discount }}折</text>
					</view>
				</view>

				<!-- 商品信息区域 -->
				<view class="product-content">
					<!-- 商品标题和描述 -->
					<view class="product-header">
						<text class="product-title">{{ item.title }}</text>
						<text class="product-description">{{ item.description }}</text>
					</view>

					<!-- 价格信息 -->
					<view class="price-info">
						<view class="price-main">
							<text class="current-price">¥{{ item.currentPrice }}</text>
							<text class="original-price">¥{{ item.originalPrice }}</text>
						</view>
						<view class="save-info">
							<text class="save-icon">💰</text>
							<text class="save-amount">省 ¥{{ item.saveAmount }}</text>
						</view>
					</view>

					<!-- 商品状态信息 -->
					<view class="product-status">
						<view class="status-tags">
							<view class="activity-tag">
								<text class="tag-text">{{ item.activityTag }}</text>
							</view>
							<view class="stock-tag">
								<text class="stock-text">仅剩{{ item.stock }}件</text>
							</view>
						</view>
					</view>

					<!-- 销售进度 -->
					<view class="sales-progress">
						<view class="progress-info">
							<text class="progress-text">已售{{ item.salesProgress }}%</text>
						</view>
						<view class="progress-bar">
							<view class="progress-fill" :style="{ width: item.salesProgress + '%' }"></view>
						</view>
					</view>

					<!-- 操作按钮 -->
					<view class="product-actions">
						<view class="buy-btn" @click.stop="addToCart(item)">
							<text class="btn-text">立即抢购</text>
						</view>
					</view>
				</view>
			</view>
		</view>


		<!-- 空状态 -->
		<view class="empty-state" v-if="currentActivityProducts.length === 0">
			<text class="empty-text">暂无活动商品</text>
			<text class="empty-subtitle">请稍后再试</text>
		</view>
	</view>
</template>

<script>
export default {
	name: 'ActivityPage',
	data() {
		return {
			activeTab: 'flash',
			activityTabs: [
				{ key: 'flash', label: '限时抢购' },
				{ key: 'discount', label: '折扣优惠' },
				{ key: 'bundle', label: '组合套装' },
				{ key: 'newcomer', label: '新人专享' }
			],
			activityInfo: {
				title: '金色年华限时特惠活动',
				subtitle: '精选黄金饰品，品质保证，限时优惠',
				description: '活动期间，所有黄金饰品享受8折优惠，新用户专享6折超低价格。精选18K黄金打造的项链、戒指、手镯等精品首饰，让您享受高贵典雅的奢华体验。部分商品支持分期付款，轻松拥有心仪首饰。',
				startTime: '2025年1月27日 10:00',
				endTime: '2025年2月10日 23:59'
			},
			activityProducts: {
				flash: [
					{
						id: 1,
						title: '经典黄金项链',
						description: '18K黄金打造，限时特价',
						originalPrice: 2980,
						currentPrice: 2384,
						saveAmount: 596,
						discount: 8.0,
						image: '/static/images/001.png',
						activityType: '限时抢购',
						activityTag: '爆款',
						stock: 15,
						salesProgress: 75
					},
					{
						id: 2,
						title: '璀璨钻石戒指',
						description: '精选钻石，奢华设计',
						originalPrice: 3580,
						currentPrice: 2864,
						saveAmount: 716,
						discount: 8.0,
						image: '/static/images/002.png',
						activityType: '限时抢购',
						activityTag: '热销',
						stock: 8,
						salesProgress: 85
					}
				],
				discount: [
					{
						id: 3,
						title: '优雅黄金手镯',
						description: '简约优雅，适合日常佩戴',
						originalPrice: 1880,
						currentPrice: 1504,
						saveAmount: 376,
						discount: 8.0,
						image: '/static/images/003.png',
						activityType: '折扣优惠',
						activityTag: '推荐',
						stock: 25,
						salesProgress: 60
					},
					{
						id: 4,
						title: '精致耳环套装',
						description: '套装设计，多种搭配选择',
						originalPrice: 1280,
						currentPrice: 1024,
						saveAmount: 256,
						discount: 8.0,
						image: '/static/images/004.png',
						activityType: '折扣优惠',
						activityTag: '新品',
						stock: 30,
						salesProgress: 45
					}
				],
				bundle: [
					{
						id: 5,
						title: '黄金首饰套装',
						description: '项链+戒指+耳环，超值组合',
						originalPrice: 4500,
						currentPrice: 3600,
						saveAmount: 900,
						discount: 8.0,
						image: '/static/images/002.png',
						activityType: '组合套装',
						activityTag: '套装',
						stock: 12,
						salesProgress: 70
					}
				],
				newcomer: [
					{
						id: 6,
						title: '新人专享吊坠',
						description: '专为新用户打造，超低价格',
						originalPrice: 980,
						currentPrice: 588,
						saveAmount: 392,
						discount: 6.0,
						image: '/static/images/001.png',
						activityType: '新人专享',
						activityTag: '专享',
						stock: 50,
						salesProgress: 30
					}
				]
			},
		}
	},
	computed: {
		currentActivityProducts() {
			return this.activityProducts[this.activeTab] || []
		}
	},
	methods: {
		// 切换活动标签
		switchTab(tabKey) {
			this.activeTab = tabKey
		},
		
		// 查看商品详情
		viewProductDetail(product) {
			console.log('查看商品详情:', product)
			uni.showToast({
				title: `查看${product.title}`,
				icon: 'none'
			})
		},
		
		// 添加到购物车
		addToCart(product) {
			console.log('添加到购物车:', product)
			uni.showToast({
				title: '已添加到购物车',
				icon: 'success'
			})
		},
		
	},
	
	onLoad() {
		console.log('限时活动页面加载')
	}
}
</script>

<style scoped>
/* 页面容器 */
.activity-container {
	min-height: 100vh;
	background: linear-gradient(135deg, #f8f9fa 0%, #ffffff 100%);
	padding-bottom: 40rpx;
	box-sizing: border-box;
}

/* 活动信息区域 */
.activity-info-section {
	padding: 30rpx 20rpx;
}

.activity-info-card {
	background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
	border-radius: 20rpx;
	padding: 40rpx 30rpx;
	box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.1);
	margin-bottom: 20rpx;
	border: 1rpx solid #e0e0e0;
}

.info-header {
	text-align: center;
	margin-bottom: 30rpx;
}

.info-title {
	color: #333333;
	font-size: 36rpx;
	font-weight: bold;
	margin-bottom: 12rpx;
	display: block;
}

.info-subtitle {
	color: #666666;
	font-size: 28rpx;
	font-weight: 500;
	display: block;
}

.info-content {
	margin-bottom: 30rpx;
	padding: 20rpx;
	background: rgba(255, 255, 255, 0.8);
	border-radius: 12rpx;
	border: 1rpx solid #d0d0d0;
}

.info-description {
	color: #555555;
	font-size: 26rpx;
	line-height: 1.6;
}

.info-time {
	display: flex;
	flex-direction: column;
	gap: 16rpx;
}

.time-item {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 16rpx 20rpx;
	background: rgba(255, 255, 255, 0.6);
	border-radius: 12rpx;
	border: 1rpx solid #c0c0c0;
}

.time-label {
	color: #666666;
	font-size: 26rpx;
	font-weight: 500;
}

.time-value {
	color: #333333;
	font-size: 26rpx;
	font-weight: bold;
}

/* 活动标签 */
.activity-tabs {
	background: #ffffff;
	padding: 30rpx 0;
}

.tabs-scroll {
	white-space: nowrap;
}

.tabs-container {
	display: flex;
	padding: 0 40rpx;
}

.activity-tab {
	padding: 20rpx 40rpx;
	margin-right: 20rpx;
	background: #f8f9fa;
	border-radius: 40rpx;
	border: 2rpx solid transparent;
	transition: all 0.3s ease;
}

.activity-tab.active {
	background: #d4af37;
	border-color: #d4af37;
}

.tab-text {
	font-size: 28rpx;
	color: #495057;
	transition: color 0.3s ease;
}

.activity-tab.active .tab-text {
	color: #ffffff;
	font-weight: bold;
}

/* 活动商品列表 */
.activity-products {
	padding: 20rpx;
	box-sizing: border-box;
}

/* 商品卡片 */
.product-card {
	display: flex;
	background: #ffffff;
	border-radius: 20rpx;
	margin-bottom: 24rpx;
	box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.08);
	overflow: hidden;
	transition: all 0.3s ease;
}

.product-card:active {
	transform: translateY(-4rpx);
	box-shadow: 0 8rpx 30rpx rgba(0, 0, 0, 0.15);
}

/* 商品图片区域 */
.product-image-section {
	position: relative;
	width: 240rpx;
	height: 240rpx;
	flex-shrink: 0;
}

.product-image {
	width: 100%;
	height: 100%;
	background: #f8f9fa;
}

.activity-badge {
	position: absolute;
	top: 12rpx;
	left: 12rpx;
	background: #e74c3c;
	padding: 6rpx 12rpx;
	border-radius: 16rpx;
	box-shadow: 0 2rpx 8rpx rgba(231, 76, 60, 0.3);
	display: flex;
	align-items: center;
	justify-content: center;
}

.badge-text {
	font-size: 20rpx;
	font-weight: bold;
	color: #ffffff;
}

.discount-badge {
	position: absolute;
	bottom: 12rpx;
	right: 12rpx;
	background: linear-gradient(135deg, #d4af37 0%, #f4d03f 100%);
	padding: 6rpx 10rpx;
	border-radius: 12rpx;
	box-shadow: 0 2rpx 8rpx rgba(212, 175, 55, 0.3);
	display: flex;
	align-items: center;
	justify-content: center;
}

.discount-text {
	font-size: 20rpx;
	font-weight: bold;
	color: #ffffff;
}

/* 商品内容区域 */
.product-content {
	flex: 1;
	padding: 24rpx;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

/* 商品标题区域 */
.product-header {
	margin-bottom: 16rpx;
	display: flex;
	flex-direction: column;
	justify-content: center;
}

.product-title {
	display: block;
	font-size: 32rpx;
	font-weight: bold;
	color: #2c3e50;
	margin-bottom: 8rpx;
	line-height: 1.4;
}

.product-description {
	display: block;
	font-size: 24rpx;
	color: #7f8c8d;
	line-height: 1.3;
}

/* 价格信息 */
.price-info {
	margin-bottom: 16rpx;
	display: flex;
	flex-direction: column;
	justify-content: center;
}

.price-main {
	display: flex;
	align-items: baseline;
	margin-bottom: 8rpx;
}

.current-price {
	font-size: 40rpx;
	font-weight: bold;
	color: #e74c3c;
	margin-right: 16rpx;
}

.original-price {
	font-size: 24rpx;
	color: #bdc3c7;
	text-decoration: line-through;
}

.save-info {
	display: flex;
	align-items: center;
}

.save-icon {
	font-size: 20rpx;
	margin-right: 4rpx;
}

.save-amount {
	font-size: 22rpx;
	color: #d4af37;
	font-weight: bold;
}

/* 商品状态 */
.product-status {
	margin-bottom: 16rpx;
	display: flex;
	flex-direction: column;
	justify-content: center;
}

.status-tags {
	display: flex;
	gap: 12rpx;
}

.activity-tag {
	background: linear-gradient(135deg, #d4af37 0%, #f4d03f 100%);
	padding: 6rpx 12rpx;
	border-radius: 12rpx;
	box-shadow: 0 2rpx 6rpx rgba(212, 175, 55, 0.2);
	display: flex;
	align-items: center;
	justify-content: center;
}

.tag-text {
	font-size: 20rpx;
	font-weight: bold;
	color: #ffffff;
}

.stock-tag {
	background: #ecf0f1;
	padding: 6rpx 12rpx;
	border-radius: 12rpx;
	display: flex;
	align-items: center;
	justify-content: center;
}

.stock-text {
	font-size: 20rpx;
	color: #7f8c8d;
	font-weight: 500;
}

/* 销售进度 */
.sales-progress {
	margin-bottom: 20rpx;
	display: flex;
	flex-direction: column;
	justify-content: center;
}

.progress-info {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 8rpx;
}

.progress-text {
	font-size: 20rpx;
	color: #7f8c8d;
}

.progress-bar {
	height: 6rpx;
	background: transparent;
	border-radius: 3rpx;
	overflow: hidden;
}

.progress-fill {
	height: 100%;
	background: linear-gradient(90deg, #d4af37 0%, #f4d03f 100%);
	border-radius: 3rpx;
	transition: width 0.3s ease;
}

/* 操作按钮 */
.product-actions {
	display: flex;
	justify-content: flex-end;
	align-items: center;
}

.buy-btn {
	background: linear-gradient(135deg, #e74c3c 0%, #c0392b 100%);
	padding: 16rpx 32rpx;
	border-radius: 24rpx;
	box-shadow: 0 4rpx 12rpx rgba(231, 76, 60, 0.3);
	transition: all 0.3s ease;
	display: flex;
	align-items: center;
	justify-content: center;
}

.buy-btn:active {
	transform: translateY(-2rpx);
	box-shadow: 0 6rpx 16rpx rgba(231, 76, 60, 0.4);
}

.btn-text {
	font-size: 24rpx;
	font-weight: bold;
	color: #ffffff;
}

/* 响应式设计 */
@media screen and (max-width: 750rpx) {
	.product-image-section {
		width: 200rpx;
		height: 200rpx;
	}
	
	.product-content {
		padding: 20rpx;
	}
	
	.current-price {
		font-size: 36rpx;
	}
	
	.product-title {
		font-size: 28rpx;
	}
}

@media screen and (max-width: 600rpx) {
	.activity-products {
		padding: 16rpx;
	}
	
	.product-card {
		margin-bottom: 20rpx;
	}
	
	.product-image-section {
		width: 180rpx;
		height: 180rpx;
	}
	
	.product-content {
		padding: 16rpx;
	}
	
	.current-price {
		font-size: 32rpx;
	}
	
	.product-title {
		font-size: 26rpx;
	}
	
	.buy-btn {
		padding: 12rpx 24rpx;
	}
	
	.btn-text {
		font-size: 22rpx;
	}
}


/* 空状态 */
.empty-state {
	text-align: center;
	padding: 120rpx 40rpx;
}

.empty-text {
	display: block;
	font-size: 32rpx;
	color: #495057;
	margin-bottom: 16rpx;
}

.empty-subtitle {
	display: block;
	font-size: 24rpx;
	color: #999;
}
</style>
