---
title: 性能优化
date: 2026-01-14
tags:
  - 性能优化
categories:
  - React
---

# React 性能优化

## 1. 优化策略

### React.memo

对函数组件进行浅比较,避免不必要的重渲染。

### useMemo 和 useCallback

- `useMemo`: 缓存计算值
- `useCallback`: 缓存回调函数

### 虚拟列表

长列表渲染优化,只渲染可见区域。

## 2. 代码分割

### React.lazy 和 Suspense

动态导入组件,减小首屏加载体积。

```jsx
const Component = React.lazy(() => import('./Component'));

<Suspense fallback={<div>Loading...</div>}>
  <Component />
</Suspense>;
```

## 3. 避免常见性能陷阱

- 避免在渲染函数中创建新对象
- 合理使用 key
- 避免不必要的状态更新
