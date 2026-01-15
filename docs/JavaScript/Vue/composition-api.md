---
title: Vue Composition API
date: 2026-01-14
tags:
  - Vue
categories:
  - Vue
---

# Vue Composition API

## 1. 什么是 Composition API

Composition API 是 Vue 3 中引入的一组新的 API,用于更灵活地组织组件逻辑。

## 2. 核心 API

### setup()

组件的入口函数,在 `beforeCreate` 之前调用。

### ref() 和 reactive()

- `ref`: 创建响应式引用,适用于基本类型
- `reactive`: 创建响应式对象

### computed() 和 watch()

- `computed`: 计算属性
- `watch`: 侦听器

## 3. 优势

- 更好的类型推导
- 更灵活的代码组织
- 更容易复用逻辑
