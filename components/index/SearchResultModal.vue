<template>
	<view class="search-modal" v-if="visible" @click="handleMaskClick">
		<view class="modal-content" @click.stop>
			<!-- 弹窗头部 -->
			<view class="modal-header">
				<text class="modal-title">搜索结果</text>
				<view class="close-btn" @click="handleClose">
					<image class="close-icon" src="/static/icons/shanchu.png" mode="aspectFit"></image>
				</view>
			</view>
			
			<!-- 搜索关键词显示 -->
			<view class="search-keyword">
				<text class="keyword-text">"{{ searchKeyword }}"</text>
				<text class="result-count">找到 {{ searchResults.length }} 个结果</text>
			</view>
			
			<!-- 搜索结果列表 -->
			<scroll-view class="result-list" scroll-y="true">
				<view class="result-item" v-for="product in searchResults" :key="product.id" @click="handleProductClick(product)">
					<view class="product-image-container">
						<image 
							:src="product.image" 
							class="product-image" 
							mode="aspectFit"
							@error="handleImageError"
						/>
					</view>
					<view class="product-info">
						<text class="product-name">{{ product.name }}</text>
						<text class="product-category">{{ product.category }}</text>
						<text class="product-description">{{ product.description }}</text>
						<view class="product-price-container">
							<text class="product-price">¥{{ product.price }}</text>
							<view class="product-tag" v-if="product.tag">
								<text class="tag-text">{{ product.tag }}</text>
							</view>
						</view>
					</view>
				</view>
				
				<!-- 无结果提示 -->
				<view class="no-result" v-if="searchResults.length === 0">
					<image class="no-result-icon" src="/static/icons/sousuo.png" mode="aspectFit"></image>
					<text class="no-result-text">未找到相关商品</text>
					<text class="no-result-tip">请尝试其他关键词</text>
				</view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'SearchResultModal',
		props: {
			visible: {
				type: Boolean,
				default: false
			},
			searchKeyword: {
				type: String,
				default: ''
			},
			searchResults: {
				type: Array,
				default: () => []
			}
		},
		methods: {
			// 关闭弹窗
			handleClose() {
				this.$emit('close')
			},
			
			// 点击遮罩层关闭
			handleMaskClick() {
				this.$emit('close')
			},
			
			// 商品点击事件
			handleProductClick(product) {
				this.$emit('productClick', product)
				// 点击商品后关闭弹窗
				this.handleClose()
			},
			
			// 图片加载错误处理
			handleImageError(e) {
				console.log('商品图片加载失败:', e)
			}
		}
	}
</script>

<style scoped>
	.search-modal {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background-color: rgba(0, 0, 0, 0.5);
		z-index: 1000;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 40rpx;
		box-sizing: border-box;
	}
	
	.modal-content {
		background-color: #fff;
		border-radius: 20rpx;
		width: 100%;
		max-width: 700rpx;
		max-height: 80vh;
		display: flex;
		flex-direction: column;
		overflow: hidden;
		box-shadow: 0 10rpx 30rpx rgba(0, 0, 0, 0.3);
	}
	
	.modal-header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 30rpx 30rpx 20rpx;
		border-bottom: 1rpx solid #f0f0f0;
	}
	
	.modal-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #333;
	}
	
	.close-btn {
		width: 60rpx;
		height: 60rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: 50%;
		background-color: #f5f5f5;
	}
	
	.close-icon {
		width: 30rpx;
		height: 30rpx;
	}
	
	.search-keyword {
		padding: 20rpx 30rpx;
		background-color: #f8f9fa;
		border-bottom: 1rpx solid #f0f0f0;
	}
	
	.keyword-text {
		font-size: 28rpx;
		color: #667eea;
		font-weight: bold;
		margin-right: 20rpx;
	}
	
	.result-count {
		font-size: 24rpx;
		color: #666;
	}
	
	.result-list {
		flex: 1;
	}
	
	.result-item {
		display: flex;
		background-color: #fff;
		border-radius: 16rpx;
		padding: 20rpx;
		margin-bottom: 20rpx;
		box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
		border: 1rpx solid #f0f0f0;
		transition: all 0.3s ease;
	}
	
	.result-item:active {
		transform: scale(0.98);
		box-shadow: 0 1rpx 4rpx rgba(0, 0, 0, 0.2);
	}
	
	.product-image-container {
		width: 160rpx;
		height: 160rpx;
		border-radius: 12rpx;
		overflow: hidden;
		background-color: #f5f5f5;
		margin-right: 20rpx;
		flex-shrink: 0;
	}
	
	.product-image {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}
	
	.product-info {
		flex: 1;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}
	
	.product-name {
		font-size: 28rpx;
		font-weight: bold;
		color: #333;
		margin-bottom: 8rpx;
		line-height: 1.3;
		/* 最多显示2行 */
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		overflow: hidden;
	}
	
	.product-category {
		font-size: 22rpx;
		color: #667eea;
		margin-bottom: 8rpx;
		background-color: #f0f4ff;
		padding: 4rpx 12rpx;
		border-radius: 12rpx;
		align-self: flex-start;
	}
	
	.product-description {
		font-size: 24rpx;
		color: #666;
		line-height: 1.4;
		margin-bottom: 12rpx;
		/* 最多显示2行 */
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		overflow: hidden;
	}
	
	.product-price-container {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	
	.product-price {
		font-size: 28rpx;
		font-weight: bold;
		color: #ff6b6b;
	}
	
	.product-tag {
		background-color: #f8f9fa;
		border-radius: 12rpx;
		padding: 6rpx 12rpx;
		border: 1rpx solid #e9ecef;
	}
	
	.tag-text {
		font-size: 20rpx;
		color: #666;
	}
	
	.no-result {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 80rpx 40rpx;
		text-align: center;
	}
	
	.no-result-icon {
		width: 120rpx;
		height: 120rpx;
		opacity: 0.3;
		margin-bottom: 30rpx;
	}
	
	.no-result-text {
		font-size: 28rpx;
		color: #666;
		margin-bottom: 10rpx;
	}
	
	.no-result-tip {
		font-size: 24rpx;
		color: #999;
	}
</style>
