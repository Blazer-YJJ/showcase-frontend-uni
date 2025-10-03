<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-01-27 10:00:00
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-02 21:09:56
 * @FilePath: \showcase-frontend-uni\pages\index\explore.vue
 * @Description: 精选探索页面 - 展示精选黄金饰品
-->
<template>
	<view class="explore-container">
		<!-- 筛选标签 -->
		<view class="filter-section">
			<scroll-view class="filter-scroll" scroll-x="true">
				<view class="filter-tags">
					<view 
						class="filter-tag" 
						:class="{ active: activeFilter === filter.key }"
						v-for="filter in filterOptions" 
						:key="filter.key"
						@click="selectFilter(filter.key)"
					>
						<text class="filter-text">{{ filter.label }}</text>
					</view>
				</view>
			</scroll-view>
		</view>

		<!-- 商品网格 -->
		<view class="products-section">
			<view class="products-grid">
				<view 
					class="product-card" 
					v-for="product in currentPageProducts" 
					:key="product.id"
					@click="viewProductDetail(product)"
				>
					<view class="product-image-container">
						<image :src="product.image" class="product-image" mode="aspectFit" />
						<view class="product-badge" v-if="product.isNew">新品</view>
						<view class="favorite-btn" @click.stop="toggleFavorite(product)">
							<text class="favorite-icon" :class="{ active: product.isFavorite }">♥</text>
						</view>
					</view>
					<view class="product-info">
						<text class="product-title">{{ product.title }}</text>
						<text class="product-category">{{ getCategoryLabel(product.category) }}</text>
						<view class="product-tags">
							<text class="product-tag" v-for="tag in product.tags" :key="tag">{{ tag }}</text>
						</view>
						<view class="product-price">
							<text class="current-price">¥{{ product.price }}</text>
							<text class="original-price" v-if="product.originalPrice">¥{{ product.originalPrice }}</text>
						</view>
					</view>
				</view>
			</view>
			
			<!-- 分页器 -->
			<Pagination 
				:currentPage="currentPage"
				:totalPages="totalPages"
				@pageChange="handlePageChange"
				v-if="totalPages > 1"
			/>
			
			<!-- 空状态 -->
			<view class="empty-state" v-if="currentPageProducts.length === 0">
				<text class="empty-text">暂无商品</text>
			</view>
		</view>
	</view>
</template>

<script>
import Pagination from '@/components/index/Pagination.vue'

export default {
	name: 'ExplorePage',
	components: {
		Pagination
	},
	data() {
		return {
			activeFilter: 'all',
			filterOptions: [
				{ key: 'all', label: '全部' },
				{ key: 'necklace', label: '项链' },
				{ key: 'ring', label: '戒指' },
				{ key: 'bracelet', label: '手镯' },
				{ key: 'earring', label: '耳环' },
				{ key: 'pendant', label: '吊坠' }
			],
			// 分页相关数据
			currentPage: 1,
			pageSize: 6,
			totalProducts: 0,
			products: [
				{
					id: 1,
					title: '经典黄金项链',
					category: 'necklace',
					price: 2580,
					originalPrice: 2980,
					image: '/static/images/001.png',
					tags: ['经典', '百搭'],
					isNew: true,
					isFavorite: false
				},
				{
					id: 2,
					title: '璀璨钻石戒指',
					category: 'ring',
					price: 3580,
					image: '/static/images/002.png',
					tags: ['钻石', '奢华'],
					isNew: false,
					isFavorite: true
				},
				{
					id: 3,
					title: '优雅黄金手镯',
					category: 'bracelet',
					price: 1880,
					image: '/static/images/003.png',
					tags: ['优雅', '简约'],
					isNew: true,
					isFavorite: false
				},
				{
					id: 4,
					title: '精致耳环套装',
					category: 'earring',
					price: 1280,
					image: '/static/images/004.png',
					tags: ['套装', '精致'],
					isNew: false,
					isFavorite: false
				},
				{
					id: 5,
					title: '心形黄金吊坠',
					category: 'pendant',
					price: 980,
					image: '/static/images/001.png',
					tags: ['心形', '浪漫'],
					isNew: true,
					isFavorite: true
				},
				{
					id: 6,
					title: '复古风格项链',
					category: 'necklace',
					price: 2280,
					image: '/static/images/002.png',
					tags: ['复古', '个性'],
					isNew: false,
					isFavorite: false
				},
				{
					id: 7,
					title: '奢华钻石手链',
					category: 'bracelet',
					price: 4580,
					originalPrice: 4980,
					image: '/static/images/003.png',
					tags: ['奢华', '钻石'],
					isNew: true,
					isFavorite: false
				},
				{
					id: 8,
					title: '珍珠耳环',
					category: 'earring',
					price: 1680,
					image: '/static/images/004.png',
					tags: ['珍珠', '优雅'],
					isNew: false,
					isFavorite: true
				},
				{
					id: 9,
					title: '翡翠吊坠',
					category: 'pendant',
					price: 3280,
					image: '/static/images/001.png',
					tags: ['翡翠', '传统'],
					isNew: false,
					isFavorite: false
				},
				{
					id: 10,
					title: '玫瑰金戒指',
					category: 'ring',
					price: 2880,
					image: '/static/images/002.png',
					tags: ['玫瑰金', '时尚'],
					isNew: true,
					isFavorite: false
				},
				{
					id: 11,
					title: '银质项链',
					category: 'necklace',
					price: 1280,
					image: '/static/images/003.png',
					tags: ['银质', '简约'],
					isNew: false,
					isFavorite: true
				},
				{
					id: 12,
					title: '水晶手镯',
					category: 'bracelet',
					price: 1980,
					image: '/static/images/004.png',
					tags: ['水晶', '清新'],
					isNew: true,
					isFavorite: false
				}
			]
		}
	},
	computed: {
		// 根据筛选条件过滤商品
		filteredProducts() {
			let filtered = this.products
			if (this.activeFilter !== 'all') {
				filtered = this.products.filter(product => product.category === this.activeFilter)
			}
			this.totalProducts = filtered.length
			return filtered
		},
		// 当前页显示的商品
		currentPageProducts() {
			const start = (this.currentPage - 1) * this.pageSize
			const end = start + this.pageSize
			return this.filteredProducts.slice(start, end)
		},
		// 总页数
		totalPages() {
			return Math.ceil(this.totalProducts / this.pageSize)
		}
	},
	methods: {
		// 选择筛选条件
		selectFilter(filterKey) {
			this.activeFilter = filterKey
			this.currentPage = 1 // 重置到第一页
		},
		
		// 获取分类标签
		getCategoryLabel(categoryKey) {
			const category = this.filterOptions.find(item => item.key === categoryKey)
			return category ? category.label : categoryKey
		},
		
		// 查看商品详情
		viewProductDetail(product) {
			console.log('查看商品详情:', product)
			uni.showToast({
				title: `查看${product.title}`,
				icon: 'none'
			})
		},
		
		// 切换收藏状态
		toggleFavorite(product) {
			product.isFavorite = !product.isFavorite
			uni.showToast({
				title: product.isFavorite ? '已收藏' : '已取消收藏',
				icon: 'none'
			})
		},
		
		// 分页变化处理
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
		console.log('精选探索页面加载')
	}
}
</script>

