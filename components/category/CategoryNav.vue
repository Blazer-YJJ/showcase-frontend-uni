<template>
	<view class="category-nav">
		<!-- 全部款式 -->
		<view 
			class="nav-item all-styles"
			:class="{ active: selectedLevel1 === 'all' }"
			@click="handleAllStylesClick"
		>
			<view class="nav-content">
				<text class="nav-text">全部款式</text>
			</view>
		</view>
		
		<!-- 一级分类 -->
		<view 
			v-for="level1 in categories" 
			:key="level1.id"
			class="nav-item level1"
			:class="{ active: selectedLevel1 === level1.id && selectedLevel2 === null && selectedLevel3 === null }"
			@click="handleLevel1Click(level1)"
		>
			<view class="nav-content">
				<text class="nav-text">{{ level1.name }}</text>
				<text 
					v-if="level1.children && level1.children.length > 0"
					class="expand-icon"
					:class="{ expanded: expandedLevel1.includes(level1.id) }"
				>▶</text>
			</view>
			
			<!-- 二级分类 -->
			<view 
				v-if="level1.children && level1.children.length > 0 && expandedLevel1.includes(level1.id)"
				class="level2-container"
			>
				<view 
					v-for="level2 in level1.children" 
					:key="level2.id"
					class="nav-item level2"
					:class="{ active: selectedLevel2 === level2.id && selectedLevel3 === null }"
					@click.stop="handleLevel2Click(level2)"
				>
					<view class="nav-content">
						<text class="nav-text">{{ level2.name }}</text>
						<text 
							v-if="level2.children && level2.children.length > 0"
							class="expand-icon"
							:class="{ expanded: expandedLevel2.includes(level2.id) }"
						>▶</text>
					</view>
					
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'CategoryNav',
		props: {
			categories: {
				type: Array,
				default: () => []
			},
			selectedCategory: {
				type: Object,
				default: () => null
			}
		},
		data() {
			return {
				selectedLevel1: null,
				selectedLevel2: null,
				expandedLevel1: [],
				expandedLevel2: []
			}
		},
		watch: {
			selectedCategory: {
				handler(newVal) {
					console.log('CategoryNav selectedCategory changed:', newVal)
					if (newVal) {
						this.selectedLevel1 = newVal.level1Id
						this.selectedLevel2 = newVal.level2Id
						
						console.log('设置激活状态:', {
							level1: this.selectedLevel1,
							level2: this.selectedLevel2
						})
						
						// 只在必要时展开分类，不要强制展开
						// 让用户手动控制展开/折叠状态
					} else {
						// 如果没有选中分类，重置所有状态
						this.selectedLevel1 = null
						this.selectedLevel2 = null
					}
				},
				immediate: true
			}
		},
		methods: {
			// 处理全部款式点击
			handleAllStylesClick() {
				this.selectedLevel1 = 'all'
				this.selectedLevel2 = null
				this.selectedLevel3 = null
				
				// 清空所有展开状态
				this.expandedLevel1 = []
				this.expandedLevel2 = []
				
				this.$emit('categoryChange', {
					level1Id: 'all',
					level2Id: null,
					level3Id: null,
					level1Name: '全部款式',
					level2Name: '',
					level3Name: ''
				})
			},
			
			// 处理一级分类点击
			handleLevel1Click(category) {
				// 切换展开状态
				const index = this.expandedLevel1.indexOf(category.id)
				if (index > -1) {
					// 如果已经展开，则折叠
					this.expandedLevel1.splice(index, 1)
					// 清空下级展开状态
					this.expandedLevel2 = []
				} else {
					// 如果未展开，则展开
					this.expandedLevel1.push(category.id)
					// 清空下级展开状态
					this.expandedLevel2 = []
				}
				
				// 设置选中状态
				this.selectedLevel1 = category.id
				
				// 如果有二级分类，默认选中第一个二级分类
				let selectedLevel2Id = null
				let selectedLevel2Name = ''
				
				if (category.children && category.children.length > 0) {
					const firstLevel2 = category.children[0]
					selectedLevel2Id = firstLevel2.id
					selectedLevel2Name = firstLevel2.name
					this.selectedLevel2 = firstLevel2.id
				} else {
					this.selectedLevel2 = null
				}
				
				this.selectedLevel3 = null
				
				// 发送分类变化事件（无论展开还是折叠都发送，保持选中状态）
				this.$emit('categoryChange', {
					level1Id: category.id,
					level2Id: selectedLevel2Id,
					level3Id: null,
					level1Name: category.name,
					level2Name: selectedLevel2Name,
					level3Name: ''
				})
			},
			
			// 处理二级分类点击
			handleLevel2Click(category) {
				this.selectedLevel2 = category.id
				
				// 确保父级一级分类也展开
				const parentLevel1 = this.categories.find(item => 
					item.children && item.children.some(child => child.id === category.id)
				)
				
				if (parentLevel1 && !this.expandedLevel1.includes(parentLevel1.id)) {
					this.expandedLevel1.push(parentLevel1.id)
				}
				
				// 二级分类点击时，发送事件让父组件显示该二级分类下的所有商品
				this.$emit('categoryChange', {
					level1Id: parentLevel1 ? parentLevel1.id : null,
					level2Id: category.id,
					level3Id: null,
					level1Name: parentLevel1 ? parentLevel1.name : '',
					level2Name: category.name,
					level3Name: ''
				})
			}
		}
	}
