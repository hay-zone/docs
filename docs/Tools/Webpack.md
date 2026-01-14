---
title: Webpack 构建工具
date: 2026-01-14
tags: 
 - Webpack
categories:
 - Tools
---
# Webpack 构建工具

## 1. 什么是 Webpack

Webpack 是一个现代 JavaScript 应用的静态模块打包工具。

### 核心概念
- Entry: 入口
- Output: 输出
- Loader: 转换器
- Plugin: 插件
- Mode: 模式

## 2. 基础配置

```javascript
module.exports = {
  entry: './src/index.js',

  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },

  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  },

  plugins: [
    new HtmlWebpackPlugin()
  ],

  mode: 'production'
};
```

## 3. 常用 Loader

### babel-loader
转换 ES6+ 代码。

### css-loader / style-loader
处理 CSS 文件。

### file-loader / url-loader
处理图片和字体。

### vue-loader
处理 Vue 单文件组件。

## 4. 常用 Plugin

### HtmlWebpackPlugin
生成 HTML 文件。

### MiniCssExtractPlugin
提取 CSS 到单独文件。

### CleanWebpackPlugin
清理输出目录。

### DefinePlugin
定义全局常量。

## 5. 性能优化

- 代码分割(Code Splitting)
- Tree Shaking
- 压缩代码
- 缓存(contenthash)
- DllPlugin
- 多进程打包
