<template>
  <view class="login-container">
    <view class="login-card">
      <view class="welcome-text">欢迎回来</view>
      <view class="login-hint">请登录您的账号</view>
      
      <view class="input-group">
        <view class="input-item">
          <text>用户名</text>
          <input type="text" placeholder="请输入用户名" v-model="username">
          <image class="input-icon" src="/static/images/user.svg"></image>
        </view>
        
        <view class="input-item">
          <text>密码</text>
          <input type="password" placeholder="请输入密码" v-model="password">
          <image class="input-icon" src="/static/images/lock.svg"></image>
        </view>
      </view>
      
      <view class="remember-row">
        <checkbox-group @change="rememberChange">
          <label class="checkbox">
            <checkbox :checked="rememberMe" />
            <text>记住我</text>
          </label>
        </checkbox-group>
        <text class="forget-pwd" @click="forgetPassword">忘记密码?</text>
      </view>
      
      <button class="login-btn" @click="handleLogin">登 录</button>
      
      <view class="register-hint">
        <text>还没有账号？</text>
        <text class="register-link" @click="goToRegister">立即注册</text>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue';

const username = ref('');
const password = ref('');
const rememberMe = ref(false);

const rememberChange = (e) => {
  rememberMe.value = e.detail.value.length > 0;
};

const handleLogin = () => {
  if (!username.value) {
    uni.showToast({
      title: '请输入用户名',
      icon: 'none'
    });
    return;
  }
  
  if (!password.value) {
    uni.showToast({
      title: '请输入密码',
      icon: 'none'
    });
    return;
  }
  
  // 这里添加登录逻辑
  uni.showLoading({
    title: '登录中...'
  });
  
  // 模拟登录请求
  setTimeout(() => {
    uni.hideLoading();
    uni.switchTab({
      url: '/pages/index/index'
    });
  }, 1500);
};

const forgetPassword = () => {
  uni.showToast({
    title: '该功能暂未开放',
    icon: 'none'
  });
};

const goToRegister = () => {
  uni.navigateTo({
    url: '/pages/login/register'
  });
};
</script>

<style lang="scss">
.login-container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #afb7ff;
  padding: 0 30rpx;
}

.login-card {
  width: 100%;
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 20rpx;
  padding: 50rpx 40rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.1);
}

.welcome-text {
  font-size: 40rpx;
  font-weight: bold;
  text-align: center;
  margin-bottom: 10rpx;
}

.login-hint {
  font-size: 28rpx;
  color: #666;
  text-align: center;
  margin-bottom: 60rpx;
}

.input-group {
  margin-bottom: 40rpx;
}

.input-item {
  position: relative;
  margin-bottom: 30rpx;
  
  text {
    font-size: 28rpx;
    color: #333;
    margin-bottom: 10rpx;
    display: block;
  }
  
  input {
    width: 100%;
    height: 88rpx;
    background-color: #f7f7f7;
    border-radius: 10rpx;
    padding: 0 80rpx 0 30rpx;
    font-size: 28rpx;
  }
  
  .input-icon {
    position: absolute;
    right: 30rpx;
    bottom: 24rpx;
    width: 40rpx;
    height: 40rpx;
  }
}

.remember-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 40rpx;
  
  .checkbox {
    font-size: 26rpx;
    color: #666;
    display: flex;
    align-items: center;
    
    checkbox {
      transform: scale(0.8);
      margin-right: 6rpx;
    }
  }
  
  .forget-pwd {
    font-size: 26rpx;
    color: #666;
  }
}

.login-btn {
  width: 100%;
  height: 88rpx;
  background-color: #7e85dd;
  color: #fff;
  border-radius: 44rpx;
  font-size: 32rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 40rpx;
}

.register-hint {
  text-align: center;
  font-size: 26rpx;
  color: #666;
  
  .register-link {
    color: #7e85dd;
    margin-left: 10rpx;
  }
}
</style> 