<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-01 03:13:46
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-01 05:32:36
 * @FilePath: \showcase-frontend-uni\components\index\PopularPicksGrid.vue
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
<template>
	<view class="popular-picks-grid">
		<view class="section-header">
			<text class="section-title">热门款式推荐</text>
		</view>
		<view class="products-grid">
			<ProductCard 
				v-for="product in products" 
				:key="product.id"
				:product="product"
				@cardClick="handleProductClick"
			/>
		</view>
		
		<!-- 分页组件 -->
		<Pagination 
			:currentPage="currentPage"
			:totalPages="totalPages"
			@pageChange="handlePageChange"
		/>
	</view>
</template>

<script>
	import ProductCard from './ProductCard.vue'
	import Pagination from './Pagination.vue'
	
	export default {
		name: 'HotRecommendations',
		components: {
			ProductCard,
			Pagination
		},
		data() {
			return {
				allProducts: [
					{ id: 1, imagePlaceholder: '/static/images/001.png', title: '18K黄金项链', weight: '15.6g', price: 2580.00, tag: '经典款' },
					{ id: 2, imagePlaceholder: '/static/images/002.png', title: '黄金手镯', weight: '22.3g', price: 3680.00, tag: '镂空设计' },
					{ id: 3, imagePlaceholder: '/static/images/003.png', title: '黄金戒指', weight: '8.7g', price: 1890.00, tag: '弯绕小蛇' },
					{ id: 4, imagePlaceholder: '/static/images/004.png', title: '黄金耳环', weight: '6.2g', price: 1280.00, tag: '黄金挂饰' },
					{ id: 5, imagePlaceholder: '/static/images/001.png', title: '黄金吊坠', weight: '18.9g', price: 2980.00, tag: '限量版' },
					{ id: 6, imagePlaceholder: '/static/images/002.png', title: '黄金胸针', weight: '12.4g', price: 1580.00, tag: '复古风' },
					{ id: 7, imagePlaceholder: '/static/images/003.png', title: '黄金手链', weight: '14.8g', price: 2280.00, tag: '现代款' },
					{ id: 8, imagePlaceholder: '/static/images/004.png', title: '黄金脚链', weight: '9.1g', price: 980.00, tag: '时尚款' },
					{ id: 9, imagePlaceholder: '/static/images/001.png', title: '黄金发饰', weight: '4.5g', price: 680.00, tag: '精致款' },
					{ id: 10, imagePlaceholder: '/static/images/002.png', title: '黄金袖扣', weight: '7.3g', price: 1880.00, tag: '商务款' },
					{ id: 11, imagePlaceholder: '/static/images/003.png', title: '18K白金项链', weight: '16.2g', price: 3280.00, tag: '优雅款' },
					{ id: 12, imagePlaceholder: '/static/images/004.png', title: '钻石戒指', weight: '5.8g', price: 4580.00, tag: '奢华款' },
					{ id: 13, imagePlaceholder: '/static/images/001.png', title: '翡翠手镯', weight: '28.5g', price: 6880.00, tag: '传统款' },
					{ id: 14, imagePlaceholder: '/static/images/002.png', title: '珍珠耳环', weight: '3.2g', price: 1680.00, tag: '温婉款' },
					{ id: 15, imagePlaceholder: '/static/images/003.png', title: '银质手链', weight: '12.6g', price: 880.00, tag: '简约款' },
					{ id: 16, imagePlaceholder: '/static/images/004.png', title: '铂金吊坠', weight: '11.3g', price: 3880.00, tag: '高端款' },
					{ id: 17, imagePlaceholder: '/static/images/001.png', title: '红宝石戒指', weight: '6.9g', price: 5280.00, tag: '华贵款' },
					{ id: 18, imagePlaceholder: '/static/images/002.png', title: '蓝宝石项链', weight: '19.7g', price: 7280.00, tag: '尊贵款' },
					{ id: 19, imagePlaceholder: '/static/images/003.png', title: '祖母绿耳环', weight: '4.8g', price: 4580.00, tag: '典雅款' },
					{ id: 20, imagePlaceholder: '/static/images/004.png', title: '水晶手链', weight: '8.4g', price: 1280.00, tag: '清新款' }
				],
				currentPage: 1,
				pageSize: 8
			}
		},
		computed: {
			products() {
				const start = (this.currentPage - 1) * this.pageSize
				const end = start + this.pageSize
				return this.allProducts.slice(start, end)
			},
			totalPages() {
				return Math.ceil(this.allProducts.length / this.pageSize)
			}
		},
		methods: {
			handleProductClick(product) {
				this.$emit('productClick', product)
			},
			handlePageChange(page) {
				this.currentPage = page
			}
		}
	}
</script>

<style scoped>
	.popular-picks-grid {
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
	
	.products-grid {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		grid-template-rows: repeat(4, 1fr);
		gap: 20rpx;
	}
</style>
