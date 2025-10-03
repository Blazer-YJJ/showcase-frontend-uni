<template>
	<view class="category-page">
		<!-- 左侧分类导航 - 固定定位 -->
		<CategoryNav 
			:categories="categories"
			:selectedCategory="selectedCategory"
			@categoryChange="handleCategoryChange"
		/>
		
		<!-- 顶部搜索和导出 - 固定在页面顶部 -->
		<CategoryHeader 
			:currentCategory="selectedCategory"
			@search="handleSearch"
			@searchExport="handleSearchExport"
			@categoryExport="handleCategoryExport"
		/>
		
		<!-- 右侧内容区域 -->
		<view class="right-content">
			<!-- 固定三级分类显示 - 只在有三级分类时显示 -->
			<view 
				v-if="hasLevel3Categories()" 
				class="fixed-level3"
			>
				<Level3Display 
					:level3Categories="getCurrentLevel3Categories()"
					:level2Name="selectedCategory.level2Name"
					:selectedLevel3="selectedCategory.level3Id"
					@level3Click="handleLevel3Click"
				/>
			</view>
			
			<!-- 全部标签 - 只在没有三级分类时显示 -->
			<view 
				v-if="!hasLevel3Categories()" 
				class="all-tag-container"
			>
				<view class="all-tag">
					<text class="all-tag-text">全部</text>
				</view>
			</view>
			
			<!-- 商品网格区域 - 可滚动 -->
			<view class="products-container">
				<ProductGrid 
					:products="filteredProducts"
					:loading="loading"
					:columns="2"
					@productClick="handleProductClick"
				/>
				
				<!-- 分页器 -->
				<view class="pagination-container">
					<Pagination 
						v-if="!loading && filteredProducts.length > 0"
						:currentPage="currentPage"
						:totalPages="totalPages"
						@pageChange="handlePageChange"
					/>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import CategoryNav from '../../components/category/CategoryNav.vue'
	import CategoryHeader from '../../components/category/CategoryHeader.vue'
	import ProductGrid from '../../components/category/ProductGrid.vue'
	import Level3Display from '../../components/category/Level3Display.vue'
	import Pagination from '../../components/index/Pagination.vue'
	
	export default {
		name: 'CategoryPage',
		components: {
			CategoryNav,
			CategoryHeader,
			ProductGrid,
			Level3Display,
			Pagination
		},
		data() {
			return {
				// 分类数据
				categories: [
					{
						id: 1,
						name: '黄金首饰',
						children: [
							{
								id: 11,
								name: '项链吊坠',
								children: [
									{ id: 111, name: '黄金项链' },
									{ id: 112, name: '黄金吊坠' },
									{ id: 113, name: '黄金套链' }
								]
							},
							{
								id: 12,
								name: '戒指手镯',
								children: [
									{ id: 121, name: '黄金戒指' },
									{ id: 122, name: '黄金手镯' },
									{ id: 123, name: '黄金手链' }
								]
							}
						]
					},
					{
						id: 2,
						name: '耳饰系列',
						children: [
							{
								id: 21,
								name: '耳环耳钉',
								children: [
									{ id: 211, name: '黄金耳环' },
									{ id: 212, name: '黄金耳钉' },
									{ id: 213, name: '黄金耳线' }
								]
							},
							{
								id: 22,
								name: '耳夹耳钩',
								children: [
									{ id: 221, name: '黄金耳夹' },
									{ id: 222, name: '黄金耳钩' },
									{ id: 223, name: '黄金耳扣' }
								]
							}
						]
					},
					{
						id: 3,
						name: '婚庆珠宝',
						children: [
							{
								id: 31,
								name: '婚戒对戒',
								children: [
									{ id: 311, name: '黄金婚戒' },
									{ id: 312, name: '黄金对戒' },
									{ id: 313, name: '黄金情侣戒' }
								]
							},
							{
								id: 32,
								name: '婚庆套装',
								children: [
									{ id: 321, name: '黄金三金套' },
									{ id: 322, name: '黄金五件套' },
									{ id: 323, name: '黄金婚庆套' }
								]
							}
						]
					}
				],
				
				// 当前选中的分类
				selectedCategory: {
					level1Id: 'all',
					level2Id: null,
					level3Id: null,
					level1Name: '全部款式',
					level2Name: '',
					level3Name: ''
				},
				
				// 商品数据
				allProducts: [
					{
						id: 1,
						imagePlaceholder: '/static/images/001.png',
						title: '足金999项链',
						weight: '18K金 15g 经典款',
						price: 8999,
						tag: '热销',
						categoryId: 111
					},
					{
						id: 2,
						imagePlaceholder: '/static/images/002.png',
						title: '黄金钻石戒指',
						weight: '足金999 2克拉 经典款',
						price: 19999,
						tag: '新品',
						categoryId: 121
					},
					{
						id: 3,
						imagePlaceholder: '/static/images/003.png',
						title: '黄金耳环套装',
						weight: '足金999 8g 时尚款',
						price: 3899,
						tag: '经典',
						categoryId: 211
					},
					{
						id: 4,
						imagePlaceholder: '/static/images/004.png',
						title: '黄金手镯',
						weight: '足金999 25g 古法工艺',
						price: 12999,
						tag: '时尚',
						categoryId: 122
					},
					{
						id: 5,
						imagePlaceholder: '/static/images/001.png',
						title: '黄金婚戒对戒',
						weight: '足金999 男女款 经典款',
						price: 15999,
						tag: '舒适',
						categoryId: 311
					},
					{
						id: 6,
						imagePlaceholder: '/static/images/002.png',
						title: '黄金三金套装',
						weight: '足金999 项链+戒指+耳环',
						price: 25999,
						tag: '高效',
						categoryId: 321
					},
					{
						id: 7,
						imagePlaceholder: '/static/images/003.png',
						title: '黄金吊坠',
						weight: '足金999 12g 生肖款',
						price: 5999,
						tag: '限量',
						categoryId: 112
					},
					{
						id: 8,
						imagePlaceholder: '/static/images/004.png',
						title: '黄金耳钉',
						weight: '足金999 3g 简约款',
						price: 1999,
						tag: '简约',
						categoryId: 212
					},
					{
						id: 9,
						imagePlaceholder: '/static/images/001.png',
						title: '黄金手链',
						weight: '足金999 10g 编织款',
						price: 4999,
						tag: '精致',
						categoryId: 123
					},
					{
						id: 10,
						imagePlaceholder: '/static/images/002.png',
						title: '黄金五件套',
						weight: '足金999 项链+戒指+耳环+手镯+吊坠',
						price: 35999,
						tag: '豪华',
						categoryId: 322
					},
					{
						id: 11,
						imagePlaceholder: '/static/images/003.png',
						title: '黄金龙凤手镯',
						weight: '足金999 30g 传统工艺',
						price: 15999,
						tag: '传统',
						categoryId: 122
					},
					{
						id: 12,
						imagePlaceholder: '/static/images/004.png',
						title: '黄金花丝耳环',
						weight: '足金999 6g 花丝工艺',
						price: 4599,
						tag: '工艺',
						categoryId: 211
					},
					{
						id: 13,
						imagePlaceholder: '/static/images/001.png',
						title: '黄金平安扣',
						weight: '足金999 15g 平安寓意',
						price: 7999,
						tag: '平安',
						categoryId: 112
					},
					{
						id: 14,
						imagePlaceholder: '/static/images/002.png',
						title: '黄金转运珠',
						weight: '足金999 5g 转运系列',
						price: 2999,
						tag: '转运',
						categoryId: 111
					},
					{
						id: 15,
						imagePlaceholder: '/static/images/003.png',
						title: '黄金古法手链',
						weight: '足金999 12g 古法工艺',
						price: 6999,
						tag: '古法',
						categoryId: 123
					},
					{
						id: 16,
						imagePlaceholder: '/static/images/004.png',
						title: '黄金生肖吊坠',
						weight: '足金999 8g 生肖系列',
						price: 3999,
						tag: '生肖',
						categoryId: 112
					},
					{
						id: 17,
						imagePlaceholder: '/static/images/001.png',
						title: '黄金情侣对戒',
						weight: '足金999 男女款 情侣系列',
						price: 11999,
						tag: '情侣',
						categoryId: 311
					},
					{
						id: 18,
						imagePlaceholder: '/static/images/002.png',
						title: '黄金花丝项链',
						weight: '足金999 20g 花丝工艺',
						price: 12999,
						tag: '花丝',
						categoryId: 111
					},
					{
						id: 19,
						imagePlaceholder: '/static/images/003.png',
						title: '黄金耳线',
						weight: '足金999 4g 时尚耳线',
						price: 2599,
						tag: '时尚',
						categoryId: 213
					},
					{
						id: 20,
						imagePlaceholder: '/static/images/004.png',
						title: '黄金婚庆六件套',
						weight: '足金999 全套婚庆珠宝',
						price: 45999,
						tag: '婚庆',
						categoryId: 322
					}
				],
				
				// 过滤后的商品
				filteredProducts: [],
				
				// 搜索关键词
				searchKeyword: '',
				
				// 分页相关
				currentPage: 1,
				pageSize: 20,
				totalItems: 20,
				totalPages: 1,
				
				// 加载状态
				loading: false
			}
		},
		onLoad() {
			console.log('分类页加载')
			this.initData()
		},
		methods: {
			// 初始化数据
			initData() {
				this.filteredProducts = [...this.allProducts]
				this.totalItems = this.filteredProducts.length
				this.totalPages = Math.ceil(this.totalItems / this.pageSize)
			},
			
			// 处理分类变化
			handleCategoryChange(category) {
				console.log('分类变化:', category)
				this.selectedCategory = category
				this.currentPage = 1
				this.filterProducts()
			},
			
			// 处理搜索
			handleSearch(keyword) {
				console.log('搜索关键词:', keyword)
				this.searchKeyword = keyword
				this.currentPage = 1
				this.filterProducts()
			},
			
			// 过滤商品
			filterProducts() {
				this.loading = true
				
				// 模拟异步加载
				setTimeout(() => {
					let products = [...this.allProducts]
					
					// 按分类过滤
					if (this.selectedCategory) {
						// 全部款式 - 显示所有商品
						if (this.selectedCategory.level1Id === 'all') {
							// 不进行任何过滤，显示所有商品
						}
						// 三级分类 - 显示特定三级分类的商品
						else if (this.selectedCategory.level3Id) {
							products = products.filter(p => p.categoryId === this.selectedCategory.level3Id)
						}
						// 二级分类 - 显示该二级分类下所有三级分类的商品
						else if (this.selectedCategory.level2Id) {
							// 找到该二级分类下的所有三级分类ID
							const level2Id = this.selectedCategory.level2Id
							const level2Category = this.findCategoryById(level2Id, 2)
							
							if (level2Category && level2Category.children) {
								const level3Ids = level2Category.children.map(child => child.id)
								products = products.filter(p => level3Ids.includes(p.categoryId))
							}
						}
						// 一级分类 - 显示该一级分类下所有商品
						else if (this.selectedCategory.level1Id) {
							// 找到该一级分类下的所有三级分类ID
							const level1Id = this.selectedCategory.level1Id
							const level1Category = this.findCategoryById(level1Id, 1)
							
							if (level1Category) {
								const allLevel3Ids = this.getAllLevel3Ids(level1Category)
								products = products.filter(p => allLevel3Ids.includes(p.categoryId))
							}
						}
					}
					
					// 按搜索关键词过滤
					if (this.searchKeyword) {
						products = products.filter(p => 
							p.title.toLowerCase().includes(this.searchKeyword.toLowerCase()) ||
							p.weight.toLowerCase().includes(this.searchKeyword.toLowerCase())
						)
					}
					
					this.filteredProducts = products
					this.totalItems = products.length
					this.totalPages = Math.ceil(this.totalItems / this.pageSize)
					this.loading = false
				}, 500)
			},
			
			// 处理商品点击
			handleProductClick(product) {
				console.log('商品点击:', product)
				uni.showToast({
					title: `点击了${product.title}`,
					icon: 'none',
					duration: 2000
				})
			},
			
			// 处理搜索导出PDF
			handleSearchExport(data) {
				console.log('搜索导出PDF:', data)
				uni.showToast({
					title: '搜索PDF导出成功',
					icon: 'success',
					duration: 2000
				})
			},
			
			// 处理分类导出PDF
			handleCategoryExport(data) {
				console.log('分类导出PDF:', data)
				uni.showToast({
					title: '分类PDF导出成功',
					icon: 'success',
					duration: 2000
				})
			},
			
			// 处理页码变化
			handlePageChange(page) {
				console.log('页码变化:', page)
				this.currentPage = page
				// 这里可以添加分页加载逻辑
			},
			
			// 根据ID查找分类
			findCategoryById(id, level) {
				if (level === 1) {
					return this.categories.find(cat => cat.id === id)
				} else if (level === 2) {
					for (const level1 of this.categories) {
						if (level1.children) {
							const found = level1.children.find(child => child.id === id)
							if (found) return found
						}
					}
				} else if (level === 3) {
					for (const level1 of this.categories) {
						if (level1.children) {
							for (const level2 of level1.children) {
								if (level2.children) {
									const found = level2.children.find(child => child.id === id)
									if (found) return found
								}
							}
						}
					}
				}
				return null
			},
			
			// 获取一级分类下所有三级分类的ID
			getAllLevel3Ids(level1Category) {
				const level3Ids = []
				
				if (level1Category.children) {
					level1Category.children.forEach(level2 => {
						if (level2.children) {
							level2.children.forEach(level3 => {
								level3Ids.push(level3.id)
							})
						}
					})
				}
				
				return level3Ids
			},
			
			// 获取当前分类下的三级分类
			getCurrentLevel3Categories() {
				if (!this.selectedCategory || this.selectedCategory.level1Id === 'all') {
					return []
				}
				
				// 如果选中了三级分类，返回该三级分类所在二级分类下的所有三级分类
				if (this.selectedCategory.level3Id) {
					const level2Category = this.findCategoryById(this.selectedCategory.level2Id, 2)
					return level2Category && level2Category.children ? level2Category.children : []
				}
				
				// 如果选中了二级分类，返回该二级分类下的所有三级分类
				if (this.selectedCategory.level2Id) {
					const level2Category = this.findCategoryById(this.selectedCategory.level2Id, 2)
					return level2Category && level2Category.children ? level2Category.children : []
				}
				
				// 如果只选中了一级分类，返回该一级分类下所有二级分类的三级分类
				if (this.selectedCategory.level1Id) {
					const level1Category = this.findCategoryById(this.selectedCategory.level1Id, 1)
					if (level1Category && level1Category.children) {
						const allLevel3Categories = []
						level1Category.children.forEach(level2 => {
							if (level2.children) {
								allLevel3Categories.push(...level2.children)
							}
						})
						return allLevel3Categories
					}
				}
				
				return []
			},
			
			// 判断是否有三级分类数据
			hasLevel3Categories() {
				const level3Categories = this.getCurrentLevel3Categories()
				return level3Categories && level3Categories.length > 0
			},
			
			// 处理三级分类点击
			handleLevel3Click(level3) {
				console.log('三级分类点击:', level3)
				
				// 更新选中的分类
				this.selectedCategory = {
					level1Id: this.selectedCategory.level1Id,
					level2Id: this.selectedCategory.level2Id,
					level3Id: level3.id,
					level1Name: this.selectedCategory.level1Name,
					level2Name: this.selectedCategory.level2Name,
					level3Name: level3.name
				}
				
				// 触发分类变化事件
				this.$emit('categoryChange', this.selectedCategory)
				
				// 重新过滤商品
				this.filterProducts()
			},
			
			// 处理页码变化
			handlePageChange(page) {
				console.log('页码变化:', page)
				this.currentPage = page
				// 这里可以添加分页加载逻辑
			}
		}
	}