<style scoped>
/* 页面容器 */
.explore-container {
	min-height: 100vh;
	background: linear-gradient(135deg, #f8f9fa 0%, #ffffff 100%);
	padding-bottom: 40rpx;
}

/* 筛选区域 */
.filter-section {
	background: #ffffff;
	padding: 30rpx 0;
	margin-bottom: 20rpx;
	box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.05);
}

.filter-scroll {
	white-space: nowrap;
}

.filter-tags {
	display: flex;
	padding: 0 40rpx;
}

.filter-tag {
	padding: 16rpx 32rpx;
	margin-right: 20rpx;
	background: #f8f9fa;
	border-radius: 40rpx;
	border: 2rpx solid transparent;
	transition: all 0.3s ease;
}

.filter-tag.active {
	background: linear-gradient(135deg, #d4af37 0%, #f4d03f 100%);
	border-color: #d4af37;
}

.filter-text {
	font-size: 28rpx;
	color: #495057;
	transition: color 0.3s ease;
}

.filter-tag.active .filter-text {
	color: #ffffff;
	font-weight: bold;
}

/* 商品区域 */
.products-section {
	padding: 0 20rpx;
}

.products-grid {
	display: grid;
	grid-template-columns: repeat(2, 1fr);
	gap: 20rpx;
	margin-bottom: 40rpx;
}

.product-card {
	background: #ffffff;
	border-radius: 16rpx;
	overflow: hidden;
	box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.08);
	transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.product-card:active {
	transform: translateY(-4rpx);
	box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.12);
}

.product-image-container {
	position: relative;
	height: 300rpx;
	overflow: hidden;
}

.product-image {
	width: 100%;
	height: 100%;
}

.product-badge {
	position: absolute;
	top: 16rpx;
	left: 16rpx;
	background: linear-gradient(135deg, #d4af37 0%, #f4d03f 100%);
	color: #ffffff;
	font-size: 20rpx;
	padding: 8rpx 16rpx;
	border-radius: 20rpx;
	font-weight: bold;
}

.favorite-btn {
	position: absolute;
	top: 16rpx;
	right: 16rpx;
	width: 60rpx;
	height: 60rpx;
	background: rgba(255, 255, 255, 0.9);
	border-radius: 50%;
	display: flex;
	align-items: center;
	justify-content: center;
	backdrop-filter: blur(10rpx);
}

.favorite-icon {
	font-size: 32rpx;
	color: #dee2e6;
	transition: color 0.3s ease;
}

.favorite-icon.active {
	color: #d4af37;
}

.product-info {
	padding: 24rpx;
}

.product-title {
	display: block;
	font-size: 28rpx;
	font-weight: bold;
	color: #212529;
	margin-bottom: 8rpx;
	line-height: 1.4;
}

.product-category {
	display: block;
	font-size: 24rpx;
	color: #6c757d;
	margin-bottom: 12rpx;
}

.product-tags {
	display: flex;
	flex-wrap: wrap;
	margin-bottom: 16rpx;
}

.product-tag {
	font-size: 20rpx;
	color: #d4af37;
	background: rgba(212, 175, 55, 0.1);
	padding: 4rpx 12rpx;
	border-radius: 12rpx;
	margin-right: 8rpx;
	margin-bottom: 8rpx;
}

.product-price {
	display: flex;
	align-items: center;
}

.current-price {
	font-size: 32rpx;
	font-weight: bold;
	color: #d4af37;
	margin-right: 12rpx;
}

.original-price {
	font-size: 24rpx;
	color: #6c757d;
	text-decoration: line-through;
}

/* 空状态 */
.empty-state {
	text-align: center;
	padding: 80rpx 40rpx;
	background: #ffffff;
	border-radius: 16rpx;
	box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.05);
}

.empty-text {
	font-size: 28rpx;
	color: #6c757d;
}
</style>