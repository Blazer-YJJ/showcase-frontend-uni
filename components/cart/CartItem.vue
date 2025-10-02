<template>
  <view class="cart-item">
    <!-- 选择框 -->
    <view class="item-checkbox" @click="toggleSelect">
      <view class="checkbox" :class="{ checked: item.selected }">
        <text class="checkmark" v-if="item.selected">✓</text>
      </view>
    </view>
    
    <!-- 商品图片 -->
    <view class="item-image">
      <image :src="item.image" class="product-img" mode="aspectFit" />
    </view>
    
    <!-- 商品信息 -->
    <view class="item-info">
      <!-- 商品名称 -->
      <view class="product-name">{{ item.name }}</view>
      
      <!-- 商品分类 -->
      <view class="product-category">{{ item.category }}</view>
      
      <!-- 商品规格 -->
      <view class="product-spec" v-if="item.spec">{{ item.spec }}</view>
      
      <!-- 备注输入框 -->
      <view class="remark-section">
        <input 
          v-model="item.remark"
          placeholder="添加备注（如：尺寸、包装等）..."
          class="remark-input"
          @blur="updateRemark"
          @confirm="updateRemark"
        />
      </view>
      
      <!-- 价格和数量 -->
      <view class="price-quantity">
        <view class="price-section">
          <text class="price">¥{{ item.price }}</text>
        </view>
        
        <view class="quantity-section">
          <view class="quantity-control">
            <view class="qty-btn minus" @click="decreaseQuantity">
              <text class="qty-text">-</text>
            </view>
            <view class="qty-number">
              <text class="qty-text">{{ item.quantity }}</text>
            </view>
            <view class="qty-btn plus" @click="increaseQuantity">
              <text class="qty-text">+</text>
            </view>
          </view>
        </view>
      </view>
    </view>
    
    <!-- 删除按钮（编辑模式下显示） -->
    <view class="delete-btn" v-if="isEditMode" @click="deleteItem">
      <text class="delete-text">删除</text>
    </view>
  </view>
</template>

<script>
export default {
  name: 'CartItem',
  props: {
    // 商品数据
    item: {
      type: Object,
      required: true
    },
    // 是否编辑模式
    isEditMode: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    /**
     * 切换选择状态
     */
    toggleSelect() {
      this.$emit('toggle-select', this.item.id)
    },
    
    /**
     * 增加数量
     */
    increaseQuantity() {
      const newQuantity = this.item.quantity + 1
      this.$emit('update-quantity', this.item.id, newQuantity)
    },
    
    /**
     * 减少数量
     */
    decreaseQuantity() {
      if (this.item.quantity > 1) {
        const newQuantity = this.item.quantity - 1
        this.$emit('update-quantity', this.item.id, newQuantity)
      }
    },
    
    /**
     * 更新备注
     */
    updateRemark() {
      this.$emit('update-remark', this.item.id, this.item.remark)
    },
    
    /**
     * 删除商品
     */
    deleteItem() {
      uni.showModal({
        title: '确认删除',
        content: '确定要删除这个商品吗？',
        success: (res) => {
          if (res.confirm) {
            this.$emit('delete-item', this.item.id)
          }
        }
      })
    }
  }
}
</script>

<style scoped>
.cart-item {
  background: #fff;
  margin: 20rpx;
  border-radius: 16rpx;
  padding: 24rpx;
  display: flex;
  align-items: center;
  gap: 20rpx;
  box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.06);
  position: relative;
}

/* 选择框 */
.item-checkbox {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.checkbox {
  width: 40rpx;
  height: 40rpx;
  border: 2rpx solid #ddd;
  border-radius: 8rpx;
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

.checkmark {
  color: #fff;
  font-size: 24rpx;
  font-weight: bold;
}

/* 商品图片 */
.item-image {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.product-img {
  width: 120rpx;
  height: 120rpx;
  border-radius: 12rpx;
  background: #f8f8f8;
}

/* 商品信息 */
.item-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.product-name {
  font-size: 28rpx;
  color: #333;
  font-weight: 500;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
}

.product-category {
  font-size: 22rpx;
  color: #999;
  background: #f8f8f8;
  padding: 4rpx 12rpx;
  border-radius: 12rpx;
  align-self: flex-start;
}

.product-spec {
  font-size: 24rpx;
  color: #666;
  background: #f0f0f0;
  padding: 6rpx 12rpx;
  border-radius: 12rpx;
  align-self: flex-start;
}

/* 备注输入框 */
.remark-section {
  margin: 8rpx 0;
}

.remark-input {
  width: 90%;
  height: 60rpx;
  background: #f8f8f8;
  border-radius: 12rpx;
  padding: 0 20rpx;
  font-size: 24rpx;
  color: #333;
  border: 1rpx solid #e8e8e8;
}

.remark-input::placeholder {
  color: #999;
}

/* 价格和数量 */
.price-quantity {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 12rpx;
}

.price-section {
  flex: 1;
}

.price {
  font-size: 32rpx;
  color: #ff4757;
  font-weight: 600;
}

.quantity-section {
  flex-shrink: 0;
}

.quantity-control {
  display: flex;
  align-items: center;
  background: #f8f8f8;
  border-radius: 20rpx;
  overflow: hidden;
}

.qty-btn {
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fff;
  border: 1rpx solid #e8e8e8;
  transition: all 0.3s ease;
}

.qty-btn:active {
  background: #f0f0f0;
}

.qty-btn.minus {
  border-radius: 20rpx 0 0 20rpx;
}

.qty-btn.plus {
  border-radius: 0 20rpx 20rpx 0;
}

.qty-number {
  width: 80rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fff;
  border-top: 1rpx solid #e8e8e8;
  border-bottom: 1rpx solid #e8e8e8;
}

.qty-text {
  font-size: 28rpx;
  color: #333;
  font-weight: 500;
}

/* 删除按钮 */
.delete-btn {
  position: absolute;
  top: 24rpx;
  right: 24rpx;
  padding: 8rpx 16rpx;
  background: #ff4757;
  border-radius: 16rpx;
  opacity: 0.9;
}

.delete-text {
  color: #fff;
  font-size: 22rpx;
  font-weight: 500;
}
</style>