---
title: React Hooks
date: 2026-01-14
tags: 
 - Hooks
categories:
 - React
---
# React Hooks

::: tip 

React Hooks 是 React 16.8 引入的新特性,让函数组件也能使用状态和其他 React 特性。

:::

## 1. 什么是 Hooks

Hooks 是 React 16.8 引入的新特性,让函数组件也能使用状态和其他 React 特性。

## 2. 常用 Hooks

### useState
管理组件状态。

```jsx
const [count, setCount] = useState(0);
```

### useEffect
处理副作用(数据获取、订阅、DOM 操作等)。

```jsx
useEffect(() => {
  // 副作用代码
  return () => {
    // 清理函数
  };
}, [依赖项]);
```

### useContext
访问 Context。

### useMemo 和 useCallback
- `useMemo`: 缓存计算结果
- `useCallback`: 缓存函数引用

## 3. 自定义 Hooks

封装可复用的逻辑。
