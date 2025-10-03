<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-01-27 10:00:00
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-03 06:37:14
 * @FilePath: \showcase-frontend-uni\pages\index\favorites.vue
 * @Description: 收藏页面 - 展示商品卡片和收藏次数
-->
<template>
	<view class="favorites-container">
		<!-- 商品卡片列表 -->
		<view class="product-list">
			<view 
				class="product-card" 
				v-for="item in productList" 
				:key="item.id"
				@click="viewProductDetail(item)"
			>
				<view class="product-image-container">
					<image :src="item.image" class="product-image" mode="aspectFit" />
					<view class="favorite-count">
						<text class="count-text">{{ item.favoriteCount }}</text>
					</view>
				</view>
				<view class="product-info">
					<text class="product-title">{{ item.title }}</text>
					<text class="product-description">{{ item.description }}</text>
					<view class="product-price-section">
						<text class="product-price">¥{{ item.price }}</text>
					</view>
				</view>
			</view>
		</view>

		<!-- 空状态 -->
		<view class="empty-state" v-if="productList.length === 0">
			<text class="empty-text">暂无商品数据</text>
		</view>

		<!-- 分页器 - 固定在底部 -->
		<view class="pagination-container">
			<Pagination 
				:currentPage="currentPage"
				:totalPages="totalPages"
				@pageChange="handlePageChange"
			/>
		</view>
	</view>
</template>

<script>
import Pagination from '@/components/index/Pagination.vue'

export default {
	name: 'FavoritesPage',
	components: {
		Pagination
	},
	data() {
		return {
			// 分页相关数据
			currentPage: 1,
			pageSize: 6, // 每页显示6个商品
			allProducts: [
				{
					id: 1,
					title: '经典黄金项链',
					description: '18K黄金打造，经典设计，百搭款式',
					price: 2580,
					image: '/static/images/001.png',
					favoriteCount: 1256
				},
				{
					id: 2,
					title: '璀璨钻石戒指',
					description: '精选钻石，奢华设计，完美切割',
					price: 3580,
					image: '/static/images/002.png',
					favoriteCount: 1205
				},
				{
					id: 3,
					title: '优雅黄金手镯',
					description: '简约优雅，适合日常佩戴',
					price: 1880,
					image: '/static/images/003.png',
					favoriteCount: 987
				},
				{
					id: 4,
					title: '精致耳环套装',
					description: '套装设计，多种搭配选择',
					price: 1280,
					image: '/static/images/004.png',
					favoriteCount: 856
				},
				{
					id: 5,
					title: '心形黄金吊坠',
					description: '浪漫心形设计，表达爱意',
					price: 980,
					image: '/static/images/001.png',
					favoriteCount: 789
				},
				{
					id: 6,
					title: '复古风格项链',
					description: '复古设计，个性十足',
					price: 2280,
					image: '/static/images/002.png',
					favoriteCount: 634
				},
				{
					id: 7,
					title: '现代简约戒指',
					description: '现代设计理念，简约而不简单',
					price: 1680,
					image: '/static/images/003.png',
					favoriteCount: 523
				},
				{
					id: 8,
					title: '时尚手链',
					description: '时尚潮流，年轻化设计',
					price: 1280,
					image: '/static/images/004.png',
					favoriteCount: 456
				},
				{
					id: 9,
					title: '奢华钻石耳环',
					description: '奢华钻石，闪耀动人',
					price: 4580,
					image: '/static/images/001.png',
					favoriteCount: 389
				},
				{
					id: 10,
					title: '经典珍珠项链',
					description: '天然珍珠，温润如玉',
					price: 2880,
					image: '/static/images/002.png',
					favoriteCount: 312
				},
				{
					id: 11,
					title: '时尚黄金手链',
					description: '时尚设计，精致工艺',
					price: 1580,
					image: '/static/images/003.png',
					favoriteCount: 267
				},
				{
					id: 12,
					title: '精美吊坠',
					description: '精美工艺，独特设计',
					price: 1180,
					image: '/static/images/004.png',
					favoriteCount: 198
				}
			]
		}
	},
	computed: {
		// 当前页显示的商品列表
		productList() {
			const start = (this.currentPage - 1) * this.pageSize
			const end = start + this.pageSize
			return this.allProducts.slice(start, end)
		},
		// 总页数
		totalPages() {
			return Math.ceil(this.allProducts.length / this.pageSize)
		}
	},
	methods: {
		// 查看商品详情
		viewProductDetail(product) {
			console.log('查看商品详情:', product)
			uni.showToast({
				title: `查看${product.title}`,
				icon: 'none'
			})
		},
		// 处理分页变化
		handlePageChange(page) {
			this.currentPage = page
			// 滚动到顶部
			uni.pageScrollTo({
				scrollTop: 0,
				duration: 300
			})
		}
	},
	
	onLoad() {
		console.log('收藏页面加载')
	}
}
</script>

<style scoped>
/* 页面容器 */
.favorites-container {
	min-height: 100vh;
	background: linear-gradient(135deg, #f8f9fa 0%, #ffffff 100%);
	padding: 40rpx 20rpx 150rpx 20rpx; /* 底部留出分页器空间 */
	position: relative;
}

/* 商品列表 */
.product-list {
	display: flex;
	flex-direction: column;
	gap: 20rpx;
}

/* 商品卡片 */
.product-card {
	display: flex;
	align-items: center;
	background: #ffffff;
	border-radius: 16rpx;
	padding: 30rpx;
	box-shadow: 0 4rpx 12rpx rgba(0,0,0,0.08);
	transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.product-card:active {
	transform: translateY(-2rpx);
	box-shadow: 0 8rpx 20rpx rgba(0,0,0,0.15);
}

/* 商品图片容器 */
.product-image-container {
	position: relative;
	width: 160rpx;
	height: 160rpx;
	margin-right: 30rpx;
	border-radius: 12rpx;
	overflow: hidden;
}

.product-image {
	width: 100%;
	height: 100%;
	background: #f8f9fa;
}

/* 收藏次数显示 */
.favorite-count {
	position: absolute;
	bottom: 8rpx;
	right: 8rpx;
	background: rgba(212, 175, 55, 0.9);
	color: #ffffff;
	padding: 6rpx 12rpx;
	border-radius: 12rpx;
}

.count-text {
	font-size: 20rpx;
	font-weight: bold;
}

/* 商品信息 */
.product-info {
	flex: 1;
}

.product-title {
	display: block;
	font-size: 32rpx;
	font-weight: bold;
	color: #495057;
	margin-bottom: 8rpx;
	line-height: 1.4;
}

.product-description {
	display: block;
	font-size: 24rpx;
	color: #999;
	margin-bottom: 16rpx;
	line-height: 1.3;
}

.product-price-section {
	display: flex;
	align-items: center;
}

.product-price {
	font-size: 32rpx;
	font-weight: bold;
	color: #d4af37;
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
}

/* 分页器容器 - 固定在底部 */
.pagination-container {
	position: fixed;
	bottom: 0;
	left: 0;
	right: 0;
	background: #ffffff;
	border-top: 1rpx solid #f0f0f0;
	box-shadow: 0 -2rpx 8rpx rgba(0,0,0,0.1);
	z-index: 1000;
	min-height: 100rpx; /* 确保有足够的高度 */
}
</style>
