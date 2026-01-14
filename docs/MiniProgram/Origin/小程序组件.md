---
title: 小程序组件
date: 2026-01-14
categories:
  - MiniProgram
---

# 小程序组件

## 1. 内置组件

### 视图容器

- `view`: 视图容器
- `scroll-view`: 滚动视图
- `swiper`: 滑块视图

### 基础内容

- `text`: 文本
- `icon`: 图标
- `image`: 图片

### 表单组件

- `button`: 按钮
- `input`: 输入框
- `checkbox`: 复选框
- `radio`: 单选框
- `picker`: 选择器

## 2. 自定义组件

### 创建组件

```javascript
Component({
  properties: {
    title: String,
  },

  data: {
    count: 0,
  },

  methods: {
    increment() {
      this.setData({
        count: this.data.count + 1,
      });
    },
  },
});
```

### 使用组件

```json
{
  "usingComponents": {
    "my-component": "/components/my-component/index"
  }
}
```

```xml
<my-component title="标题" />
```

## 3. 组件通信

### 父传子

通过 properties。

### 子传父

通过事件。

```javascript
// 子组件触发事件
this.triggerEvent('myevent', { data: 'value' });

// 父组件监听
<my-component bind:myevent='handleEvent' />;
```
