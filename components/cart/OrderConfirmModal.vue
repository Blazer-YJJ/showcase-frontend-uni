<template>
  <view class="modal-overlay" v-if="visible" @click="closeModal">
    <view class="modal-content" @click.stop>
      <!-- 弹窗头部 -->
      <view class="modal-header">
        <text class="modal-title">确认订单</text>
        <view class="close-btn" @click="closeModal">
          <text class="close-icon">×</text>
        </view>
      </view>
      
      <!-- 弹窗内容 -->
      <view class="modal-body-wrapper">
        <view class="modal-body">
        <!-- 收货信息 -->
        <view class="section">
          <view class="section-title">
            <text class="title-text">收货信息</text>
            <view class="edit-btn" @click="editAddress">
              <text class="edit-text">编辑</text>
            </view>
          </view>
          <view class="address-info" v-if="addressInfo">
            <view class="address-item">
              <text class="label">收货人：</text>
              <text class="value">{{ addressInfo.name }}</text>
            </view>
            <view class="address-item">
              <text class="label">联系电话：</text>
              <text class="value">{{ addressInfo.phone }}</text>
            </view>
            <view class="address-item">
              <text class="label">收货地址：</text>
              <text class="value">{{ addressInfo.address }}</text>
            </view>
          </view>
          <view class="no-address" v-else @click="addAddress">
            <text class="no-address-text">请添加收货地址</text>
            <text class="arrow">></text>
          </view>
        </view>
        
        <!-- 商品信息 -->
        <view class="section">
          <view class="section-title">
            <text class="title-text">商品信息</text>
            <text class="count-text">({{ selectedItems.length }}件)</text>
          </view>
          <view class="product-list">
            <view 
              class="product-item" 
              v-for="item in selectedItems" 
              :key="item.id"
            >
              <image :src="item.image" class="product-img" mode="aspectFit" />
              <view class="product-details">
                <view class="product-name">{{ item.name }}</view>
                <view class="product-spec" v-if="item.spec">{{ item.spec }}</view>
                <view class="product-remark" v-if="item.remark">
                  <text class="remark-label">备注：</text>
                  <text class="remark-text">{{ item.remark }}</text>
                </view>
                <view class="product-price-qty">
                  <text class="price">¥{{ item.price }}</text>
                  <text class="quantity">×{{ item.quantity }}</text>
                </view>
              </view>
            </view>
          </view>
        </view>
        
        <!-- 订单备注 -->
        <view class="section">
          <view class="section-title">
            <text class="title-text">订单备注</text>
          </view>
          <textarea 
            v-model="orderRemark"
            placeholder="请输入订单备注（如：尺寸要求、包装要求等）"
            class="remark-textarea"
            maxlength="200"
            :show-count="true"
          />
        </view>
        
        <!-- 价格汇总 -->
        <view class="section">
          <view class="price-summary">
            <view class="summary-item">
              <text class="summary-label">商品总价：</text>
              <text class="summary-value">¥{{ totalPrice }}</text>
            </view>
            <view class="summary-item">
              <text class="summary-label">运费：</text>
              <text class="summary-value">¥{{ shippingFee }}</text>
            </view>
            <view class="summary-item total">
              <text class="summary-label">实付金额：</text>
              <text class="summary-value">¥{{ finalPrice }}</text>
            </view>
          </view>
        </view>
        </view>
      </view>
      
      <!-- 弹窗底部 -->
      <view class="modal-footer">
        <view class="footer-info">
          <text class="total-text">实付：¥{{ finalPrice }}</text>
        </view>
        <view class="confirm-btn" @click="confirmOrder">
          <text class="confirm-text">确认下单</text>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  name: 'OrderConfirmModal',
  props: {
    // 是否显示弹窗
    visible: {
      type: Boolean,
      default: false
    },
    // 选中的商品
    selectedItems: {
      type: Array,
      default: () => []
    },
    // 总价格
    totalPrice: {
      type: Number,
      default: 0
    },
    // 收货地址信息
    addressInfo: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      orderRemark: '', // 订单备注
      shippingFee: 0, // 运费
      scrollTop: 0, // 滚动位置
      isScrolling: false // 是否正在滚动
    }
  },
  computed: {
    /**
     * 最终支付金额
     */
    finalPrice() {
      return this.totalPrice + this.shippingFee
    }
  },
  methods: {
    /**
     * 滚动事件处理
     */
    onScroll(e) {
      this.scrollTop = e.detail.scrollTop
    },
    
    /**
     * 触摸开始
     */
    onTouchStart() {
      this.isScrolling = true
    },
    
    /**
     * 触摸结束
     */
    onTouchEnd() {
      setTimeout(() => {
        this.isScrolling = false
      }, 100)
    },
    
    /**
     * 关闭弹窗
     */
    closeModal() {
      this.$emit('close')
    },
    
    /**
     * 编辑地址
     */
    editAddress() {
      this.$emit('edit-address')
    },
    
    /**
     * 添加地址
     */
    addAddress() {
      this.$emit('add-address')
    },
    
    /**
     * 确认下单
     */
    confirmOrder() {
      if (!this.addressInfo) {
        uni.showToast({
          title: '请先添加收货地址',
          icon: 'none'
        })
        return
      }
      
      const orderData = {
        addressInfo: this.addressInfo,
        items: this.selectedItems,
        totalPrice: this.totalPrice,
        shippingFee: this.shippingFee,
        finalPrice: this.finalPrice,
        remark: this.orderRemark
      }
      
      this.$emit('confirm-order', orderData)
    }
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1000;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  /* 防止滚动穿透 */
  overflow: hidden;
}

