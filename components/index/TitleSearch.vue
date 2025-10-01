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
	</view>
</template>

<script>
	export default {
		name: 'TitleSearch',
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
				searchValue: ''
			}
		},
		methods: {
			handleSearch() {
				if (this.searchValue.trim()) {
					this.$emit('search', this.searchValue.trim())
				}
			},
			handleInputChange(e) {
				this.searchValue = e.detail.value
			},
			handleImageSearch() {
				this.$emit('imageSearch')
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
		background-color: #f8f9fa;
		border: 1rpx solid #e9ecef;
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
