<!--
 * @Author: JingChengCool jingchengcool@outlook.com
 * @Date: 2025-10-01 01:09:39
 * @LastEditors: JingChengCool jingchengcool@outlook.com
 * @LastEditTime: 2025-10-02 06:05:32
 * @FilePath: \showcase-frontend-uni\pages\cart\cart.vue
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
<template>
  <view class="cart-page">
    
    <!-- 购物车内容 -->
    <scroll-view class="cart-content" scroll-y>
      <!-- 空购物车状态 -->
      <view class="empty-cart" v-if="cartItems.length === 0">
        <image src="/static/images/empty-cart.png" class="empty-image" mode="aspectFit" />
        <text class="empty-text">购物车空空如也</text>
        <text class="empty-desc">快去挑选心仪的黄金首饰吧</text>
        <view class="go-shopping-btn" @click="goShopping">
          <text class="btn-text">去购物</text>
        </view>
      </view>
      
      <!-- 商品列表 -->
      <view class="cart-list" v-else>
        <CartItem
          v-for="item in cartItems"
          :key="item.id"
          :item="item"
          :isEditMode="false"
          @toggle-select="toggleItemSelect"
          @update-quantity="updateItemQuantity"
          @update-remark="updateItemRemark"
          @delete-item="deleteItem"
        />
      </view>
    </scroll-view>
    
    <!-- 购物车底部 -->
    <CartFooter
      v-if="cartItems.length > 0"
      :totalPrice="totalPrice"
      :selectedCount="selectedCount"
      :isAllSelected="isAllSelected"
      @toggle-select-all="toggleSelectAll"
      @delete-selected="deleteSelectedItems"
      @submit-order="showOrderModal"
    />
    
    <!-- 订单确认弹窗 -->
    <OrderConfirmModal
      :visible="showModal"
      :selectedItems="selectedItems"
      :totalPrice="totalPrice"
      :addressInfo="addressInfo"
      @close="hideOrderModal"
      @edit-address="editAddress"
      @add-address="addAddress"
      @confirm-order="confirmOrder"
    />
  </view>
</template>

<script>
import CartItem from '@/components/cart/CartItem.vue'
import CartFooter from '@/components/cart/CartFooter.vue'
import OrderConfirmModal from '@/components/cart/OrderConfirmModal.vue'

