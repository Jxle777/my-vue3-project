<template>
  <view class="edit-container">
    <view class="edit-header">
      <view class="back-btn" @click="goBack">
        <text class="back-icon">〈</text>
      </view>
      <text class="header-title">{{ isEdit ? '编辑备忘录' : '新建备忘录' }}</text>
      <view class="save-btn" @click="saveMemo">
        <text class="save-text">保存</text>
      </view>
    </view>
    
    <view class="edit-content">
      <input 
        class="memo-title-input" 
        v-model="memoInfo.title" 
        placeholder="请输入标题"
        maxlength="50"
      />
      <textarea 
        class="memo-content-textarea"
        v-model="memoInfo.content"
        placeholder="请输入内容"
        maxlength="5000"
        auto-height
      ></textarea>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      isEdit: false,
      memoId: 0,
      memoInfo: {
        title: '',
        content: ''
      }
    }
  },
  onLoad(option) {
    if (option.id) {
      this.isEdit = true;
      this.memoId = option.id;
      // 加载现有备忘录数据
      this.loadMemoData(option.id);
    }
  },
  methods: {
    // 加载备忘录数据
    loadMemoData(id) {
      // 模拟从服务器获取数据
      // 实际项目中应该调用API获取数据
      uni.getStorage({
        key: 'memoList',
        success: (res) => {
          const memoList = res.data || [];
          const memo = memoList.find(item => item.id === parseInt(id));
          if (memo) {
            this.memoInfo = {
              title: memo.title,
              content: memo.content
            };
          }
        },
        fail: () => {
          // 如果没有找到数据，使用默认数据（模拟数据）
          this.memoInfo = {
            title: '标题标题标题',
            content: '内容内容内容内容内容内容内容内容内容内容内容内容'
          };
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
    
    // 保存备忘录
    saveMemo() {
      if (!this.memoInfo.title.trim()) {
        uni.showToast({
          title: '请输入标题',
          icon: 'none'
        });
        return;
      }
      
      if (!this.memoInfo.content.trim()) {
        uni.showToast({
          title: '请输入内容',
          icon: 'none'
        });
        return;
      }
      
      // 模拟保存操作
      uni.showLoading({
        title: '保存中...'
      });
      
      // 模拟与服务器交互的延迟
      setTimeout(() => {
        // 获取现有的备忘录列表
        uni.getStorage({
          key: 'memoList',
          success: (res) => {
            let memoList = res.data || [];
            
            if (this.isEdit) {
              // 编辑模式：更新现有备忘录
              const index = memoList.findIndex(item => item.id === parseInt(this.memoId));
              if (index !== -1) {
                memoList[index] = {
                  ...memoList[index],
                  title: this.memoInfo.title,
                  content: this.memoInfo.content,
                  updateTime: new Date().getTime()
                };
              }
            } else {
              // 新建模式：添加新备忘录
              const newMemo = {
                id: new Date().getTime(),
                title: this.memoInfo.title,
                content: this.memoInfo.content,
                createTime: new Date().getTime(),
                updateTime: new Date().getTime()
              };
              memoList.unshift(newMemo);
            }
            
            // 保存更新后的列表
            uni.setStorage({
              key: 'memoList',
              data: memoList,
              success: () => {
                uni.hideLoading();
                uni.showToast({
                  title: this.isEdit ? '更新成功' : '添加成功',
                  icon: 'success'
                });
                
                // 保存成功后返回
                setTimeout(() => {
                  uni.navigateBack();
                }, 1000);
              }
            });
          },
          fail: () => {
            // 如果之前没有数据，创建新的列表
            const memoList = [];
            const newMemo = {
              id: new Date().getTime(),
              title: this.memoInfo.title,
              content: this.memoInfo.content,
              createTime: new Date().getTime(),
              updateTime: new Date().getTime()
            };
            memoList.push(newMemo);
            
            uni.setStorage({
              key: 'memoList',
              data: memoList,
              success: () => {
                uni.hideLoading();
                uni.showToast({
                  title: '添加成功',
                  icon: 'success'
                });
                
                setTimeout(() => {
                  uni.navigateBack();
                }, 1000);
              }
            });
          }
        });
      }, 800);
    }
  }
}
</script>

<style lang="scss">
.edit-container {
  min-height: 100vh;
  background-color: #fff;
  display: flex;
  flex-direction: column;
}

.edit-header {
  height: 88rpx;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  border-bottom: 1rpx solid #f2f2f2;
  padding: 0 30rpx;
  
  .header-title {
    font-size: 32rpx;
    font-weight: 500;
    color: #333;
  }
  
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
  
  .save-btn {
    position: absolute;
    right: 30rpx;
    top: 50%;
    transform: translateY(-50%);
    height: 60rpx;
    padding: 0 24rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #f0f2ff;
    border-radius: 30rpx;
    
    .save-text {
      font-size: 28rpx;
      color: #7e85dd;
      font-weight: 500;
    }
  }
}

.edit-content {
  flex: 1;
  padding: 30rpx;
  
  .memo-title-input {
    font-size: 36rpx;
    font-weight: bold;
    color: #333;
    padding: 20rpx 0;
    border-bottom: 1rpx solid #f2f2f2;
    margin-bottom: 30rpx;
  }
  
  .memo-content-textarea {
    width: 100%;
    font-size: 30rpx;
    color: #333;
    line-height: 1.6;
    padding: 10rpx 0;
    min-height: 400rpx;
  }
}
</style> 