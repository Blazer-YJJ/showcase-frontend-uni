<template>
	<view class="image-search-modal" v-if="visible">
		<!-- 固定头部 -->
		<view class="modal-header">
			<text class="modal-title">以图搜图</text>
			<view class="close-btn" @click="handleClose">
				<image class="close-icon" src="/static/icons/shanchu.png" mode="aspectFit"></image>
			</view>
		</view>
		
		<!-- 可滚动内容区域 -->
		<scroll-view class="modal-content" scroll-y="true" :scroll-top="scrollTop">
			<!-- 图片选择区域 -->
			<view class="image-select-section">
				<view class="select-tip">
					<text class="tip-text">请选择要搜索的商品图片</text>
				</view>
				
				<!-- 图片预览区域 -->
				<view class="image-preview-container" v-if="selectedImage">
					<image 
						:src="selectedImage" 
						class="preview-image" 
						mode="aspectFit"
						@error="handleImageError"
					/>
					<view class="image-overlay">
						<view class="change-image-btn" @click="selectImage">
							<image class="change-icon" src="/static/icons/sousuo.png" mode="aspectFit"></image>
							<text class="change-text">重新选择</text>
						</view>
					</view>
				</view>
				
				<!-- 选择图片按钮 -->
				<view class="select-image-btn" v-else @click="selectImage">
					<image class="select-icon" src="/static/icons/xj.png" mode="aspectFit"></image>
					<text class="select-text">选择图片</text>
					<text class="select-tip-text">支持 JPG、PNG 格式</text>
				</view>
			</view>
			
			<!-- 搜索按钮 -->
			<view class="search-section">
				<button 
					class="search-btn" 
					:class="{ 'disabled': !selectedImage || isSearching }"
					:disabled="!selectedImage || isSearching"
					@click="handleImageSearch"
				>
					<image v-if="isSearching" class="loading-icon" src="/static/icons/sousuo.png" mode="aspectFit"></image>
					<text class="search-text">{{ isSearching ? '搜索中...' : '开始搜索' }}</text>
				</button>
			</view>
			
			<!-- 搜索结果区域 -->
			<view class="search-results-section" v-if="showResults">
				<view class="results-header">
					<text class="results-title">搜索结果</text>
					<text class="results-count">找到 {{ searchResults.length }} 个相似商品</text>
				</view>
				
				<view class="results-list">
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
							<view class="similarity-score" v-if="product.similarity">
								<text class="similarity-text">相似度: {{ product.similarity }}%</text>
							</view>
						</view>
					</view>
					
					<!-- 无结果提示 -->
					<view class="no-result" v-if="searchResults.length === 0">
						<image class="no-result-icon" src="/static/icons/sousuo.png" mode="aspectFit"></image>
						<text class="no-result-text">未找到相似商品</text>
						<text class="no-result-tip">请尝试其他图片</text>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		name: 'ImageSearchModal',
		props: {
			visible: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				selectedImage: '', // 选中的图片路径
				isSearching: false, // 是否正在搜索
				showResults: false, // 是否显示搜索结果
				searchResults: [], // 搜索结果
				scrollTop: 0 // 滚动位置
			}
		},
		methods: {
			// 关闭弹窗
			handleClose() {
				this.$emit('close')
				this.resetModal()
			},
			
			// 重置弹窗状态
			resetModal() {
				this.selectedImage = ''
				this.isSearching = false
				this.showResults = false
				this.searchResults = []
			},
			
			// 选择图片
			selectImage() {
				uni.chooseImage({
					count: 1,
					sizeType: ['compressed'],
					sourceType: ['album', 'camera'],
					success: (res) => {
						this.selectedImage = res.tempFilePaths[0]
						this.showResults = false // 选择新图片时隐藏之前的结果
						console.log('选择的图片:', this.selectedImage)
					},
					fail: (err) => {
						console.log('选择图片失败:', err)
						uni.showToast({
							title: '选择图片失败',
							icon: 'none'
						})
					}
				})
			},
			
			// 执行图片搜索
			handleImageSearch() {
				if (!this.selectedImage) {
					uni.showToast({
						title: '请先选择图片',
						icon: 'none'
					})
					return
				}
				
				this.isSearching = true
				this.showResults = true
				
				// 模拟图片搜索API调用
				setTimeout(() => {
					this.performImageSearch()
					this.isSearching = false
					// 搜索完成后滚动到结果区域
					this.scrollToResults()
				}, 2000)
			},
			
			// 执行图片搜索逻辑
			performImageSearch() {
				// 模拟基于图片的搜索结果
				const mockImageSearchResults = [
					{
						id: 1,
						name: '经典黄金项链',
						category: '项链',
						description: '采用18K黄金制作，经典简约设计，适合日常佩戴',
						image: '/static/images/001.png',
						similarity: 95
					},
					{
						id: 2,
						name: '钻石手镯',
						category: '手镯',
						description: '镶嵌精美钻石，工艺精湛，彰显优雅气质',
						image: '/static/images/002.png',
						similarity: 88
					},
					{
						id: 3,
						name: '珍珠戒指',
						category: '戒指',
						description: '天然珍珠制作，温润如玉，展现女性柔美',
						image: '/static/images/003.png',
						similarity: 82
					},
					{
						id: 4,
						name: '翡翠吊坠',
						category: '吊坠',
						description: '上等翡翠材质，雕工精美，寓意吉祥如意',
						image: '/static/images/004.png',
						similarity: 76
					},
					{
						id: 5,
						name: '银质耳环',
						category: '耳环',
						description: '纯银制作，简约时尚，适合各种场合佩戴',
						image: '/static/images/001.png',
						similarity: 71
					}
				]
				
				// 按相似度排序
				this.searchResults = mockImageSearchResults.sort((a, b) => b.similarity - a.similarity)
				
				console.log('图片搜索结果:', this.searchResults)
				
				uni.showToast({
					title: `找到 ${this.searchResults.length} 个相似商品`,
					icon: 'none'
				})
			},
			
			// 商品点击事件
			handleProductClick(product) {
				this.$emit('productClick', product)
				uni.showToast({
					title: `查看${product.name}`,
					icon: 'none'
				})
			},
			
			// 图片加载错误处理
			handleImageError(e) {
				console.log('图片加载失败:', e)
			},
			
			// 滚动到搜索结果区域
			scrollToResults() {
				// 延迟执行，确保DOM已更新
				this.$nextTick(() => {
					// 滚动到搜索结果区域（大约400rpx的位置）
					this.scrollTop = 400
				})
			}
		}
	}
