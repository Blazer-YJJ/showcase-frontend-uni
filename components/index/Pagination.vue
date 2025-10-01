<template>
	<view class="pagination">
		<!-- 上一页按钮 -->
		<view 
			class="nav-button prev-button" 
			:class="{ disabled: currentPage <= 1 }"
			@click="handlePrevPage"
		>
			<text class="nav-text">上一页</text>
		</view>
		
		<!-- 当前页显示 -->
		<view class="page-info">
			<text class="current-page-text">{{ currentPage }}</text>
			<text class="page-separator">/</text>
			<text class="total-pages-text">{{ totalPages }}</text>
		</view>
		
		<!-- 下一页按钮 -->
		<view 
			class="nav-button next-button" 
			:class="{ disabled: currentPage >= totalPages }"
			@click="handleNextPage"
		>
			<text class="nav-text">下一页</text>
		</view>
		
		<!-- 页数输入和跳转 -->
		<view class="page-input-container">
			<input 
				class="page-input" 
				type="number" 
				v-model="inputPage"
				:placeholder="currentPage.toString()"
				@confirm="handlePageJump"
				@blur="handlePageJump"
			/>
			<view class="jump-button" @click="handlePageJump">
				<text class="jump-text">跳转</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'Pagination',
		props: {
			currentPage: {
				type: Number,
				default: 1
			},
			totalPages: {
				type: Number,
				default: 5
			},
			maxVisiblePages: {
				type: Number,
				default: 5
			}
		},
		data() {
			return {
				inputPage: ''
			}
		},
		methods: {
			handlePrevPage() {
				if (this.currentPage > 1) {
					this.$emit('pageChange', this.currentPage - 1)
				}
			},
			handleNextPage() {
				if (this.currentPage < this.totalPages) {
					this.$emit('pageChange', this.currentPage + 1)
				}
			},
			handlePageJump() {
				const page = parseInt(this.inputPage)
				if (page && page >= 1 && page <= this.totalPages && page !== this.currentPage) {
					this.$emit('pageChange', page)
					this.inputPage = ''
				} else if (page && (page < 1 || page > this.totalPages)) {
					uni.showToast({
						title: `请输入1-${this.totalPages}之间的页数`,
						icon: 'none',
						duration: 2000
					})
					this.inputPage = ''
				}
			}
		}
	}
</script>

<style scoped>
	.pagination {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 8rpx;
		padding: 20rpx;
		background-color: #fff;
		border-top: 1rpx solid #eee;
		flex-wrap: wrap;
	}
	
	/* 页数信息显示 */
	.page-info {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 0 20rpx;
		min-width: 120rpx;
	}
	
	.current-page-text {
		font-size: 28rpx;
		font-weight: bold;
		color: #d4af37;
	}
	
	.page-separator {
		font-size: 24rpx;
		color: #999;
		margin: 0 8rpx;
	}
	
	.total-pages-text {
		font-size: 24rpx;
		color: #666;
	}
	
	/* 导航按钮样式 */
	.nav-button {
		min-width: 80rpx;
		height: 60rpx;
		padding: 0 16rpx;
		border-radius: 8rpx;
		background-color: #fff;
		border: 1rpx solid #ddd;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: all 0.3s ease;
	}
	
	.nav-button:active {
		background-color: #f5f5f5;
		transform: scale(0.95);
	}
	
	.nav-button.disabled {
		background-color: #f5f5f5;
		border-color: #e0e0e0;
		opacity: 0.5;
	}
	
	.nav-button.disabled:active {
		transform: none;
		background-color: #f5f5f5;
	}
	
	.nav-text {
		font-size: 24rpx;
		font-weight: 500;
		color: #333;
		white-space: nowrap;
	}
	
	.nav-button.disabled .nav-text {
		color: #999;
	}
	
	/* 页数输入容器 */
	.page-input-container {
		display: flex;
		align-items: center;
		margin-left: 20rpx;
		gap: 8rpx;
	}
	
	.page-input {
		width: 80rpx;
		height: 60rpx;
		border: 1rpx solid #ddd;
		border-radius: 8rpx;
		text-align: center;
		font-size: 24rpx;
		background-color: #fff;
	}
	
	.page-input:focus {
		border-color: #d4af37;
		outline: none;
	}
	
	.jump-button {
		height: 60rpx;
		padding: 0 16rpx;
		background-color: #d4af37;
		border-radius: 8rpx;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: all 0.3s ease;
	}
	
	.jump-button:active {
		background-color: #d4af37;
		transform: scale(0.95);
	}
	
	.jump-text {
		font-size: 24rpx;
		color: #fff;
		font-weight: 500;
	}
</style>
