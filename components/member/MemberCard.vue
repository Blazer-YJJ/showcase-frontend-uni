<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-02 
 * @Description: 会员卡片组件 - 显示用户头像、欢迎语、名称、登录账号和权益信息
-->
<template>
	<view class="member-card">
		<!-- 背景装饰 -->
		<view class="card-bg-decoration"></view>
		
		<!-- 用户信息区域 -->
		<view class="user-info-section">
			<view class="avatar-container">
				<image 
					:src="userInfo.avatar || '/static/images/default-avatar.png'" 
					class="user-avatar" 
					mode="aspectFill"
				/>
				<view class="avatar-border"></view>
			</view>
			
			<view class="user-details">
				<view class="welcome-text">{{ welcomeMessage }}</view>
				<view class="user-name">{{ userInfo.name || '未设置昵称' }}</view>
				<view class="user-account">{{ userInfo.account || '未绑定账号' }}</view>
			</view>
		</view>
		
		
		<!-- 会员等级标识 -->
		<view class="member-level">
			<text class="level-text">{{ userInfo.level || '普通会员' }}</text>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'MemberCard',
		props: {
			// 用户信息
			userInfo: {
				type: Object,
				default: () => ({
					avatar: '',
					name: '',
					account: '',
					level: '普通会员'
				})
			}
		},
		data() {
			return {
			}
		},
		computed: {
			// 欢迎语根据时间动态变化
			welcomeMessage() {
				const hour = new Date().getHours()
				if (hour < 6) {
					return '夜深了，注意休息'
				} else if (hour < 12) {
					return '早上好'
				} else if (hour < 18) {
					return '下午好'
				} else {
					return '晚上好'
				}
			}
		},
		methods: {
			// 处理头像点击事件
			handleAvatarClick() {
				this.$emit('avatarClick')
			}
		}
	}
</script>

<style scoped>
	.member-card {
		position: relative;
		margin: 20rpx;
		background: linear-gradient(135deg, #6c757d 0%, #5a6268 50%, #495057 100%);
		border-radius: 16rpx;
		padding: 40rpx 30rpx;
		box-shadow: 0 8rpx 24rpx rgba(108, 117, 125, 0.3);
		overflow: hidden;
	}
	
	.card-bg-decoration {
		position: absolute;
		top: -50rpx;
		right: -50rpx;
		width: 200rpx;
		height: 200rpx;
		background: rgba(255, 255, 255, 0.15);
		border-radius: 50%;
		z-index: 1;
	}
	
	.card-bg-decoration::before {
		content: '';
		position: absolute;
		top: 60rpx;
		left: 60rpx;
		width: 80rpx;
		height: 80rpx;
		background: rgba(255, 255, 255, 0.12);
		border-radius: 50%;
	}
	
	.user-info-section {
		display: flex;
		align-items: center;
		position: relative;
		z-index: 2;
	}
	
	.avatar-container {
		position: relative;
		margin-right: 30rpx;
	}
	
	.user-avatar {
		width: 120rpx;
		height: 120rpx;
		border-radius: 50%;
		border: 4rpx solid rgba(255, 255, 255, 0.5);
	}
	
	.avatar-border {
		position: absolute;
		top: -6rpx;
		left: -6rpx;
		right: -6rpx;
		bottom: -6rpx;
		border: 2rpx solid rgba(255, 255, 255, 0.7);
		border-radius: 50%;
		animation: pulse 2s infinite;
	}
	
	@keyframes pulse {
		0% {
			transform: scale(1);
			opacity: 1;
		}
		50% {
			transform: scale(1.05);
			opacity: 0.7;
		}
		100% {
			transform: scale(1);
			opacity: 1;
		}
	}
	
	.user-details {
		flex: 1;
	}
	
	.welcome-text {
		font-size: 28rpx;
		color: rgba(255, 255, 255, 0.8);
		margin-bottom: 8rpx;
	}
	
	.user-name {
		font-size: 36rpx;
		font-weight: bold;
		color: #ffffff;
		margin-bottom: 6rpx;
	}
	
	.user-account {
		font-size: 24rpx;
		color: rgba(255, 255, 255, 0.6);
	}
	
	.member-level {
		position: absolute;
		top: 30rpx;
		right: 30rpx;
		background: rgba(255, 255, 255, 0.3);
		padding: 8rpx 16rpx;
		border-radius: 20rpx;
		backdrop-filter: blur(10rpx);
		z-index: 2;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.level-text {
		font-size: 22rpx;
		color: #ffffff;
		font-weight: 600;
		line-height: 1;
	}
</style>
