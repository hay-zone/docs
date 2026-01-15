---
title: 小程序基础
date: 2026-01-14
tags:
  - 小程序
categories:
  - MiniProgram
---

# 小程序基础

## 1. 什么是小程序

微信小程序是一种无需下载安装即可使用的应用。

### 特点

- 体验接近原生 APP
- 无需安装
- 触手可及
- 用完即走

## 2. 文件结构

### 页面文件

- `.wxml`: 模板文件
- `.wxss`: 样式文件
- `.js`: 逻辑文件
- `.json`: 配置文件

### 全局文件

- `app.js`: 小程序逻辑
- `app.json`: 小程序配置
- `app.wxss`: 小程序样式

## 3. 生命周期

### 应用生命周期

- `onLaunch`: 初始化
- `onShow`: 显示
- `onHide`: 隐藏

### 页面生命周期

- `onLoad`: 加载
- `onReady`: 初次渲染完成
- `onShow`: 显示
- `onHide`: 隐藏
- `onUnload`: 卸载

## 4. 数据绑定

```javascript
Page({
  data: {
    message: 'Hello World',
  },

  updateMessage() {
    this.setData({
      message: 'New Message',
    });
  },
});
```
