<template>
  <view class="memo-container">
    <view class="memo-header">
      <text class="header-title">备忘录</text>
    </view>
    
    <view class="memo-list" v-if="memoList.length > 0">
      <view class="memo-item" v-for="(item, index) in memoList" :key="index" @click="viewMemo(item)">
        <view class="memo-title">{{ item.title }}</view>
        <view class="memo-content">{{ item.content }}</view>
      </view>
    </view>
    
    <view class="empty-content" v-else>
      <text class="empty-text">暂无备忘录</text>
      <text class="empty-tip">点击右下角按钮创建新的备忘录</text>
    </view>
    
    <view class="add-btn" hover-class="add-btn-hover" @click="addMemo">
      <text class="add-icon">+</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      memoList: []
    }
  },
  onShow() {
    // 每次页面显示时重新加载数据
    this.loadMemoList();
  },
  methods: {
    // 加载备忘录列表
    loadMemoList() {
      uni.getStorage({
        key: 'memoList',
        success: (res) => {
          this.memoList = res.data || [];
        },
        fail: () => {
          this.memoList = [];
          // 初始化时创建一些演示数据
          if (this.memoList.length === 0) {
            const demoList = [];
            for (let i = 1; i <= 6; i++) {
              demoList.push({
                id: i,
                title: `标题标题标题${i}`,
                content: `内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容...`,
                createTime: new Date().getTime(),
                updateTime: new Date().getTime()
              });
            }
            
            uni.setStorage({
              key: 'memoList',
              data: demoList,
              success: () => {
                this.memoList = demoList;
              }
            });
          }
        }
      });
    },
    
    // 查看备忘录详情
    viewMemo(memo) {
      uni.navigateTo({
        url: `/pages/memo/detail?id=${memo.id}`
      });
    },
    
    // 添加新备忘录
    addMemo() {
      uni.navigateTo({
        url: '/pages/memo/edit'
      });
    }
  }
}
</script>

<style lang="scss">
.memo-container {
  min-height: 100vh;
  background-color: #f8f8f8;
  padding-bottom: 100rpx;
  position: relative;
}

.memo-header {
  height: 88rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #fff;
  position: sticky;
  top: 0;
  z-index: 10;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  
  .header-title {
    font-size: 36rpx;
    font-weight: 500;
    color: #333;
  }
}

.memo-list {
  padding: 20rpx;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.memo-item {
  width: 48%;
  background-color: #fff;
  border-radius: 20rpx;
  padding: 30rpx;
  margin-bottom: 20rpx;
  box-shadow: 0 2rpx 10rpx rgba(0, 0, 0, 0.05);
  min-height: 200rpx;
  display: flex;
  flex-direction: column;
  
  .memo-title {
    font-size: 32rpx;
    font-weight: 500;
    color: #333;
    margin-bottom: 16rpx;
  }
  
  .memo-content {
    font-size: 26rpx;
    color: #666;
    line-height: 1.5;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 4;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}

.empty-content {
  padding: 200rpx 40rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  
  .empty-text {
    font-size: 32rpx;
    color: #999;
    margin-bottom: 20rpx;
  }
  
  .empty-tip {
    font-size: 26rpx;
    color: #bbb;
  }
}

.add-btn {
  position: fixed;
  right: 40rpx;
  bottom: 40rpx;
  width: 100rpx;
  height: 100rpx;
  background: linear-gradient(135deg, #7e85dd, #a4a9ff);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 6rpx 20rpx rgba(126, 133, 221, 0.4);
  z-index: 100;
  transition: all 0.2s ease;
  
  .add-icon {
    font-size: 60rpx;
    color: #fff;
    line-height: 1;
    margin-top: -8rpx;
  }
}

.add-btn-hover {
  transform: scale(0.95);
  box-shadow: 0 3rpx 10rpx rgba(126, 133, 221, 0.4);
}
</style> 