</script>

<style scoped>
	.category-nav {
		position: fixed;
		top: 200rpx;
		left: 0;
		width: 200rpx;
		height: calc(100vh);
		background-color: #f8f9fa;
		border-right: 1rpx solid #e9ecef;
		overflow-y: auto;
		overflow-x: hidden;
		z-index: 999;
		box-sizing: border-box;
	}
	
	.nav-item {
		position: relative;
		cursor: pointer;
		transition: all 0.3s ease;
	}
	
	.nav-item.all-styles {
		border-bottom: 2rpx solid #d4af37;
		background-color: #fff;
	}
	
	.nav-item.level1 {
		border-bottom: 1rpx solid #e9ecef;
	}
	
	.nav-item.level2 {
		background-color: #fff;
		border-bottom: 1rpx solid #f0f0f0;
		margin-left: 20rpx;
		border-left: 3rpx solid #e9ecef;
	}
	
	
	.nav-content {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 20rpx 16rpx;
		min-height: 60rpx;
	}
	
	.nav-text {
		font-size: 26rpx;
		color: #333;
		flex: 1;
		line-height: 1.4;
	}
	
	
	.expand-icon {
		font-size: 20rpx;
		color: #999;
		transition: transform 0.3s ease;
		margin-left: 10rpx;
	}
	
	.expand-icon.expanded {
		transform: rotate(90deg);
	}
	
	.level2-container {
		background-color: #fff;
		border-left: 3rpx solid #e9ecef;
		margin-left: 20rpx;
	}
	
	
	/* 激活状态样式 */
	.nav-item.active {
		position: relative;
	}
	
	.nav-item.active .nav-text {
		color: #d4af37 !important; /* 金色文字 */
		font-weight: 600; /* 加粗 */
	}
	
	.nav-item.active .expand-icon {
		color: #d4af37 !important; /* 金色图标 */
	}
	
	/* 悬停效果 */
	.nav-item:not(.active):hover {
		background-color: #e9ecef;
	}
	
	.nav-item.all-styles:not(.active):hover {
		background-color: #f0f0f0;
	}
	
	.nav-item.level2:not(.active):hover {
		background-color: #f8f9fa;
	}
	
	/* 响应式布局 */
	@media screen and (max-width: 750rpx) {
		.category-nav {
			width: 160rpx; /* 小屏幕下调整宽度 */
		}
	}
</style>
