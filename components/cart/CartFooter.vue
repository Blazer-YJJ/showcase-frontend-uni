<template>
  <view class="cart-footer">
    <!-- 操作栏 -->
    <view class="action-bar">
      <!-- 全选 -->
      <view class="select-all" @click="toggleSelectAll">
        <view class="checkbox" :class="{ checked: isAllSelected }">
          <text class="check-icon" v-if="isAllSelected">✓</text>
        </view>
        <text class="select-text">全选</text>
      </view>
      
      <!-- 删除选中 -->
      <view 
        class="delete-btn" 
        :class="{ disabled: selectedCount === 0 }"
        @click="deleteSelected"
      >
        <text class="delete-text">删除</text>
      </view>
    </view>
    
    <view class="footer-content">
      <!-- 价格汇总 -->
      <view class="price-summary">
        <text class="total-label">合计:</text>
        <text class="total-price">¥{{ totalPrice }}</text>
      </view>
      
      <!-- 提交按钮 -->
      <view 
        class="submit-btn" 
        :class="{ disabled: selectedCount === 0 }"
        @click="submitOrder"
      >
        <text class="submit-text">提交订单</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  name: 'CartFooter',
  props: {
    // 总价格
    totalPrice: {
      type: Number,
      default: 0
    },
    // 选中商品数量
    selectedCount: {
      type: Number,
      default: 0
    },
    // 是否全选
    isAllSelected: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    /**
     * 切换全选状态
     */
    toggleSelectAll() {
      this.$emit('toggle-select-all')
    },
    
    /**
     * 删除选中商品
     */
    deleteSelected() {
      if (this.selectedCount === 0) {
        uni.showToast({
          title: '请选择要删除的商品',
          icon: 'none'
        })
        return
      }
      
      this.$emit('delete-selected')
    },
    
    /**
     * 提交订单
     */
    submitOrder() {
      if (this.selectedCount === 0) {
        uni.showToast({
          title: '请选择商品',
          icon: 'none'
        })
        return
      }
      
      this.$emit('submit-order')
    }
  }
}
</script>

<style scoped>
.cart-footer {
  margin-bottom: 0rpx;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #fff;
  border-top: 1rpx solid #f0f0f0;
  padding-bottom: env(safe-area-inset-bottom);
  z-index: 100;
}

/* 操作栏 */
.action-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16rpx 30rpx;
  border-bottom: 1rpx solid #f0f0f0;
  background: #fafafa;
}

/* 全选 */
.select-all {
  display: flex;
  align-items: center;
  gap: 12rpx;
}

.checkbox {
  width: 36rpx;
  height: 36rpx;
  border: 2rpx solid #ddd;
  border-radius: 6rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fff;
  transition: all 0.3s ease;
}

.checkbox.checked {
  background: #daa520;
  border-color: #daa520;
}

.check-icon {
  color: #fff;
  font-size: 24rpx;
  font-weight: bold;
}

.select-text {
  font-size: 28rpx;
  color: #333;
  font-weight: 500;
}

/* 删除按钮 */
.delete-btn {
  background: #ff4757;
  border-radius: 8rpx;
  padding: 16rpx 32rpx;
  transition: all 0.3s ease;
}

.delete-btn:active {
  transform: scale(0.95);
}

.delete-btn.disabled {
  background: #ccc;
  transform: none;
}

.delete-text {
  color: #fff;
  font-size: 26rpx;
  font-weight: 500;
}

.delete-btn.disabled .delete-text {
  color: #999;
}

.footer-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20rpx 30rpx;
  height: 88rpx;
}

/* 价格汇总 */
.price-summary {
  display: flex;
  align-items: center;
  gap: 8rpx;
}

.total-label {
  font-size: 28rpx;
  color: #333;
  font-weight: 500;
}

.total-price {
  font-size: 32rpx;
  color: #ff4757;
  font-weight: 600;
}

/* 提交按钮 */
.submit-btn {
  background: linear-gradient(135deg, #daa520 0%, #daa520 100%);
  border-radius: 50rpx;
  padding: 20rpx 50rpx;
  min-width: 180rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4rpx 12rpx rgba(255, 215, 0, 0.3);
  transition: all 0.3s ease;
}

.submit-btn:active {
  transform: scale(0.95);
}

.submit-btn.disabled {
  background: #ccc;
  box-shadow: none;
  transform: none;
}

.submit-text {
  color: #fff;
  font-size: 28rpx;
  font-weight: 600;
}

.submit-btn.disabled .submit-text {
  color: #999;
}
</style>