</script>

<style scoped>
	.category-page {
		position: relative;
		height: 100vh;
		background-color: #f8f9fa;
		padding-top: 200rpx; /* 为固定头部预留空间 */
		padding-left: 200rpx; /* 为固定侧边栏预留空间 */
	}
	
	/* 右侧内容区域 */
	.right-content {
		position: relative;
		background-color: #f8f9fa;
		height: calc(100vh - 200rpx); /* 减去固定头部的高度 */
		width: calc(100vw - 200rpx); /* 减去固定侧边栏的宽度 */
		display: flex;
		flex-direction: column;
	}
	
	/* 固定三级分类 */
	.fixed-level3 {
		position: fixed;
		top: 200rpx; /* 在固定头部下方 */
		left: 200rpx; /* 在固定侧边栏右侧 */
		right: 0;
		background: #fff;
		box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
		z-index: 99;
		padding: 0 20rpx;
	}
	
	/* 全部标签容器 */
	.all-tag-container {
		position: fixed;
		top: 200rpx; /* 在固定头部下方 */
		left: 200rpx; /* 在固定侧边栏右侧 */
		right: 0;
		background: #fff;
		box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
		z-index: 99;
		padding: 20rpx;
	}
	
	/* 全部标签 */
	.all-tag {
		display: inline-block;
		padding: 12rpx 20rpx;
		background: #f8f9fa;
		border: 2rpx solid #e9ecef;
		border-radius: 8rpx;
		transition: all 0.3s ease;
		cursor: pointer;
		min-width: 100rpx;
		text-align: center;
	}
	
	.all-tag:hover {
		background: #e9ecef;
		border-color: #d4af37;
		transform: translateY(-2rpx);
		box-shadow: 0 4rpx 12rpx rgba(212, 175, 55, 0.2);
	}
	
	.all-tag-text {
		color: #495057;
		font-size: 28rpx;
		font-weight: 500;
		transition: color 0.3s ease;
	}
	
	/* 商品容器区域 */
	.products-container {
		flex: 1;
		min-height: 0; /* 允许flex子项收缩 */
		position: relative;
		display: flex;
		flex-direction: column;
		margin-top: 120rpx;
	}
	
	/* 分页器容器 */
	.pagination-container {
		flex-shrink: 0;
		padding: 20rpx;
		background: #fff;
		border-top: 1rpx solid #f0f0f0;
	}
	
	/* 响应式布局 */
	@media screen and (max-width: 750rpx) {
		.category-page {
			padding-left: 160rpx; /* 小屏幕下调整侧边栏宽度 */
		}
		
		.right-content {
			width: calc(100vw - 160rpx); /* 小屏幕下调整内容区域宽度 */
		}
		
		.fixed-level3 {
			left: 160rpx; /* 小屏幕下调整固定三级分类位置 */
		}
		
		.all-tag-container {
			left: 160rpx; /* 小屏幕下调整全部标签位置 */
		}
	}
</style>