</script>

<style scoped>
	.image-search-modal {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background-color: #fff;
		z-index: 1000;
		display: flex;
		flex-direction: column;
	}
	
	.modal-header {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 1001;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 30rpx 30rpx 20rpx;
		background-color: #fff;
		border-bottom: 1rpx solid #f0f0f0;
		box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
	}
	
	.modal-content {
		background-color: #fff;
		width: 100%;
		height: 100vh;
		padding-top: 100rpx; /* 为固定头部预留空间 */
		box-sizing: border-box;
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
	
	.image-select-section {
		padding: 30rpx;
		border-bottom: 1rpx solid #f0f0f0;
	}
	
	.select-tip {
		margin-bottom: 20rpx;
	}
	
	.tip-text {
		font-size: 26rpx;
		color: #666;
	}
	
	.image-preview-container {
		position: relative;
		width: 100%;
		height: 300rpx;
		border-radius: 16rpx;
		overflow: hidden;
		background-color: #f5f5f5;
		margin-bottom: 20rpx;
	}
	
	.preview-image {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}
	
	.image-overlay {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background-color: rgba(0, 0, 0, 0.3);
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.change-image-btn {
		background-color: rgba(255, 255, 255, 0.9);
		border-radius: 20rpx;
		padding: 12rpx 24rpx;
		display: flex;
		align-items: center;
		flex-direction: column;
	}
	
	.change-icon {
		width: 40rpx;
		height: 40rpx;
		margin-bottom: 8rpx;
	}
	
	.change-text {
		font-size: 22rpx;
		color: #333;
	}
	
	.select-image-btn {
		width: 100%;
		height: 300rpx;
		border: 2rpx dashed #ddd;
		border-radius: 16rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-color: #fafafa;
		transition: all 0.3s ease;
	}
	
	.select-image-btn:active {
		background-color: #f0f0f0;
		border-color: #667eea;
	}
	
	.select-icon {
		width: 80rpx;
		height: 80rpx;
		margin-bottom: 20rpx;
		opacity: 0.6;
	}
	
	.select-text {
		font-size: 28rpx;
		color: #333;
		font-weight: bold;
		margin-bottom: 10rpx;
	}
	
	.select-tip-text {
		font-size: 22rpx;
		color: #999;
	}
	
	.search-section {
		padding: 30rpx;
		border-bottom: 1rpx solid #f0f0f0;
	}
	
	.search-btn {
		width: 100%;
		height: 80rpx;
		background: linear-gradient(135deg, #d4af37 0%, #b8941f 100%);
		border-radius: 40rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		border: none;
		box-shadow: 0 4rpx 12rpx rgba(212, 175, 55, 0.3);
		transition: all 0.3s ease;
	}
	
	.search-btn:not(.disabled):active {
		transform: translateY(2rpx);
		box-shadow: 0 2rpx 8rpx rgba(212, 175, 55, 0.4);
	}
	
	.search-btn.disabled {
		background: #495057;
		box-shadow: none;
	}
	
	.loading-icon {
		width: 32rpx;
		height: 32rpx;
		margin-right: 12rpx;
		animation: rotate 1s linear infinite;
	}
	
	@keyframes rotate {
		from { transform: rotate(0deg); }
		to { transform: rotate(360deg); }
	}
	
	.search-text {
		font-size: 28rpx;
		color: #fff;
		font-weight: bold;
	}
	
	.search-results-section {
		display: flex;
		flex-direction: column;
	}
	
	.results-header {
		padding: 20rpx 30rpx;
		background-color: #f8f9fa;
		border-bottom: 1rpx solid #f0f0f0;
	}
	
	.results-title {
		font-size: 28rpx;
		font-weight: bold;
		color: #d4af37;
		margin-bottom: 8rpx;
	}
	
	.results-count {
		font-size: 24rpx;
		color: #495057;
	}
	
	.results-list {
		padding: 20rpx;
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
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		overflow: hidden;
	}
	
	.product-category {
		font-size: 22rpx;
		color: #d4af37;
		margin-bottom: 8rpx;
		background-color: rgba(212, 175, 55, 0.1);
		padding: 4rpx 12rpx;
		border-radius: 12rpx;
		align-self: flex-start;
	}
	
	.product-description {
		font-size: 24rpx;
		color: #495057;
		line-height: 1.4;
		margin-bottom: 12rpx;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		-webkit-box-orient: vertical;
		overflow: hidden;
	}
	
	.similarity-score {
		background-color: rgba(212, 175, 55, 0.1);
		border-radius: 12rpx;
		padding: 6rpx 12rpx;
		align-self: center;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.similarity-text {
		font-size: 20rpx;
		color: #d4af37;
		font-weight: bold;
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
