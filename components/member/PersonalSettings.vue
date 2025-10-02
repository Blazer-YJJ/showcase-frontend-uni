<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-02 
 * @Description: 个人中心与设置组件 - 包含我的资料、我的地址等基础功能
-->
<template>
	<view class="personal-settings">
		<view class="section-header">
			<text class="section-title">个人中心与设置</text>
		</view>
		
		<view class="settings-list">
			<view 
				class="settings-item" 
				v-for="(item, index) in settingsItems" 
				:key="index"
				@click="handleItemClick(item)"
			>
				<view class="item-left">
					<view class="item-icon-container" :class="item.iconClass">
						<image :src="item.icon" class="item-icon" mode="aspectFit" />
					</view>
					<view class="item-info">
						<text class="item-title">{{ item.title }}</text>
						<text class="item-desc">{{ item.description }}</text>
					</view>
				</view>
				
				<view class="item-right">
					<view v-if="item.rightText" class="status-indicator">
						<text class="status-text">{{ item.rightText }}</text>
					</view>
					<view class="arrow-wrapper">
						<text class="arrow-icon">›</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'PersonalSettings',
		props: {
			// 用户设置信息
			userSettings: {
				type: Object,
				default: () => ({
					profileComplete: '80%',
					addressCount: 2,
					notificationEnabled: true
				})
			}
		},
		data() {
			return {
				settingsItems: [
					{
						icon: '/static/icons/zl.png',
						title: '我的资料',
						description: '完善个人信息',
						rightText: '85%',
						action: 'profile',
						iconClass: 'profile-icon'
					},
					{
						icon: '/static/icons/dzgl.png',
						title: '我的地址',
						description: '管理收货地址',
						rightText: '3个',
						action: 'address',
						iconClass: 'address-icon'
					},
				]
			}
		},
		watch: {
			// 监听用户设置变化，更新显示文本
			userSettings: {
				handler(newSettings) {
					this.updateSettingsDisplay(newSettings)
				},
				immediate: true,
				deep: true
			}
		},
		methods: {
			// 更新设置显示文本
			updateSettingsDisplay(settings) {
				// 更新资料完成度
				const profileItem = this.settingsItems.find(item => item.action === 'profile')
				if (profileItem) {
					profileItem.rightText = settings.profileComplete || '0%'
				}
				
				// 更新地址数量
				const addressItem = this.settingsItems.find(item => item.action === 'address')
				if (addressItem) {
					addressItem.rightText = `${settings.addressCount || 0}个`
				}
			},
			
			// 处理项目点击事件
			handleItemClick(item) {
				this.$emit('itemClick', item)
				
				// 根据不同的操作类型进行页面跳转
				switch(item.action) {
					case 'profile':
						uni.navigateTo({
							url: '/pages/profile/profile'
						})
						break
					case 'address':
						uni.navigateTo({
							url: '/pages/address/addressList'
						})
						break
				}
			}
		}
	}
</script>

<style scoped>
	.personal-settings {
		margin: 20rpx;
		background-color: #fff;
		border-radius: 12rpx;
		padding: 30rpx;
		box-shadow: 0 2rpx 8rpx rgba(0,0,0,0.1);
	}
	
	.section-header {
		margin-bottom: 30rpx;
	}
	
	.section-title {
		font-size: 32rpx;
		font-weight: bold;
		color: #333;
	}
	
	.settings-list {
		margin-bottom: 0;
	}
	
	.settings-item {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 30rpx 0;
		border-bottom: 1rpx solid #f0f0f0;
		transition: background-color 0.3s ease;
	}
	
	.settings-item:last-child {
		border-bottom: none;
	}
	
	.settings-item:active {
		background-color: #f8f9fa;
	}
	
	.item-left {
		display: flex;
		align-items: center;
		flex: 1;
	}
	
	.item-icon-container {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-right: 30rpx;
	}
	
	.item-icon {
		width: 40rpx;
		height: 40rpx;
	}
	
	.item-info {
		display: flex;
		flex-direction: column;
		flex: 1;
	}
	
	.item-title {
		font-size: 30rpx;
		color: #333;
		font-weight: 600;
		margin-bottom: 6rpx;
	}
	
	.item-desc {
		font-size: 24rpx;
		color: #999;
	}
	
	.item-right {
		display: flex;
		align-items: center;
	}
	
	.status-indicator {
		padding: 4rpx 12rpx;
		border-radius: 12rpx;
		margin-right: 10rpx;
		background-color: #f0f0f0;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.status-text {
		font-size: 22rpx;
		font-weight: 600;
		color: #666;
		line-height: 1;
	}
	
	.arrow-wrapper {
		width: 40rpx;
		height: 40rpx;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.arrow-icon {
		font-size: 32rpx;
		color: #ccc;
		font-weight: bold;
	}
	
	/* 不同图标的背景颜色 */
	.profile-icon {
		background: linear-gradient(135deg, #e8f5e8, #f8f9fa);
		border: 1rpx solid #4cd964;
	}
	
	.address-icon {
		background: linear-gradient(135deg, #fff9e6, #f8f9fa);
		border: 1rpx solid #d4af37;
	}
	
</style>