export default {
  name: 'CartPage',
  components: {
    CartItem,
    CartFooter,
    OrderConfirmModal
  },
  data() {
    return {
      // 购物车商品列表
      cartItems: [
        {
          id: '1',
          name: '周大福 足金999项链 经典O字链',
          category: '黄金首饰',
          spec: '足金999 18英寸',
          image: '/static/images/001.png',
          price: 2580,
          quantity: 1,
          selected: false,
          remark: ''
        },
        {
          id: '2',
          name: '老凤祥 足金手镯 古法工艺',
          category: '黄金首饰',
          spec: '足金999 圈口56mm',
          image: '/static/images/002.png',
          price: 3680,
          quantity: 2,
          selected: false,
          remark: '请尽快发货'
        },
        {
          id: '3',
          name: '六福珠宝 足金耳环 简约耳钉',
          category: '黄金首饰',
          spec: '足金999 单只0.8g',
          image: '/static/images/003.png',
          price: 1280,
          quantity: 1,
          selected: false,
          remark: ''
        },
        {
          id: '4',
          name: '周生生 足金戒指 经典素圈',
          category: '黄金首饰',
          spec: '足金999 圈口12号',
          image: '/static/images/004.png',
          price: 1580,
          quantity: 1,
          selected: false,
          remark: '请选择合适尺寸'
        },
        {
          id: '5',
          name: '中国黄金 足金吊坠 福字款',
          category: '黄金首饰',
          spec: '足金999 重量2.5g',
          image: '/static/images/001.png',
          price: 1980,
          quantity: 1,
          selected: false,
          remark: ''
        }
      ],
      // 是否显示订单确认弹窗
      showModal: false,
      // 收货地址信息
      addressInfo: {
        name: '张三',
        phone: '13800138000',
        address: '北京市朝阳区三里屯街道工体北路8号院'
      }
    }
  },
  computed: {
    /**
     * 是否全选
     */
    isAllSelected() {
      return this.cartItems.length > 0 && this.cartItems.every(item => item.selected)
    },
    
    /**
     * 选中商品数量
     */
    selectedCount() {
      return this.cartItems.filter(item => item.selected).length
    },
    
    /**
     * 选中商品列表
     */
    selectedItems() {
      return this.cartItems.filter(item => item.selected)
    },
    
    /**
     * 总价格
     */
    totalPrice() {
      return this.cartItems
        .filter(item => item.selected)
        .reduce((total, item) => total + (item.price * item.quantity), 0)
    }
  },
  onLoad() {
    console.log('购物车页加载')
    this.loadCartData()
  },
  methods: {
    /**
     * 加载购物车数据
     */
    loadCartData() {
      // 这里可以从本地存储或API获取购物车数据
      // const cartData = uni.getStorageSync('cartItems')
      // if (cartData) {
      //   this.cartItems = cartData
      // }
    },
    
    /**
     * 保存购物车数据
     */
    saveCartData() {
      uni.setStorageSync('cartItems', this.cartItems)
    },
    
    /**
     * 切换全选状态
     */
    toggleSelectAll() {
      const shouldSelectAll = !this.isAllSelected
      this.cartItems.forEach(item => {
        item.selected = shouldSelectAll
      })
      this.saveCartData()
    },
    
    /**
     * 切换商品选择状态
     */
    toggleItemSelect(itemId) {
      const item = this.cartItems.find(item => item.id === itemId)
      if (item) {
        item.selected = !item.selected
        this.saveCartData()
      }
    },
    
    /**
     * 更新商品数量
     */
    updateItemQuantity(itemId, quantity) {
      const item = this.cartItems.find(item => item.id === itemId)
      if (item && quantity > 0) {
        item.quantity = quantity
        this.saveCartData()
      }
    },
    
    /**
     * 更新商品备注
     */
    updateItemRemark(itemId, remark) {
      const item = this.cartItems.find(item => item.id === itemId)
      if (item) {
        item.remark = remark
        this.saveCartData()
      }
    },
    
    /**
     * 删除商品
     */
    deleteItem(itemId) {
      const index = this.cartItems.findIndex(item => item.id === itemId)
      if (index > -1) {
        this.cartItems.splice(index, 1)
        this.saveCartData()
        
        uni.showToast({
          title: '删除成功',
          icon: 'success'
        })
      }
    },
    
    /**
     * 删除选中商品
     */
    deleteSelectedItems() {
      if (this.selectedCount === 0) {
        uni.showToast({
          title: '请选择要删除的商品',
          icon: 'none'
        })
        return
      }
      
      uni.showModal({
        title: '确认删除',
        content: `确定要删除选中的 ${this.selectedCount} 件商品吗？`,
        success: (res) => {
          if (res.confirm) {
            this.cartItems = this.cartItems.filter(item => !item.selected)
            this.saveCartData()
            
            uni.showToast({
              title: '删除成功',
              icon: 'success'
            })
          }
        }
      })
    },
    
    /**
     * 显示订单确认弹窗
     */
    showOrderModal() {
      if (this.selectedCount === 0) {
        uni.showToast({
          title: '请选择商品',
          icon: 'none'
        })
        return
      }
      this.showModal = true
    },
    
    /**
     * 隐藏订单确认弹窗
     */
    hideOrderModal() {
      this.showModal = false
    },
    
    /**
     * 编辑地址
     */
    editAddress() {
      uni.showToast({
        title: '跳转到地址编辑页面',
        icon: 'none'
      })
      // 这里可以跳转到地址编辑页面
      // uni.navigateTo({
      //   url: '/pages/address/edit'
      // })
    },
    
    /**
     * 添加地址
     */
    addAddress() {
      uni.showToast({
        title: '跳转到地址添加页面',
        icon: 'none'
      })
      // 这里可以跳转到地址添加页面
      // uni.navigateTo({
      //   url: '/pages/address/add'
      // })
    },
    
    /**
     * 确认下单
     */
    confirmOrder(orderData) {
      console.log('订单数据:', orderData)
      
      // 这里可以调用下单API
      uni.showLoading({
        title: '正在下单...'
      })
      
      // 模拟API调用
      setTimeout(() => {
        uni.hideLoading()
        uni.showToast({
          title: '下单成功',
          icon: 'success'
        })
        
        // 清空已选商品
        this.cartItems = this.cartItems.filter(item => !item.selected)
        this.saveCartData()
        
        this.hideOrderModal()
        
        // 跳转到订单页面
        setTimeout(() => {
          uni.navigateTo({
            url: '/pages/order/list'
          })
        }, 1500)
      }, 2000)
    },
    
    /**
     * 去购物
     */
    goShopping() {
      uni.switchTab({
        url: '/pages/index/index'
      })
    }
  }
}
</script>

<style scoped>
.cart-page {
  background: #f5f5f5;
  min-height: 100vh;
  padding-bottom: 140rpx;
}

.cart-content {
  height: calc(100vh - 120rpx);
}

/* 空购物车状态 */
.empty-cart {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 120rpx 60rpx;
  text-align: center;
}

.empty-image {
  width: 200rpx;
  height: 200rpx;
  margin-bottom: 40rpx;
  opacity: 0.6;
}

.empty-text {
  font-size: 32rpx;
  color: #666;
  font-weight: 500;
  margin-bottom: 16rpx;
}

.empty-desc {
  font-size: 26rpx;
  color: #999;
  margin-bottom: 60rpx;
}

.go-shopping-btn {
  background: linear-gradient(135deg, #007aff 0%, #0056cc 100%);
  border-radius: 50rpx;
  padding: 24rpx 60rpx;
  box-shadow: 0 4rpx 12rpx rgba(0, 122, 255, 0.3);
}

.btn-text {
  color: #fff;
  font-size: 28rpx;
  font-weight: 600;
}

/* 购物车列表 */
.cart-list {
  padding: 20rpx 0;
}
</style>