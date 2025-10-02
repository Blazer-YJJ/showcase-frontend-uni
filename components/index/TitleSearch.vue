<template>
	<view class="app-header">
		<view class="header-left">
			<text class="app-title">{{ title }}</text>
		</view>
		<view class="header-center">
			<view class="search-box">
				<input 
					class="search-input" 
					type="text" 
					:placeholder="searchPlaceholder"
					v-model="searchValue"
					@confirm="handleSearch"
					@input="handleInputChange"
				/>
			<view class="search-btn" @click="handleSearch">
				<image class="search-icon" src="/static/icons/sousuo.png" mode="aspectFit"></image>
			</view>
			</view>
		</view>
		<view class="header-right">
			<view class="image-search-btn" @click="handleImageSearch">
				<image class="image-search-icon" src="/static/icons/xj.png" mode="aspectFit"></image>
			</view>
		</view>
		
		<!-- 搜索结果弹窗 -->
		<SearchResultModal 
			:visible="showSearchModal"
			:searchKeyword="searchValue"
			:searchResults="searchResults"
			@close="handleCloseSearchModal"
			@productClick="handleProductClick"
		/>
		
		<!-- 以图搜图弹窗 -->
		<ImageSearchModal 
			:visible="showImageSearchModal"
			@close="handleCloseImageSearchModal"
			@productClick="handleProductClick"
		/>
	</view>
</template>

<script>
	import SearchResultModal from './SearchResultModal.vue'
	import ImageSearchModal from './ImageSearchModal.vue'
	
	export default {
		name: 'TitleSearch',
		components: {
			SearchResultModal,
			ImageSearchModal
		},
		props: {
			title: {
				type: String,
				default: 'Aether 黄金'
			},
			searchPlaceholder: {
				type: String,
				default: '搜索项链、手镯、戒指...'
			}
		},
		data() {
			return {
				searchValue: '',
				showSearchModal: false,
				showImageSearchModal: false,
				searchResults: []
			}
		},
		methods: {
			handleSearch() {
				if (this.searchValue.trim()) {
					// 执行搜索逻辑
					this.performSearch(this.searchValue.trim())
					// 显示搜索结果弹窗
					this.showSearchModal = true
				} else {
					uni.showToast({
						title: '请输入搜索关键词',
						icon: 'none'
					})
				}
			},
			
			handleInputChange(e) {
				this.searchValue = e.detail.value
			},
			
			handleImageSearch() {
				// 显示以图搜图弹窗
				this.showImageSearchModal = true
			},
			
			// 执行搜索功能
			performSearch(keyword) {
				// 模拟搜索数据 - 实际项目中这里应该调用API
				const mockProducts = [
					{
						id: 1,
						name: '经典黄金项链',
						category: '项链',
						description: '采用18K黄金制作，经典简约设计，适合日常佩戴',
						price: 2580,
						image: '/static/images/001.png',
						tag: '热销'
					},
					{
						id: 2,
						name: '钻石手镯',
						category: '手镯',
						description: '镶嵌精美钻石，工艺精湛，彰显优雅气质',
						price: 5680,
						image: '/static/images/002.png',
						tag: '新品'
					},
					{
						id: 3,
						name: '珍珠戒指',
						category: '戒指',
						description: '天然珍珠制作，温润如玉，展现女性柔美',
						price: 1280,
						image: '/static/images/003.png',
						tag: '推荐'
					},
					{
						id: 4,
						name: '翡翠吊坠',
						category: '吊坠',
						description: '上等翡翠材质，雕工精美，寓意吉祥如意',
						price: 8880,
						image: '/static/images/004.png',
						tag: '限量'
					},
					{
						id: 5,
						name: '银质耳环',
						category: '耳环',
						description: '纯银制作，简约时尚，适合各种场合佩戴',
						price: 680,
						image: '/static/images/001.png',
						tag: '特价'
					}
				]
				
				// 根据关键词过滤商品
				this.searchResults = mockProducts.filter(product => 
					product.name.includes(keyword) || 
					product.category.includes(keyword) ||
					product.description.includes(keyword)
				)
				
				console.log('搜索关键词:', keyword)
				console.log('搜索结果:', this.searchResults)
			},
			
			// 关闭搜索弹窗
			handleCloseSearchModal() {
				this.showSearchModal = false
			},
			
			// 关闭以图搜图弹窗
			handleCloseImageSearchModal() {
				this.showImageSearchModal = false
			},
			
			// 商品点击事件
			handleProductClick(product) {
				this.$emit('productClick', product)
				uni.showToast({
					title: `查看${product.name}`,
					icon: 'none'
				})
			}
		}
	}
</script>

<style scoped>
	.app-header {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 999;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 20rpx 30rpx;
		background-color: #fff;
		border-bottom: 1rpx solid #f0f0f0;
		height: 100rpx;
		box-sizing: border-box;
		box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
	}
	
	.header-left {
		flex: 0 0 auto;
	}
	
	.app-title {
		font-size: 36rpx;
		font-weight: bold;
		color: #333;
	}
	
	.header-center {
		flex: 1;
		margin: 0 20rpx;
	}
	
	.search-box {
		background-color: #f5f5f5;
		border-radius: 25rpx;
		padding: 10rpx 15rpx;
		height: 60rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	
	.search-input {
		flex: 1;
		font-size: 24rpx;
		color: #333;
		background: transparent;
		border: none;
		outline: none;
		padding: 0 10rpx;
	}
	
	.search-btn {
		width: 40rpx;
		height: 40rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-left: 10rpx;
	}
	
	.search-icon {
		width: 100%;
		height: 90%;
	}
	
	.header-right {
		flex: 0 0 auto;
	}
	
	.image-search-btn {
		background-color: #d4af37;
		border: 1rpx solid #d4af37;
		border-radius: 20rpx;
		padding: 8rpx 12rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		min-width: 120rpx;
		height: 60rpx;
		box-sizing: border-box;
	}
	
	.image-search-icon {
		width: 50rpx;
		height: 50rpx;
		margin-right: 6rpx;
	}
	
	.image-search-text {
		font-size: 20rpx;
		color: #666;
		font-weight: 500;
	}
</style>
