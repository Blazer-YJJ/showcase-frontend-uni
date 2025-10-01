<template>
	<view class="weekly-recommendations">
		<view class="section-header">
			<text class="section-title">黄金精品推荐</text>
		</view>
		<scroll-view 
			class="recommendations-scroll" 
			scroll-x="true" 
			show-scrollbar="false"
		>
			<view class="recommendations-container">
				<view 
					class="recommendation-card" 
					v-for="(item, index) in recommendations" 
					:key="index"
					@click="handleCardClick(item)"
				>
					<view class="card-image">
						<image :src="item.imagePlaceholder" class="product-image" mode="aspectFit" />
					</view>
					<view class="card-content">
						<text class="card-title">{{ item.title }}</text>
						<text class="card-author">克重: {{ item.author }}</text>
						<view class="card-tag">
							<text class="tag-text">{{ item.tag }}</text>
						</view>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		name: 'RecommendedProducts',
		data() {
			return {
				recommendations: [
					{
						id: 1,
						imagePlaceholder: '/static/images/001.png',
						title: '黄金项链',
						author: '15.6g',
						tag: '#黄金首饰'
					},
					{
						id: 2,
						imagePlaceholder: '/static/images/002.png',
						title: '黄金手镯',
						author: '22.3g',
						tag: '#镂空设计'
					},
					{
						id: 3,
						imagePlaceholder: '/static/images/003.png',
						title: '黄金戒指',
						author: '8.9g',
						tag: '#弯绕小蛇'
					},
					{
						id: 4,
						imagePlaceholder: '/static/images/004.png',
						title: '黄金配件',
						author: '12.7g',
						tag: '#黄金挂饰'
					}
				]
			}
		},
		methods: {
			handleCardClick(item) {
				this.$emit('cardClick', item)
			}
		}
	}
</script>

<style scoped>
	.weekly-recommendations {
		margin: 20rpx;
		background: #FFFFFF;
		border-radius: 12rpx;
		padding: 30rpx;
		box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.1);
		position: relative;
		overflow: hidden;
	}
	
	
	.section-header {
		margin-bottom: 30rpx;
		position: relative;
		z-index: 1;
	}
	
	.section-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #333333;
		text-shadow: none;
		letter-spacing: 1rpx;
	}
	
	.recommendations-scroll {
		height: 370rpx;
		width: 100%;
		white-space: nowrap;
		position: relative;
		z-index: 1;
	}
	
	.recommendations-container {
		display: flex;
		gap: 20rpx;
		padding-top: 10rpx;
		padding-right: 20rpx;
	}
	
	.recommendation-card {
		flex-shrink: 0;
		width: 280rpx;
		background: #FFFFFF;
		border-radius: 12rpx;
		overflow: hidden;
		box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.08);
		transition: all 0.3s ease;
		position: relative;
	}
	
	
	.recommendation-card:active {
		transform: scale(0.95);
		box-shadow: 0 6rpx 16rpx rgba(0, 0, 0, 0.12);
	}
	
	.card-image {
		height: 180rpx;
		background: #FFFFFF;
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
		overflow: hidden;
	}
	
	.card-image::before {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.2) 0%, transparent 60%);
	}
	
	.product-image {
		width: 100%;
		height: 100%;
		position: relative;
		z-index: 2;
	}
	
	.card-content {
		padding: 20rpx;
		background: #FFFFFF;
		position: relative;
	}
	
	.card-title {
		font-size: 26rpx;
		font-weight: bold;
		color: #333333;
		margin-bottom: 8rpx;
		display: block;
		line-height: 1.3;
	}
	
	.card-author {
		font-size: 22rpx;
		color: #666666;
		margin-bottom: 12rpx;
		display: block;
		font-weight: 500;
	}
	
	.card-tag {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		background-color: #daa520;
		border-radius: 15rpx;
		padding: 6rpx 14rpx;
		box-shadow: 0 2rpx 6rpx rgba(255, 215, 0, 0.3);
		position: relative;
		overflow: hidden;
	}
	
	.card-tag::before {
		content: '';
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
		transition: left 0.5s;
	}
	
	.card-tag:active::before {
		left: 100%;
	}
	
	.tag-text {
		font-size: 18rpx;
		color: #FFFFFF;
		font-weight: bold;
		text-shadow: 1rpx 1rpx 2rpx rgba(255,255,255,0.3);
		position: relative;
		z-index: 1;
	}
</style>