.modal-content {
  background: #fff;
  border-radius: 24rpx 24rpx 0 0;
  width: 100%;
  max-width: 750rpx;
  max-height: 85vh;
  min-height: 60vh;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  /* 确保内容区域可以滚动 */
  position: relative;
}

/* 弹窗头部 */
.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 30rpx;
  border-bottom: 1rpx solid #f0f0f0;
}

.modal-title {
  font-size: 32rpx;
  color: #333;
  font-weight: 600;
}

.close-btn {
  width: 60rpx;
  height: 60rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  background: #f8f8f8;
}

.close-icon {
  font-size: 36rpx;
  color: #666;
}

/* 弹窗内容包装器 */
.modal-body-wrapper {
  flex: 1;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

/* 弹窗内容 */
.modal-body {
  flex: 1;
  padding: 0 30rpx;
  /* 微信小程序滚动优化 */
  -webkit-overflow-scrolling: touch;
  overscroll-behavior: contain;
  /* 确保可以滚动 */
  overflow-y: auto;
  /* 明确允许触摸滚动 */
  touch-action: pan-y;
}

.section {
  margin-bottom: 40rpx;
  padding-top: 30rpx;
}

.section:first-child {
  padding-top: 0;
}

.section-title {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
}

.title-text {
  font-size: 28rpx;
  color: #333;
  font-weight: 600;
}

.count-text {
  font-size: 24rpx;
  color: #999;
  margin-left: 8rpx;
}

.edit-btn {
  margin-left: auto;
  padding: 8rpx 16rpx;
  background: #f0f0f0;
  border-radius: 16rpx;
}

.edit-text {
  font-size: 24rpx;
  color: #007aff;
}

/* 收货信息 */
.address-info {
  background: #f8f8f8;
  border-radius: 12rpx;
  padding: 20rpx;
}

.address-item {
  display: flex;
  margin-bottom: 12rpx;
}

.address-item:last-child {
  margin-bottom: 0;
}

.label {
  font-size: 26rpx;
  color: #666;
  min-width: 120rpx;
}

.value {
  font-size: 26rpx;
  color: #333;
  flex: 1;
}

.no-address {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 30rpx;
  background: #f8f8f8;
  border-radius: 12rpx;
  border: 2rpx dashed #ddd;
}

.no-address-text {
  font-size: 26rpx;
  color: #999;
}

.arrow {
  font-size: 24rpx;
  color: #999;
}

/* 商品列表 */
.product-list {
  background: #f8f8f8;
  border-radius: 12rpx;
  padding: 20rpx;
}

.product-item {
  display: flex;
  margin-bottom: 20rpx;
  padding-bottom: 20rpx;
  border-bottom: 1rpx solid #e8e8e8;
}

.product-item:last-child {
  margin-bottom: 0;
  padding-bottom: 0;
  border-bottom: none;
}

.product-img {
  width: 120rpx;
  height: 120rpx;
  border-radius: 8rpx;
  margin-right: 20rpx;
}

.product-details {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8rpx;
}

.product-name {
  font-size: 26rpx;
  color: #333;
  font-weight: 500;
  line-height: 1.4;
}

.product-spec {
  font-size: 22rpx;
  color: #999;
  background: #fff;
  padding: 4rpx 8rpx;
  border-radius: 8rpx;
  align-self: flex-start;
}

.product-remark {
  font-size: 22rpx;
  color: #666;
}

.remark-label {
  color: #999;
}

.remark-text {
  color: #333;
}

.product-price-qty {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 8rpx;
}

.price {
  font-size: 28rpx;
  color: #ff4757;
  font-weight: 600;
}

.quantity {
  font-size: 24rpx;
  color: #666;
}

/* 订单备注 */
.remark-textarea {
  width: 94%;
  min-height: 120rpx;
  background: #f8f8f8;
  border-radius: 12rpx;
  padding: 20rpx;
  font-size: 26rpx;
  color: #333;
  border: 1rpx solid #e8e8e8;
}

.remark-textarea::placeholder {
  color: #999;
}

/* 价格汇总 */
.price-summary {
  background: #f8f8f8;
  border-radius: 12rpx;
  padding: 20rpx;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 12rpx;
}

.summary-item:last-child {
  margin-bottom: 0;
}

.summary-item.total {
  padding-top: 12rpx;
  border-top: 1rpx solid #e8e8e8;
}

.summary-label {
  font-size: 26rpx;
  color: #666;
}

.summary-value {
  font-size: 26rpx;
  color: #333;
  font-weight: 500;
}

.summary-item.total .summary-label {
  font-size: 28rpx;
  color: #333;
  font-weight: 600;
}

.summary-item.total .summary-value {
  font-size: 28rpx;
  color: #ff4757;
  font-weight: 600;
}

/* 弹窗底部 */
.modal-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 30rpx;
  border-top: 1rpx solid #f0f0f0;
  background: #fff;
  flex-shrink: 0; /* 防止底部被压缩 */
  box-shadow: 0 -2rpx 10rpx rgba(0, 0, 0, 0.1);
}

.footer-info {
  flex: 1;
}

.total-text {
  font-size: 28rpx;
  color: #333;
  font-weight: 600;
}

.confirm-btn {
  background: linear-gradient(135deg, #007aff 0%, #0056cc 100%);
  border-radius: 50rpx;
  padding: 20rpx 40rpx;
  min-width: 200rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4rpx 12rpx rgba(0, 122, 255, 0.3);
}

.confirm-text {
  color: #fff;
  font-size: 28rpx;
  font-weight: 600;
}
</style>