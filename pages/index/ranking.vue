<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-01-27 10:00:00
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-03 05:54:21
 * @FilePath: \showcase-frontend-uni\pages\index\ranking.vue
 * @Description: 排行榜页面 - 商品卡片形式展示
-->
<template>
	<view class="ranking-container">
		<!-- 商品卡片网格 -->
		<view class="product-grid">
			<view 
				class="product-card" 
				v-for="item in currentPageProducts" 
				:key="item.id"
				@click="viewProductDetail(item)"
			>
				<view class="product-image-container">
					<image :src="item.image" class="product-image" mode="aspectFit" />
				</view>
				<view class="product-info">
					<text class="product-name">{{ item.title }}</text>
					<text class="product-category">{{ item.category }}</text>
				</view>
			</view>
		</view>

		<!-- 空状态 -->
		<view class="empty-state" v-if="allProducts.length === 0">
			<text class="empty-text">暂无商品数据</text>
		</view>

		<!-- 固定在底部的分页器 -->
		<view class="fixed-pagination" v-if="totalPages > 1">
			<Pagination 
				:currentPage="currentPage"
				:totalPages="totalPages"
				@pageChange="goToPage"
			/>
		</view>
	</view>
</template>

<script>
import Pagination from '@/components/index/Pagination.vue'

export default {
	name: 'RankingPage',
	components: {
		Pagination
	},
	data() {
		return {
			currentPage: 1,
			pageSize: 16, // 每页16个商品
			allProducts: [
				{
					id: 1,
					title: '经典黄金项链',
					category: '项链',
					image: '/static/images/001.png'
				},
				{
					id: 2,
					title: '璀璨钻石戒指',
					category: '戒指',
					image: '/static/images/002.png'
				},
				{
					id: 3,
					title: '优雅黄金手镯',
					category: '手镯',
					image: '/static/images/003.png'
				},
				{
					id: 4,
					title: '精致耳环套装',
					category: '耳环',
					image: '/static/images/004.png'
				},
				{
					id: 5,
					title: '心形黄金吊坠',
					category: '吊坠',
					image: '/static/images/001.png'
				},
				{
					id: 6,
					title: '复古风格项链',
					category: '项链',
					image: '/static/images/002.png'
				},
				{
					id: 7,
					title: '现代简约戒指',
					category: '戒指',
					image: '/static/images/003.png'
				},
				{
					id: 8,
					title: '时尚手链套装',
					category: '手镯',
					image: '/static/images/004.png'
				},
				{
					id: 9,
					title: '优雅耳钉',
					category: '耳环',
					image: '/static/images/001.png'
				},
				{
					id: 10,
					title: '简约吊坠',
					category: '吊坠',
					image: '/static/images/002.png'
				},
				{
					id: 11,
					title: '奢华钻石项链',
					category: '项链',
					image: '/static/images/003.png'
				},
				{
					id: 12,
					title: '经典黄金戒指',
					category: '戒指',
					image: '/static/images/004.png'
				},
				{
					id: 13,
					title: '时尚黄金手镯',
					category: '手镯',
					image: '/static/images/001.png'
				},
				{
					id: 14,
					title: '精美耳环',
					category: '耳环',
					image: '/static/images/002.png'
				},
				{
					id: 15,
					title: '浪漫心形吊坠',
					category: '吊坠',
					image: '/static/images/003.png'
				},
				{
					id: 16,
					title: '复古黄金项链',
					category: '项链',
					image: '/static/images/004.png'
				},
				{
					id: 17,
					title: '现代钻石戒指',
					category: '戒指',
					image: '/static/images/001.png'
				},
				{
					id: 18,
					title: '优雅手链',
					category: '手镯',
					image: '/static/images/002.png'
				},
				{
					id: 19,
					title: '精致耳钉套装',
					category: '耳环',
					image: '/static/images/003.png'
				},
				{
					id: 20,
					title: '简约黄金吊坠',
					category: '吊坠',
					image: '/static/images/004.png'
				},
				{
					id: 21,
					title: '奢华黄金项链',
					category: '项链',
					image: '/static/images/001.png'
				},
				{
					id: 22,
					title: '经典钻石戒指',
					category: '戒指',
					image: '/static/images/002.png'
				},
				{
					id: 23,
					title: '时尚黄金手镯',
					category: '手镯',
					image: '/static/images/003.png'
				},
				{
					id: 24,
					title: '精美耳环套装',
					category: '耳环',
					image: '/static/images/004.png'
				}
			]
		}
	},
	computed: {
		// 总页数
		totalPages() {
			return Math.ceil(this.allProducts.length / this.pageSize)
		},
		// 当前页的商品
		currentPageProducts() {
			const start = (this.currentPage - 1) * this.pageSize
			const end = start + this.pageSize
			return this.allProducts.slice(start, end)
		}
	},
	methods: {
		// 跳转到指定页面
		goToPage(page) {
			if (page >= 1 && page <= this.totalPages) {
				this.currentPage = page
			}
		},
		
		// 查看商品详情
		viewProductDetail(product) {
			console.log('查看商品详情:', product)
			uni.showToast({
				title: `查看${product.title}`,
				icon: 'none'
			})
		}
	},
	
	onLoad() {
		console.log('排行榜页面加载')
	}
}
</script>

<style scoped>
/* 页面容器 */
.ranking-container {
	min-height: 100vh;
	background: linear-gradient(135deg, #f8f9fa 0%, #ffffff 100%);
	padding: 20rpx;
	padding-bottom: 120rpx; /* 为固定分页器预留空间 */
}

/* 商品网格 */
.product-grid {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
	gap: 20rpx;
}

/* 商品卡片 */
.product-card {
	width: calc(45% - 10rpx);
	display: flex;
	background: #ffffff;
	border-radius: 16rpx;
	padding: 16rpx;
	box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.08);
	transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.product-card:active {
	transform: translateY(-2rpx);
	box-shadow: 0 8rpx 20rpx rgba(0, 0, 0, 0.12);
}

/* 商品图片容器 */
.product-image-container {
	width: 120rpx;
	height: 120rpx;
	border-radius: 12rpx;
	overflow: hidden;
	margin-right: 16rpx;
	flex-shrink: 0;
}

.product-image {
	width: 100%;
	height: 100%;
}

/* 商品信息 */
.product-info {
	flex: 1;
	display: flex;
	flex-direction: column;
	justify-content: center;
	gap: 6rpx;
}

.product-name {
	font-size: 26rpx;
	font-weight: bold;
	color: #212529;
	line-height: 1.3;
	overflow: hidden;
	text-overflow: ellipsis;
	display: -webkit-box;
	-webkit-line-clamp: 2;
	line-clamp: 2;
	-webkit-box-orient: vertical;
}

.product-category {
	font-size: 22rpx;
	color: #6c757d;
	line-height: 1.2;
}


/* 空状态 */
.empty-state {
	text-align: center;
	padding: 80rpx 40rpx;
	background: #ffffff;
	border-radius: 16rpx;
	margin: 40rpx 0;
	box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.05);
}

.empty-text {
	font-size: 28rpx;
	color: #6c757d;
}

/* 固定在底部的分页器 */
.fixed-pagination {
	position: fixed;
	bottom: 0;
	left: 0;
	right: 0;
	z-index: 1000;
	background: #ffffff;
	border-top: 1rpx solid #eee;
	box-shadow: 0 -2rpx 10rpx rgba(0, 0, 0, 0.1);
}
</style>