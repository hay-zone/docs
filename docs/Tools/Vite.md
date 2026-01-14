---
title: Vite 构建工具
date: 2026-01-14
tags: 
 - Vite
categories: 
 - Tools
---
# Vite 构建工具

## 1. 什么是 Vite

Vite 是新一代前端构建工具,利用浏览器原生 ES 模块特性。

### 特点
- 极速的服务启动
- 轻量快速的热重载(HMR)
- 真正的按需编译
- 丰富的功能

## 2. 核心原理

### 开发环境
- 基于 ES Modules
- 原生 ESM 按需加载
- esbuild 预构建依赖

### 生产环境
- 使用 Rollup 打包
- 代码分割
- CSS 代码分割

## 3. 基础配置

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
  plugins: [vue()],

  server: {
    port: 3000,
    open: true
  },

  build: {
    outDir: 'dist',
    assetsDir: 'assets'
  }
});
```

## 4. 常用插件

### @vitejs/plugin-vue
Vue 3 支持。

### @vitejs/plugin-react
React 支持。

### vite-plugin-pwa
PWA 支持。

### vite-plugin-compression
Gzip 压缩。

## 5. Vite vs Webpack

| 特性 | Vite | Webpack |
|------|------|---------|
| 启动速度 | 极快 | 较慢 |
| 热更新 | 极快 | 较快 |
| 生态 | 逐渐完善 | 非常丰富 |
| 配置 | 简单 | 复杂 |
| 生产构建 | Rollup | Webpack |
