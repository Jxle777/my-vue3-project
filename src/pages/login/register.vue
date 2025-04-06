<template>
  <view class="register-container">
    <view class="register-header">
      <view class="back-icon" @click="goBack">
        <text class="icon">&#xe600;</text>
      </view>
      <text class="header-title">注册账号</text>
    </view>
    
    <view class="register-form">
      <view class="input-item">
        <text class="label">用户名</text>
        <input type="text" v-model="username" placeholder="请输入用户名" />
      </view>
      
      <view class="input-item">
        <text class="label">密码</text>
        <input type="password" v-model="password" placeholder="请输入密码(不少于6位)" />
      </view>
      
      <view class="input-item">
        <text class="label">确认密码</text>
        <input type="password" v-model="confirmPassword" placeholder="请再次输入密码" />
      </view>
      
      <view class="agreement">
        <checkbox-group @change="agreementChange">
          <label class="checkbox">
            <checkbox :checked="isAgree" />
            <text>我已阅读并同意</text>
            <text class="agreement-link" @click="viewAgreement">《用户协议》</text>
            <text>和</text>
            <text class="agreement-link" @click="viewPrivacy">《隐私政策》</text>
          </label>
        </checkbox-group>
      </view>
      
      <button class="submit-btn" :disabled="!isAgree" :class="{'disabled': !isAgree}" @click="register">立即注册</button>
      
      <view class="login-hint">
        <text>已有账号？</text>
        <text class="login-link" @click="goToLogin">立即登录</text>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue';

const username = ref('');
const password = ref('');
const confirmPassword = ref('');
const isAgree = ref(false);

// 返回上一页
const goBack = () => {
  uni.navigateBack();
};

// 协议同意状态更改
const agreementChange = (e) => {
  isAgree.value = e.detail.value.length > 0;
};

// 查看用户协议
const viewAgreement = () => {
  uni.showToast({
    title: '用户协议正在完善中',
    icon: 'none'
  });
};

// 查看隐私政策
const viewPrivacy = () => {
  uni.showToast({
    title: '隐私政策正在完善中',
    icon: 'none'
  });
};

// 注册
const register = () => {
  if (!isAgree.value) {
    uni.showToast({
      title: '请先同意用户协议和隐私政策',
      icon: 'none'
    });
    return;
  }
  
  if (!username.value) {
    uni.showToast({
      title: '请输入用户名',
      icon: 'none'
    });
    return;
  }
  
  if (!password.value || password.value.length < 6) {
    uni.showToast({
      title: '密码不能少于6位',
      icon: 'none'
    });
    return;
  }
  
  if (password.value !== confirmPassword.value) {
    uni.showToast({
      title: '两次密码输入不一致',
      icon: 'none'
    });
    return;
  }
  
  // 模拟注册请求
  uni.showLoading({
    title: '注册中...'
  });
  
  setTimeout(() => {
    uni.hideLoading();
    uni.showToast({
      title: '注册成功',
      icon: 'success'
    });
    
    // 注册成功后跳转到登录页
    setTimeout(() => {
      uni.redirectTo({
        url: '/pages/login/index'
      });
    }, 1500);
  }, 1000);
};

// 跳转到登录页
const goToLogin = () => {
  uni.redirectTo({
    url: '/pages/login/index'
  });
};
</script>

<style lang="scss">
.register-container {
  min-height: 100vh;
  background-color: #f8f8f8;
  padding-bottom: 40rpx;
}

.register-header {
  position: relative;
  height: 88rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #fff;
  
  .back-icon {
    position: absolute;
    left: 30rpx;
    top: 50%;
    transform: translateY(-50%);
    width: 60rpx;
    height: 60rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    
    .icon {
      font-size: 40rpx;
      font-family: 'iconfont';
    }
  }
  
  .header-title {
    font-size: 32rpx;
    font-weight: 500;
  }
}

.register-form {
  margin-top: 30rpx;
  padding: 0 30rpx;
}

.input-item {
  background-color: #fff;
  border-radius: 12rpx;
  padding: 20rpx 30rpx;
  margin-bottom: 20rpx;
  
  .label {
    font-size: 28rpx;
    color: #333;
    margin-bottom: 10rpx;
    display: block;
  }
  
  input {
    width: 100%;
    height: 88rpx;
    font-size: 28rpx;
  }
}

.agreement {
  margin-top: 20rpx;
  margin-bottom: 40rpx;
  
  .checkbox {
    font-size: 26rpx;
    color: #666;
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    
    checkbox {
      transform: scale(0.8);
      margin-right: 6rpx;
    }
    
    .agreement-link {
      color: #7e85dd;
      margin: 0 6rpx;
    }
  }
}

.submit-btn {
  width: 100%;
  height: 88rpx;
  background-color: #7e85dd;
  color: #fff;
  border-radius: 44rpx;
  font-size: 32rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 30rpx;
  
  &.disabled {
    background-color: #cccccc;
  }
}

.login-hint {
  text-align: center;
  font-size: 26rpx;
  color: #666;
  
  .login-link {
    color: #7e85dd;
    margin-left: 10rpx;
  }
}
</style> 