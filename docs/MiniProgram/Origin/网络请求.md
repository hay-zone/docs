---
title: 网络请求
date: 2026-01-14
categories:
  - MiniProgram
---

# 小程序 API

## 1. 网络请求

### wx.request

发起 HTTPS 请求。

```javascript
wx.request({
  url: 'https://api.example.com/data',
  method: 'GET',
  success(res) {
    console.log(res.data);
  },
  fail(err) {
    console.error(err);
  },
});
```

### 上传下载

- `wx.uploadFile`: 上传文件
- `wx.downloadFile`: 下载文件

## 2. 本地存储

### wx.setStorage

异步存储数据。

```javascript
wx.setStorage({
  key: 'key',
  data: 'value',
});
```

### wx.setStorageSync

同步存储数据。

```javascript
wx.setStorageSync('key', 'value');
```

### 获取数据

- `wx.getStorage`: 异步获取
- `wx.getStorageSync`: 同步获取

## 3. 界面交互

### wx.showToast

显示提示框。

```javascript
wx.showToast({
  title: '成功',
  icon: 'success',
});
```

### wx.showModal

显示模态对话框。

### wx.showLoading

显示加载提示。

## 4. 导航

### wx.navigateTo

保留当前页面,跳转到新页面。

### wx.redirectTo

关闭当前页面,跳转到新页面。

### wx.switchTab

跳转到 tabBar 页面。

### wx.navigateBack

返回上一页。
