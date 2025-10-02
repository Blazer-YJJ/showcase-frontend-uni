<template>
	<view class="container">
		<!-- 标题搜索栏 -->
		<TitleSearch 
			@search="handleSearch" 
			@goToProfile="handleGoToProfile"
			@productClick="handleProductClick"
		/>

		<!-- 快捷入口区 -->
		<QuickEntry @itemClick="handleQuickAccessClick" />

		<!-- 信息轮播区 -->
		<InfoBanner @bannerClick="handleBannerClick" />

		<!-- 公告轮播区 -->
		<AnnouncementBanner @announcementClick="handleAnnouncementClick" />

		<!-- 精品款式推荐 -->
		<RecommendedProducts @cardClick="handleRecommendationClick" />

		<!-- 上新公告 -->
		<LatestAnnouncement 
			@seriesClick="handleSeriesClick"
			@interviewClick="handleInterviewClick" 
		/>

		<!-- 热门款式推荐 -->
		<HotRecommendations @productClick="handleProductClick" />
	</view>
</template>

<script>
	import TitleSearch from '@/components/index/TitleSearch.vue'
	import QuickEntry from '@/components/index/QuickEntry.vue'
	import InfoBanner from '@/components/index/InfoBanner.vue'
	import AnnouncementBanner from '@/components/index/AnnouncementBanner.vue'
	import RecommendedProducts from '@/components/index/RecommendedProducts.vue'
	import HotRecommendations from '@/components/index/HotRecommendations.vue'
	import LatestAnnouncement from '@/components/index/LatestAnnouncement.vue'
	import Pagination from '@/components/index/Pagination.vue'

	export default {
		components: {
			TitleSearch,
			QuickEntry,
			InfoBanner,
			AnnouncementBanner,
			RecommendedProducts,
			LatestAnnouncement,
			Pagination,
			HotRecommendations
		},
			data() {
				return {}
			},
	onLoad() {
		console.log('首页加载')
		console.log('组件列表:', Object.keys(this.$options.components))
	},
		methods: {
			handleSearch(searchKeyword) {
				console.log('执行搜索操作:', searchKeyword)
				// 搜索功能现在由 TitleSearch 组件内部处理
				// 这里可以添加额外的搜索逻辑，比如记录搜索历史等
			},
			handleGoToProfile() {
				console.log('跳转到用户中心')
				uni.switchTab({
					url: '/pages/member/member'
				})
			},
			handleQuickAccessClick(item) {
				console.log('快捷入口点击:', item)
				uni.showToast({
					title: `点击了${item.label}`,
					icon: 'none'
				})
			},
			handleBannerClick(bannerData) {
				console.log('横幅点击:', bannerData)
				uni.showToast({
					title: '查看数字国风美学',
					icon: 'none'
				})
			},
			handleAnnouncementClick(announcementData) {
				console.log('公告点击:', announcementData)
				uni.showToast({
					title: '查看公告详情',
					icon: 'none'
				})
				// 可以跳转到公告详情页
				// uni.navigateTo({
				// 	url: announcementData.url
				// })
			},
			handleRecommendationClick(item) {
				console.log('推荐卡片点击:', item)
				uni.showToast({
					title: `查看${item.title}`,
					icon: 'none'
				})
			},
			handleSeriesClick() {
				console.log('典藏系列点击')
				uni.showToast({
					title: '查看典藏系列',
					icon: 'none'
				})
			},
			handleInterviewClick() {
				console.log('独家访谈点击')
				uni.showToast({
					title: '查看独家访谈',
					icon: 'none'
				})
			},
			handleProductClick(product) {
				console.log('作品点击:', product)
				uni.showToast({
					title: `查看${product.title}`,
					icon: 'none'
				})
			},
			handlePageChange(newPage) {
				console.log('页码切换:', newPage)
				this.currentPage = newPage
				uni.showToast({
					title: `切换到第${newPage}页`,
					icon: 'none'
				})
			}
		}
	}
</script>

<style>
	.container {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		background-color: #f8f8f8;
		padding-top: 100rpx; /* 为固定头部预留空间 */
	}
</style>
