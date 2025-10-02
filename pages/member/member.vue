<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-02 
 * @Description: 会员中心页面 - 整合所有会员相关功能组件
-->
<template>
	<view class="member-page">
		<!-- 会员卡片 -->
		<MemberCard 
			:userInfo="userInfo" 
			@avatarClick="handleAvatarClick"
		/>
		
		<!-- 交易管理 -->
		<TransactionManagement 
			:orderStats="orderStats"
			@itemClick="handleTransactionClick"
		/>
		
		<!-- 个人中心与设置 -->
		<PersonalSettings 
			:userSettings="userSettings"
			@itemClick="handleSettingsClick"
		/>
		
		<!-- 帮助与信息 -->
		<HelpInfo 
			@itemClick="handleHelpClick"
			@contactClick="handleContactClick"
		/>
		
	</view>
</template>

<script>
	// 导入组件
	import MemberCard from '@/components/member/MemberCard.vue'
	import TransactionManagement from '@/components/member/TransactionManagement.vue'
	import PersonalSettings from '@/components/member/PersonalSettings.vue'
	import HelpInfo from '@/components/member/HelpInfo.vue'
	
	export default {
		name: 'MemberPage',
		components: {
			MemberCard,
			TransactionManagement,
			PersonalSettings,
			HelpInfo
		},
		data() {
			return {
				// 用户基本信息
				userInfo: {
					avatar: '/static/images/001.png', // 默认头像
					name: '尊贵用户',
					account: 'user@example.com',
					level: '黄金会员'
				},
				
				// 订单统计数据
				orderStats: {
					pendingOrders: 2,    // 待处理订单
					afterSales: 1,       // 售后订单
					favorites: 15        // 收藏数量
				},
				
				// 用户设置信息
				userSettings: {
					profileComplete: '85%',   // 资料完成度
					addressCount: 3,          // 地址数量
					notificationEnabled: true // 通知开启状态
				},
				
			}
		},
		
		onLoad() {
			console.log('会员中心页加载')
			this.initMemberData()
		},
		
		onShow() {
			// 页面显示时刷新数据
			this.refreshMemberData()
		},
		
		methods: {
			// 初始化会员数据
			async initMemberData() {
				try {
					// 获取用户信息
					await this.getUserInfo()
					// 获取订单统计
					await this.getOrderStats()
					// 获取用户设置
					await this.getUserSettings()
				} catch (error) {
					console.error('初始化会员数据失败:', error)
					uni.showToast({
						title: '数据加载失败',
						icon: 'none'
					})
				}
			},
			
			// 刷新会员数据
			async refreshMemberData() {
				// 刷新关键数据
				await this.getOrderStats()
				await this.getUserSettings()
			},
			
			// 获取用户信息
			async getUserInfo() {
				// 模拟API调用
				// const userInfo = await api.getUserInfo()
				// this.userInfo = userInfo
				
				// 临时使用本地存储的用户信息
				const storedUserInfo = uni.getStorageSync('userInfo')
				if (storedUserInfo) {
					this.userInfo = { ...this.userInfo, ...storedUserInfo }
				}
			},
			
			// 获取订单统计
			async getOrderStats() {
				// 模拟API调用
				// const stats = await api.getOrderStats()
				// this.orderStats = stats
				
				// 临时模拟数据
				this.orderStats = {
					pendingOrders: Math.floor(Math.random() * 5),
					afterSales: Math.floor(Math.random() * 3),
					favorites: Math.floor(Math.random() * 20) + 10
				}
			},
			
			// 获取用户设置
			async getUserSettings() {
				// 模拟API调用
				// const settings = await api.getUserSettings()
				// this.userSettings = settings
				
				// 临时使用本地存储
				const storedSettings = uni.getStorageSync('userSettings')
				if (storedSettings) {
					this.userSettings = { ...this.userSettings, ...storedSettings }
				}
			},
			
			
			// 处理头像点击事件
			handleAvatarClick() {
				uni.navigateTo({
					url: '/pages/profile/profile'
				})
			},
			
			// 处理交易管理项目点击
			handleTransactionClick(item) {
				console.log('交易管理点击:', item)
				// 具体的跳转逻辑已在组件内部处理
			},
			
			// 处理设置项目点击
			handleSettingsClick(item) {
				console.log('设置项目点击:', item)
				// 具体的跳转逻辑已在组件内部处理
			},
			
			// 处理帮助项目点击
			handleHelpClick(item) {
				console.log('帮助项目点击:', item)
				// 具体的跳转逻辑已在组件内部处理
			},
			
			// 处理联系方式点击
			handleContactClick(method) {
				console.log('联系方式点击:', method)
				// 具体的联系逻辑已在组件内部处理
			},
			
		}
	}
</script>

<style scoped>
	.member-page {
		min-height: 100vh;
		background-color: #f8f8f8;
		padding-bottom: 40rpx;
	}
	
	/* 确保页面滚动流畅 */
	.member-page {
		-webkit-overflow-scrolling: touch;
	}
</style>
