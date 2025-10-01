<template>
	<view class="featured-content-section">
		<view class="section-header">
			<text class="section-title">上新公告</text>
		</view>
		<view class="carousel-container">
			<swiper 
				class="carousel-swiper"
				:indicator-dots="true"
				:autoplay="true"
				:interval="4000"
				:duration="500"
				:vertical="true"
				:circular="true"
				indicator-color="rgba(255,255,255,0.5)"
				indicator-active-color="#d4af37"
			>
				<swiper-item 
					v-for="(item, index) in carouselItems" 
					:key="index"
					class="swiper-item"
				>
					<view 
						class="content-card" 
						:class="item.cardClass"
						@click="handleCardClick(item)"
					>
						<view class="card-header">
							<text class="card-title">{{ item.title }}</text>
							<view class="card-arrow">
								<text class="arrow-text">→</text>
							</view>
						</view>
							<text class="card-description">{{ item.description }}</text>
							<view class="card-time">{{ item.time || '刚刚发布' }}</view>
					</view>
				</swiper-item>
			</swiper>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'LatestAnnouncement',
		data() {
			return {
				carouselItems: [
					{
							id: 1,
							title: '黄金价格调整通知',
							description: '根据国际金价波动，我们已调整部分黄金首饰价格，确保为您提供最优惠的价格。',
							time: '2小时前',
							cardClass: 'price-card',
							type: 'price'
						},
					{
							id: 2,
							title: '新店开业庆典',
							description: '我们在市中心新开设了一家精品店，开业期间全场黄金首饰享受9.5折优惠。',
							time: '1天前',
							cardClass: 'store-card',
							type: 'store'
						},
					{
							id: 3,
							title: '工艺升级公告',
							description: '我们引进了最新的3D打印技术，为传统黄金工艺注入现代科技元素，品质更上一层楼。',
							time: '3天前',
							cardClass: 'tech-card',
							type: 'tech'
						},
					{
							id: 4,
							title: '售后服务升级',
							description: '即日起，所有黄金首饰享受终身免费保养服务，让您的珠宝永远闪耀如新。',
							time: '1周前',
							cardClass: 'service-card',
							type: 'service'
						},
					{
							id: 5,
							title: '设计师合作消息',
							description: '我们与知名珠宝设计师达成合作，将推出限量版黄金首饰系列，敬请期待。',
							time: '2周前',
							cardClass: 'designer-card',
							type: 'designer'
						}
				]
			}
		},
		methods: {
			handleCardClick(item) {
				this.$emit('cardClick', item)
			},
			handleSeriesClick() {
				this.$emit('seriesClick')
			},
			handleInterviewClick() {
				this.$emit('interviewClick')
			}
		}
	}
</script>

<style scoped>
	.featured-content-section {
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
	
	.carousel-container {
		height: 280rpx;
		position: relative;
	}
	
	.carousel-swiper {
		height: 100%;
		width: 100%;
	}
	
	.swiper-item {
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.content-card {
		display: flex;
		flex-direction: column;
		padding: 24rpx;
		background-color: #f8f9fa;
		border-radius: 8rpx;
		border: 1rpx solid #e9ecef;
		position: relative;
		transition: all 0.3s ease;
	}
	
	.content-card:active {
		transform: translateY(2rpx);
		box-shadow: 0 4rpx 12rpx rgba(0,0,0,0.08);
	}
	
	.price-card {
		border-left: 4rpx solid #d4af37;
		background: linear-gradient(135deg, #fff9e6 0%, #f8f9fa 100%);
	}

	.store-card {
		border-left: 4rpx solid #c9a96e;
		background: linear-gradient(135deg, #f8f6f0 0%, #f8f9fa 100%);
	}

	.tech-card {
		border-left: 4rpx solid #e74c3c;
		background: linear-gradient(135deg, #fdf2f2 0%, #f8f9fa 100%);
	}

	.service-card {
		border-left: 4rpx solid #9b59b6;
		background: linear-gradient(135deg, #f4f0f7 0%, #f8f9fa 100%);
	}

	.designer-card {
		border-left: 4rpx solid #3498db;
		background: linear-gradient(135deg, #f0f8ff 0%, #f8f9fa 100%);
	}
	
	.card-header {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		margin-bottom: 16rpx;
	}
	
	.card-title {
		font-size: 28rpx;
		font-weight: 600;
		display: block;
		line-height: 1.4;
		color: #333;
		flex: 1;
		margin-right: 20rpx;
	}
	
	.card-description {
		font-size: 24rpx;
		margin-bottom: 16rpx;
		display: block;
		line-height: 1.5;
		color: #666;
	}
	
	.card-time {
		font-size: 22rpx;
		color: #999;
		align-self: flex-end;
		margin-top: auto;
	}
	
	.card-arrow {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 40rpx;
		height: 40rpx;
		background-color: #f5f5f5;
		border-radius: 50%;
		flex-shrink: 0;
	}
	
	.arrow-text {
		font-size: 24rpx;
		font-weight: 600;
		color: #999999;
	}
</style>
