<template>
	<view class="hero-banner-container">
		<swiper 
			class="hero-swiper" 
			:current="currentIndex" 
			@change="onSwiperChange"
			:autoplay="true"
			:interval="4000"
			:duration="500"
			:circular="true"
		>
			<swiper-item 
				v-for="(banner, index) in bannerList" 
				:key="index"
				class="swiper-item"
			>
				<view 
					class="hero-banner" 
					:style="{ background: banner.background }"
				>
					<view class="banner-content">
						<view class="content-left">
							<text class="banner-title">{{ banner.title }}</text>
							<text class="banner-description">{{ banner.description }}</text>
						</view>
						<view class="content-right">
							<image 
								class="banner-image" 
								:src="banner.image" 
								mode="aspectFit"
							/>
						</view>
					</view>
					<button class="banner-button" @click="handleButtonClick(banner)">
						{{ banner.buttonText }}
					</button>
				</view>
			</swiper-item>
		</swiper>
		<view class="carousel-indicators">
			<view 
				class="indicator" 
				v-for="(item, index) in bannerList" 
				:key="index"
				:class="{ active: index === currentIndex }"
				@click="goToSlide(index)"
			></view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'InfoBanner',
		data() {
			return {
				currentIndex: 0,
				carouselTimer: null,
				bannerList: [
					{
						title: '黄金首饰精品',
						description: '纯手工打造，传承千年工艺，每一件都是艺术品。',
						buttonText: '立即查看',
						image: '/static/images/001.png',
						background: 'linear-gradient(135deg, #ffd700 0%, #ffed4e 100%)'
					},
					{
						title: '定制专属首饰',
						description: '根据您的需求定制独一无二的黄金首饰，彰显个性品味。',
						buttonText: '开始定制',
						image: '/static/images/002.png',
						background: 'linear-gradient(135deg, #b8860b 0%, #daa520 100%)'
					},
					{
						title: '经典传承工艺',
						description: '融合传统工艺与现代设计，打造永恒经典的黄金首饰。',
						buttonText: '了解更多',
						image: '/static/images/003.png',
						background: 'linear-gradient(135deg, #cd853f 0%, #daa520 100%)'
					},
					{
						title: '限量珍藏系列',
						description: '精选稀有黄金材质，限量发售，值得珍藏的传世之作。',
						buttonText: '立即收藏',
						image: '/static/images/004.png',
						background: 'linear-gradient(135deg, #8b7355 0%, #b8860b 100%)'
					}
				]
			}
		},
		methods: {
			handleButtonClick(banner) {
				this.$emit('bannerClick', banner)
			},
			onSwiperChange(e) {
				this.currentIndex = e.detail.current
			},
			goToSlide(index) {
				this.currentIndex = index
			}
		},
		mounted() {
			// 组件挂载后可以添加额外的初始化逻辑
		},
		beforeDestroy() {
			// 清理定时器
			if (this.carouselTimer) {
				clearInterval(this.carouselTimer)
			}
		}
	}
</script>

<style scoped>
	.hero-banner-container {
		margin: 20rpx;
		border-radius: 12rpx;
		overflow: hidden;
		position: relative;
	}
	
	.hero-swiper {
		height: 300rpx;
	}
	
	.swiper-item {
		height: 100%;
	}
	
	.hero-banner {
		height: 100%;
		padding: 0rpx 20rpx 0rpx 20rpx;
		position: relative;
		display: flex;
		align-items: center;
	}
	
	.banner-content {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 40rpx;
	}
	
	.content-left {
		flex: 1;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
	
	.content-right {
		flex-shrink: 0;
		width: 300rpx;
		height: 300rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.banner-image {
		width: 100%;
		height: 100%;
		border-radius: 20rpx;
		box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.15);
	}
	
	.banner-title {
		font-size: 48rpx;
		font-weight: bold;
		color: #fff;
		margin-bottom: 20rpx;
		line-height: 1.2;
	}
	
	.banner-description {
		font-size: 28rpx;
		color: rgba(255, 255, 255, 0.9);
		margin-bottom: 0;
		line-height: 1.4;
	}
	
	.banner-button {
		position: absolute;
		bottom: 20rpx;
		right: 20rpx;
		background-color: rgba(255, 215, 0, 0.8);
		color: #333;
		border: 1rpx solid rgba(255, 255, 255, 0.3);
		border-radius: 20rpx;
		padding: 12rpx 24rpx;
		font-size: 24rpx;
		font-weight: bold;
		width: fit-content;
		transition: all 0.3s ease;
		box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.15);
		z-index: 10;
		backdrop-filter: blur(10rpx);
	}
	
	.banner-button:active {
		transform: scale(0.95);
		background-color: rgba(255, 237, 78, 0.9);
	}
	
	/* 响应式设计 */
	@media screen and (max-width: 750rpx) {
		.banner-content {
			flex-direction: column;
			text-align: center;
			gap: 30rpx;
		}
		
		.content-right {
			width: 250rpx;
			height: 250rpx;
		}
		
		.banner-title {
			font-size: 40rpx;
		}
		
		.banner-description {
			font-size: 26rpx;
		}
		
		.banner-button {
			bottom: 15rpx;
			right: 15rpx;
			padding: 10rpx 20rpx;
			font-size: 22rpx;
			border-radius: 18rpx;
		}
	}
	
	.carousel-indicators {
		display: flex;
		justify-content: center;
		gap: 10rpx;
		padding: 20rpx;
		position: absolute;
		bottom: 0;
		left: 50%;
		transform: translateX(-50%);
	}
	
	.indicator {
		width: 12rpx;
		height: 12rpx;
		border-radius: 50%;
		background-color: rgba(255, 255, 255, 0.3);
		transition: all 0.3s ease;
		cursor: pointer;
	}
	
	.indicator.active {
		background-color: #fff;
		transform: scale(1.2);
	}
	
	.indicator:hover {
		background-color: rgba(255, 255, 255, 0.6);
	}
</style>
