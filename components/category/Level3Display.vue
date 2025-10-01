<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-01-27 10:00:00
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-01-27 10:00:00
 * @FilePath: \showcase-frontend-uni\components\category\Level3Display.vue
 * @Description: 三级分类显示组件
-->
<template>
	<view v-if="level3Categories.length > 0" class="level3-display">
		<scroll-view 
			class="level3-scroll" 
			scroll-x="true" 
			show-scrollbar="false"
			:scroll-left="scrollLeft"
		>
			<view class="level3-list">
				<view 
					v-for="level3 in level3Categories" 
					:key="level3.id"
					class="level3-item"
					:class="{ active: selectedLevel3 === level3.id }"
					@click="handleLevel3Click(level3)"
				>
					<text class="level3-text">{{ level3.name }}</text>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		name: 'Level3Display',
		props: {
			level3Categories: {
				type: Array,
				default: () => []
			},
			level2Name: {
				type: String,
				default: ''
			},
			selectedLevel3: {
				type: [String, Number],
				default: null
			}
		},
		data() {
			return {
				scrollLeft: 0
			}
		},
		watch: {
			selectedLevel3: {
				handler(newVal) {
					// 当选中项改变时，自动滚动到选中项
					this.scrollToSelected()
				},
				immediate: true
			}
		},
		methods: {
			handleLevel3Click(level3) {
				this.$emit('level3Click', level3)
			},
			
			// 滚动到选中项
			scrollToSelected() {
				if (!this.selectedLevel3 || this.level3Categories.length === 0) {
					return
				}
				
				// 计算选中项的位置
				const selectedIndex = this.level3Categories.findIndex(item => item.id === this.selectedLevel3)
				if (selectedIndex !== -1) {
					// 每个项目宽度 + 间距，这里需要根据实际样式调整
					const itemWidth = 120 // 预估每个项目宽度
					const scrollPosition = selectedIndex * itemWidth
					this.scrollLeft = scrollPosition
				}
			}
		}
	}
</script>

<style scoped>
	.level3-display {
		background: transparent; /* 移除背景色，因为父容器已有背景 */
		border-radius: 0; /* 移除圆角 */
		margin-bottom: 0; /* 移除底部边距 */
		box-shadow: none; /* 移除阴影 */
		overflow: hidden;
	}
	
	.level3-scroll {
		width: 100%;
		white-space: nowrap;
	}
	
	.level3-list {
		display: flex;
		flex-direction: row;
		padding: 20rpx 24rpx;
		gap: 16rpx;
		white-space: nowrap;
	}
	
	.level3-item {
		background: #f8f9fa;
		border: 2rpx solid #e9ecef;
		border-radius: 8rpx;
		padding: 12rpx 20rpx;
		transition: all 0.3s ease;
		cursor: pointer;
		flex-shrink: 0; /* 防止项目被压缩 */
		min-width: 100rpx; /* 设置最小宽度 */
		text-align: center;
	}
	
	.level3-item:hover {
		background: #e9ecef;
		border-color: #d4af37;
		transform: translateY(-2rpx);
		box-shadow: 0 4rpx 12rpx rgba(212, 175, 55, 0.2);
	}
	
	.level3-item.active {
		border-color: #d4af37;
		box-shadow: 0 4rpx 12rpx rgba(212, 175, 55, 0.2);
	}
	
	.level3-item.active .level3-text {
		color: #d4af37;
		font-weight: 600;
	}
	
	.level3-text {
		color: #495057;
		font-size: 28rpx;
		transition: color 0.3s ease;
	}
</style>
