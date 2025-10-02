<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-02 
 * @Description: 帮助与信息组件 - 包含联系客服、关于我们、意见反馈等功能
-->
<template>
	<view class="help-info">
		<view class="section-header">
			<text class="section-title">帮助与信息</text>
		</view>
		
		<view class="help-list">
			<view 
				class="help-item" 
				v-for="(item, index) in helpItems" 
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
					<view v-if="item.status" class="status-indicator" :class="item.statusClass">
						<text class="status-text">{{ item.status }}</text>
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
		name: 'HelpInfo',
		data() {
			return {
				helpItems: [
					{
						icon: '/static/icons/weixin.png',
						title: '联系客服',
						description: '7×24小时在线服务',
						status: '在线',
						statusClass: 'online',
						action: 'contact',
						iconClass: 'contact-icon'
					},
					{
						icon: '/static/icons/guanyu.png',
						title: '关于我们',
						description: '了解品牌故事',
						status: '',
						statusClass: '',
						action: 'about',
						iconClass: 'about-icon'
					},
					{
						icon: '/static/icons/fk.png',
						title: '意见反馈',
						description: '您的建议让我们更好',
						status: '',
						statusClass: '',
						action: 'feedback',
						iconClass: 'feedback-icon'
					}
				],
			}
		},
		methods: {
			// 处理帮助项目点击事件
			handleItemClick(item) {
				this.$emit('itemClick', item)
				
				switch(item.action) {
					case 'contact':
						this.showContactOptions()
						break
					case 'about':
						uni.navigateTo({
							url: '/pages/about/about'
						})
						break
					case 'feedback':
						this.showFeedbackOptions()
						break
				}
			},
			
			
			// 显示联系方式选项
			showContactOptions() {
				uni.showActionSheet({
					itemList: ['微信客服', '电话客服', '在线客服'],
					success: (res) => {
						switch(res.tapIndex) {
							case 0:
								this.contactWechat()
								break
							case 1:
								this.contactPhone()
								break
							case 2:
								this.contactOnline()
								break
						}
					}
				})
			},
			
			
			// 显示意见反馈选项
			showFeedbackOptions() {
				uni.showActionSheet({
					itemList: ['功能建议', '问题反馈', '体验改进'],
					success: (res) => {
						const feedbackTypes = ['suggestion', 'bug', 'experience']
						const feedbackType = feedbackTypes[res.tapIndex]
						
						// 跳转到意见反馈页面，传递反馈类型
						uni.navigateTo({
							url: `/pages/feedback/feedback?type=${feedbackType}`
						})
					}
				})
			}
		}
	}
</script>

<style scoped>
	.help-info {
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
	
	.help-list {
		margin-bottom: 0;
	}
	
	.help-item {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 30rpx 0;
		border-bottom: 1rpx solid #f0f0f0;
		transition: background-color 0.3s ease;
	}
	
	.help-item:last-child {
		border-bottom: none;
	}
	
	.help-item:active {
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
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.status-indicator.online {
		background-color: #e8f5e8;
		border: 1rpx solid #4cd964;
	}
	
	
	.status-text {
		font-size: 22rpx;
		font-weight: 600;
		line-height: 1;
	}
	
	.online .status-text {
		color: #4cd964;
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
	.contact-icon {
		background: linear-gradient(135deg, #e8f5e8, #f8f9fa);
		border: 1rpx solid #4cd964;
	}
	
	.about-icon {
		background: linear-gradient(135deg, #f0f8ff, #f8f9fa);
		border: 1rpx solid #007aff;
	}
	
	.feedback-icon {
		background: linear-gradient(135deg, #fff0f5, #f8f9fa);
		border: 1rpx solid #ff6b9d;
	}
</style>
