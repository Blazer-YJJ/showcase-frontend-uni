<template>
	<view class="announcement-container">
		<view class="announcement-banner" @click="handleAnnouncementClick">
			<!-- 轮播容器 -->
			<view class="announcement-scroll-container">
				<view 
					class="announcement-text" 
					:style="{ transform: `translateY(${scrollOffset}px)` }"
				>
					<view 
						v-for="(announcement, index) in announcementList" 
						:key="index"
						class="announcement-item"
					>
						<text class="announcement-message">温馨提醒：{{ announcement.text }}</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'AnnouncementBanner',
		data() {
			return {
				scrollOffset: 0,
				scrollTimer: null,
				isScrolling: false,
				currentIndex: 0,
				itemHeight: 40, // 每个公告项的高度
				announcementData: [
					{
						text: '欢迎来到 Aether 黄金饰品。',
						url: ''
					},
					{
						text: '下单前请备注好克重及指定要求。',
						url: ''
					},
					{
						text: '所有饰品均在金价基础上加20元/克手工费。',
						url: ''
					}
				]
			}
		},
		computed: {
			announcementList() {
				// 创建重复的公告列表以实现无限轮播
				const originalList = this.announcementData || []
				if (originalList.length === 0) return []
				// 重复3次以确保有足够的内容进行无限滚动
				return [...originalList, ...originalList, ...originalList]
			}
		},
		mounted() {
			this.startScroll()
		},
		beforeDestroy() {
			this.stopScroll()
		},
		methods: {
			handleAnnouncementClick() {
				// 获取当前显示的公告（使用原始索引）
				const originalIndex = this.currentIndex % this.announcementData.length
				const currentAnnouncement = this.announcementData[originalIndex]
				this.$emit('announcementClick', currentAnnouncement)
			},
			startScroll() {
				// 如果原始数据只有一个公告，不需要滚动
				if (this.announcementData.length <= 1) {
					return
				}
				
				// 延迟启动滚动，确保组件完全渲染
				setTimeout(() => {
					this.scrollTimer = setInterval(() => {
						if (!this.isScrolling) {
							this.scrollOffset -= 1
							
							// 当滚动到下一个公告的位置时
							if (this.scrollOffset <= -this.itemHeight) {
								this.currentIndex++
								
								// 当滚动到重复列表的中间位置时，重置到原始列表的开始
								if (this.currentIndex >= this.announcementData.length) {
									this.scrollOffset = 0
									this.currentIndex = 0
								}
							}
						}
					}, 100) // 每100ms移动1px，控制滚动速度
				}, 5000) // 5秒后开始滚动
			},
			stopScroll() {
				if (this.scrollTimer) {
					clearInterval(this.scrollTimer)
					this.scrollTimer = null
				}
			},
			onMouseEnter() {
				this.isScrolling = true
			},
			onMouseLeave() {
				this.isScrolling = false
			}
		}
	}
</script>

<style scoped>
	.announcement-container {
		margin: 20rpx 24rpx;
	}
	
	.announcement-banner {
		background: #FFFFFF;
		border-radius: 16rpx;
		padding: 24rpx 20rpx;
		box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
		position: relative;
		overflow: hidden;
		transition: all 0.3s ease;
		border: 1rpx solid #F0F0F0;
	}
	
	.announcement-banner:active {
		transform: scale(0.98);
		box-shadow: 0 1rpx 4rpx rgba(0, 0, 0, 0.15);
	}
	
	.announcement-scroll-container {
		height: 40rpx;
		overflow: hidden;
		position: relative;
		display: flex;
		align-items: center;
	}
	
	.announcement-text {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		transition: none; /* 移除过渡效果，让滚动更流畅 */
	}
	
	.announcement-item {
		height: 40rpx;
		display: flex;
		align-items: center;
		white-space: nowrap;
		overflow: hidden;
	}
	
	.announcement-message {
		font-size: 28rpx;
		color: #333333;
		line-height: 40rpx;
		font-weight: 500;
		width: 100%;
	}
</style>