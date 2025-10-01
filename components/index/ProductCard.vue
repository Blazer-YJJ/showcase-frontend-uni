<template>
	<view class="product-card" @click="handleCardClick">
		<view class="card-image">
			<image 
				:src="product.imagePlaceholder" 
				class="product-image" 
				mode="aspectFit"
				@error="handleImageError"
			/>
		</view>
		<view class="card-content">
			<text class="product-title">{{ product.title }}</text>
			<text class="weight-info">{{ product.weight }}</text>
			<view class="card-footer">
				<text class="product-price">¥{{ product.price }}</text>
				<view class="product-tag">
					<text class="tag-text">{{ product.tag }}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'ProductCard',
		props: {
			product: {
				type: Object,
				required: true,
				default: () => ({
					id: 1,
					imagePlaceholder: '',
					title: '',
					weight: '',
					price: 0,
					tag: ''
				})
			}
		},
		methods: {
			handleCardClick() {
				this.$emit('cardClick', this.product)
			},
			handleImageError(e) {
				console.log('图片加载失败:', e)
				// 可以设置默认图片或显示占位符
			}
		}
	}
</script>

<style scoped>
	.product-card {
		background-color: #fff;
		border-radius: 12rpx;
		overflow: hidden;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.1);
		margin-bottom: 20rpx;
	}
	
	.card-image {
		height: 200rpx;
		background-color: #e9ecef;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.product-image {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}
	
	.card-content {
		padding: 20rpx;
	}
	
	.product-title {
		font-size: 28rpx;
		font-weight: bold;
		color: #333;
		margin-bottom: 10rpx;
		display: block;
		line-height: 1.3;
	}
	
	.weight-info {
		font-size: 24rpx;
		color: #666;
		margin-bottom: 15rpx;
		display: block;
	}
	
	.card-footer {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	
	.product-price {
		font-size: 28rpx;
		font-weight: bold;
		color: #ff6b6b;
	}
	
	.product-tag {
		background-color: #f8f9fa;
		border-radius: 15rpx;
		padding: 6rpx 12rpx;
		border: 1rpx solid #e9ecef;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.tag-text {
		font-size: 20rpx;
		color: #666;
	}
</style>
