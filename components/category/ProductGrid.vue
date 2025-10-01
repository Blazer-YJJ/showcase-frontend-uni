<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-01 11:30:13
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-01 14:42:07
 * @FilePath: \showcase-frontend-uni\components\category\ProductGrid.vue
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
<template>
	<view class="product-grid">
		<!-- 商品网格容器 -->
		<view class="grid-container">
			<view 
				v-for="(product, index) in displayProducts" 
				:key="product.id || index"
				class="grid-item"
				@click="handleProductClick(product)"
			>
				<ProductCard 
					:product="product"
					@cardClick="handleCardClick"
				/>
			</view>
		</view>
		
		<!-- 空状态 -->
		<view v-if="products.length === 0" class="empty-state">
			<image 
				src="/static/images/001.png" 
				class="empty-image"
				mode="aspectFit"
			/>
			<text class="empty-text">暂无商品</text>
			<text class="empty-desc">请尝试其他分类或搜索关键词</text>
		</view>
		
		<!-- 加载状态 -->
		<view v-if="loading" class="loading-state">
			<view class="loading-spinner"></view>
			<text class="loading-text">加载中...</text>
		</view>
	</view>
</template>

<script>
	import ProductCard from '../index/ProductCard.vue'
	
	export default {
		name: 'ProductGrid',
		components: {
			ProductCard
		},
		props: {
			products: {
				type: Array,
				default: () => []
			},
			loading: {
				type: Boolean,
				default: false
			},
			columns: {
				type: Number,
				default: 2
			},
			maxRows: {
				type: Number,
				default: 3
			}
		},
		computed: {
			// 计算每行显示的商品数量
			itemsPerRow() {
				return this.columns
			},
			// 计算最大显示的商品数量（限制行数）
			maxDisplayProducts() {
				return this.maxRows * this.columns
			},
			// 获取要显示的商品列表
			displayProducts() {
				// 如果没有设置maxRows，显示所有商品
				if (!this.maxRows || this.maxRows <= 0) {
					return this.products
				}
				// 否则按maxRows限制显示
				return this.products.slice(0, this.maxDisplayProducts)
			}
		},
		methods: {
			// 处理商品点击
			handleProductClick(product) {
				this.$emit('productClick', product)
			},
			
			// 处理卡片点击（从ProductCard组件传递）
			handleCardClick(product) {
				this.$emit('productClick', product)
			}
		}
	}
</script>

<style scoped>
	.product-grid {
		flex: 1;
		background-color: #f8f9fa;
		overflow-y: auto;
		overflow-x: hidden;
		display: flex;
		flex-direction: column;
	}
	
	/* 网格容器 */
	.grid-container {
		display: flex;
		flex-wrap: wrap;
		padding: 15rpx;
		gap: 12rpx;
		justify-content: space-between;
		align-content: flex-start;
		flex: 1;
		overflow-y: auto;
	}
	
	/* 网格项 */
	.grid-item {
		width: calc(50% - 8rpx);
		margin-bottom: 10rpx;
		transition: transform 0.3s ease;
        height: 320rpx;
	}
	
	.grid-item:active {
		transform: scale(0.95);
	}
	
	/* 空状态样式 */
	.empty-state {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 100rpx 40rpx;
		text-align: center;
	}
	
	.empty-image {
		width: 200rpx;
		height: 200rpx;
		opacity: 0.6;
		margin-bottom: 30rpx;
	}
	
	.empty-text {
		font-size: 32rpx;
		color: #666;
		font-weight: 500;
		margin-bottom: 15rpx;
	}
	
	.empty-desc {
		font-size: 26rpx;
		color: #999;
		line-height: 1.5;
	}
	
	/* 加载状态样式 */
	.loading-state {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 100rpx 40rpx;
	}
	
	.loading-spinner {
		width: 60rpx;
		height: 60rpx;
		border: 4rpx solid #f3f3f3;
		border-top: 4rpx solid #d4af37;
		border-radius: 50%;
		animation: spin 1s linear infinite;
		margin-bottom: 20rpx;
	}
	
	@keyframes spin {
		0% { transform: rotate(0deg); }
		100% { transform: rotate(360deg); }
	}
	
	.loading-text {
		font-size: 26rpx;
		color: #666;
	}
	
	/* 响应式布局 */
	@media screen and (max-width: 750rpx) {
		.grid-container {
			padding: 12rpx;
			gap: 10rpx;
		}
		.grid-item {
			width: calc(50% - 5rpx);
			margin-bottom: 10rpx;
		}
	}
	
	@media screen and (min-width: 751rpx) {
		.grid-container {
			padding: 18rpx;
			gap: 15rpx;
		}
		.grid-item {
			width: calc(33.333% - 10rpx);
			margin-bottom: 15rpx;
		}
	}
	
	/* 单列布局 */
	.grid-container.single-column .grid-item {
		width: 100%;
	}
	
	/* 三列布局 */
	.grid-container.three-column .grid-item {
		width: calc(33.333% - 8rpx);
	}
	
	/* 四列布局 */
	.grid-container.four-column .grid-item {
		width: calc(25% - 9rpx);
	}
</style>
