<template>
  <view class="detail-container">
    <view class="detail-header">
      <view class="back-btn" @click="goBack">
        <text class="back-icon">〈</text>
      </view>
      <view class="edit-btn" @click="editMemo">
        <image class="edit-icon" src="/static/images/edit.svg"></image>
      </view>
      <view class="delete-btn" @click="showDeleteConfirm">
        <image class="delete-icon" src="/static/images/delete.svg"></image>
      </view>
    </view>
    
    <view class="detail-content">
      <view class="memo-title">{{memoInfo.title}}</view>
      <view class="memo-content">{{memoInfo.content}}</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      memoInfo: {
        id: 0,
        title: '标题标题标题标题',
        content: '内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容'
      }
    }
  },
  onLoad(option) {
    const id = option.id;
    if (id) {
      this.memoInfo.id = id;
      console.log('加载备忘录ID:', id);
      
      // 从本地存储加载数据
      this.loadMemoData(id);
    }
  },
  methods: {
    // 加载备忘录数据
    loadMemoData(id) {
      uni.getStorage({
        key: 'memoList',
        success: (res) => {
          const memoList = res.data || [];
          const memo = memoList.find(item => item.id === parseInt(id));
          if (memo) {
            this.memoInfo = memo;
          } else {
            uni.showToast({
              title: '备忘录不存在',
              icon: 'none'
            });
          }
        },
        fail: () => {
          uni.showToast({
            title: '加载数据失败',
            icon: 'none'
          });
        }
      });
    },
    // 返回上一页
    goBack() {
      uni.navigateBack();
    },
    
    // 编辑备忘录
    editMemo() {
      uni.navigateTo({
        url: `/pages/memo/edit?id=${this.memoInfo.id}`
      });
    },
    
    // 显示删除确认框
    showDeleteConfirm() {
      uni.showModal({
        title: '提示',
        content: '确定要删除这条备忘录吗？',
        success: (res) => {
          if (res.confirm) {
            this.deleteMemo();
          }
        }
      });
    },
    
    // 删除备忘录
    deleteMemo() {
      // 显示加载中提示
      uni.showLoading({
        title: '删除中...'
      });
      
      // 从本地存储中删除
      uni.getStorage({
        key: 'memoList',
        success: (res) => {
          let memoList = res.data || [];
          
          // 找到要删除的备忘录索引
          const index = memoList.findIndex(item => item.id === parseInt(this.memoInfo.id));
          
          if (index !== -1) {
            // 删除该备忘录
            memoList.splice(index, 1);
            
            // 保存更新后的列表
            uni.setStorage({
              key: 'memoList',
              data: memoList,
              success: () => {
                uni.hideLoading();
                uni.showToast({
                  title: '删除成功',
                  icon: 'success'
                });
                
                // 删除成功后返回列表页
                setTimeout(() => {
                  uni.navigateBack();
                }, 1000);
              }
            });
          } else {
            uni.hideLoading();
            uni.showToast({
              title: '备忘录不存在',
              icon: 'none'
            });
          }
        },
        fail: () => {
          uni.hideLoading();
          uni.showToast({
            title: '操作失败',
            icon: 'none'
          });
        }
      });
    }
  }
}
</script>

<style lang="scss">
.detail-container {
  min-height: 100vh;
  background-color: #fff;
  display: flex;
  flex-direction: column;
}

.detail-header {
  height: 88rpx;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  border-bottom: 1rpx solid #f2f2f2;
  padding: 0 30rpx;
  
  .back-btn {
    position: absolute;
    left: 30rpx;
    top: 50%;
    transform: translateY(-50%);
    width: 60rpx;
    height: 60rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    
    .back-icon {
      font-size: 36rpx;
      color: #333;
    }
  }
  
  .edit-btn {
    position: absolute;
    right: 100rpx;
    top: 50%;
    transform: translateY(-50%);
    width: 60rpx;
    height: 60rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #f5f7ff;
    border-radius: 50%;
    
    .edit-icon {
      width: 30rpx;
      height: 30rpx;
    }
  }
  
  .delete-btn {
    position: absolute;
    right: 30rpx;
    top: 50%;
    transform: translateY(-50%);
    width: 60rpx;
    height: 60rpx;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #fff2f2;
    
    .delete-icon {
      width: 30rpx;
      height: 30rpx;
    }
  }
}

.detail-content {
  flex: 1;
  padding: 40rpx 30rpx;
  
  .memo-title {
    font-size: 40rpx;
    font-weight: bold;
    color: #333;
    margin-bottom: 40rpx;
  }
  
  .memo-content {
    font-size: 30rpx;
    color: #333;
    line-height: 1.6;
  }
}
</style> 