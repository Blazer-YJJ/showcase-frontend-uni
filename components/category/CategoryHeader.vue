<template>
	<view class="category-header">
		<!-- 搜索框 -->
		<view class="search-container">
			<view class="search-box">
				<image 
					src="/static/icons/sousuo.png" 
					class="search-icon"
					mode="aspectFit"
				/>
				<input 
					class="search-input"
					type="text"
					placeholder="搜索商品..."
					v-model="searchKeyword"
					@input="handleSearchInput"
					@confirm="handleSearchConfirm"
				/>
				<view 
					v-if="searchKeyword"
					class="clear-btn"
					@click="clearSearch"
				>
					<text class="clear-text">×</text>
				</view>
			</view>
		</view>
		
		<!-- 导出按钮组 -->
		<view class="export-container">
			<view 
				class="export-btn search-export"
				@click="handleSearchExport"
			>
				<image 
					src="/static/icons/sousuo.png" 
					class="export-icon"
					mode="aspectFit"
				/>
				<text class="export-text">搜索结果PDF导出</text>
			</view>
			
			<view 
				class="export-btn category-export"
				@click="handleCategoryExport"
			>
				<image 
					src="/static/icons/fenlei.png" 
					class="export-icon"
					mode="aspectFit"
				/>
				<text class="export-text">指定分类PDF导出</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'CategoryHeader',
		props: {
			currentCategory: {
				type: Object,
				default: () => null
			}
		},
		data() {
			return {
				searchKeyword: '',
				searchTimer: null
			}
		},
		methods: {
			// 处理搜索输入
			handleSearchInput() {
				// 防抖处理
				clearTimeout(this.searchTimer)
				this.searchTimer = setTimeout(() => {
					this.$emit('search', this.searchKeyword)
				}, 500)
			},
			
			// 处理搜索确认
			handleSearchConfirm() {
				clearTimeout(this.searchTimer)
				this.$emit('search', this.searchKeyword)
			},
			
			// 清空搜索
			clearSearch() {
				this.searchKeyword = ''
				this.$emit('search', '')
			},
			
			// 处理搜索导出PDF
			handleSearchExport() {
				if (!this.searchKeyword.trim()) {
					uni.showToast({
						title: '请输入搜索关键词',
						icon: 'none',
						duration: 2000
					})
					return
				}
				
				uni.showLoading({
					title: '正在生成PDF...'
				})
				
				// 模拟PDF生成
				setTimeout(() => {
					uni.hideLoading()
					uni.showToast({
						title: '搜索PDF导出成功',
						icon: 'success',
						duration: 2000
					})
					this.$emit('searchExport', {
						keyword: this.searchKeyword,
						timestamp: new Date().getTime()
					})
				}, 2000)
			},
			
			// 处理分类导出PDF
			handleCategoryExport() {
				if (!this.currentCategory) {
					uni.showToast({
						title: '请先选择分类',
						icon: 'none',
						duration: 2000
					})
					return
				}
				
				uni.showLoading({
					title: '正在生成PDF...'
				})
				
				// 模拟PDF生成
				setTimeout(() => {
					uni.hideLoading()
					uni.showToast({
						title: '分类PDF导出成功',
						icon: 'success',
						duration: 2000
					})
					this.$emit('categoryExport', {
						category: this.currentCategory,
						timestamp: new Date().getTime()
					})
				}, 2000)
			}
		}
	}
</script>

<style scoped>
	.category-header {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 1000;
		background-color: #fff;
		padding: 20rpx;
		border-bottom: 1rpx solid #e9ecef;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.05);
	}
	
	/* 搜索容器 */
	.search-container {
		margin-bottom: 20rpx;
	}
	
	.search-box {
		display: flex;
		align-items: center;
		background-color: #f8f9fa;
		border-radius: 25rpx;
		padding: 0 20rpx;
		height: 70rpx;
		border: 1rpx solid #e9ecef;
		transition: all 0.3s ease;
	}
	
	.search-box:focus-within {
		border-color: #d4af37;
		background-color: #fff;
		box-shadow: 0 0 0 2rpx rgba(212, 175, 55, 0.2);
	}
	
	.search-icon {
		width: 32rpx;
		height: 32rpx;
		margin-right: 15rpx;
		opacity: 0.6;
	}
	
	.search-input {
		flex: 1;
		font-size: 28rpx;
		color: #333;
		background: transparent;
		border: none;
		outline: none;
	}
	
	.search-input::placeholder {
		color: #999;
	}
	
	.clear-btn {
		width: 40rpx;
		height: 40rpx;
		border-radius: 50%;
		background-color: #ccc;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-left: 10rpx;
		transition: all 0.3s ease;
	}
	
	.clear-btn:active {
		background-color: #999;
		transform: scale(0.9);
	}
	
	.clear-text {
		font-size: 24rpx;
		color: #fff;
		font-weight: bold;
	}
	
	/* 导出按钮容器 */
	.export-container {
		display: flex;
		gap: 15rpx;
	}
	
	.export-btn {
		flex: 1;
		height: 70rpx;
		border-radius: 12rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 10rpx;
		transition: all 0.3s ease;
		position: relative;
		overflow: hidden;
	}
	
	.export-btn:active {
		transform: scale(0.95);
	}
	
	.search-export {
		background-color: #6c757d;
		box-shadow: 0 4rpx 15rpx rgba(108, 117, 125, 0.3);
	}
	
	.category-export {
		background-color: #6c757d;
		box-shadow: 0 4rpx 15rpx rgba(108, 117, 125, 0.3);
	}
	
	.export-icon {
		width: 32rpx;
		height: 32rpx;
		filter: brightness(0) invert(1);
	}
	
	.export-text {
		font-size: 26rpx;
		color: #fff;
		font-weight: 500;
		white-space: nowrap;
	}
	
	/* 按钮悬停效果 */
	.export-btn::before {
		content: '';
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
		transition: left 0.5s;
	}
	
	.export-btn:active::before {
		left: 100%;
	}
</style>
