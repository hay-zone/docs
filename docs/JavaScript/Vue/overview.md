---
title: Vue 概述
date: 2024-01-14
tags:
  - Vue
categories:
  - Vue
---

# Vue 概述

## 1. Vue 的核心特性

### 响应式系统

Vue 通过数据劫持结合发布-订阅模式实现数据的响应式。

### 组件化

组件是 Vue 最强大的功能之一,每个组件都有自己的逻辑和样式。

### 虚拟 DOM

Vue 使用虚拟 DOM 来提高渲染性能。

## 2. 生命周期钩子

- `beforeCreate`
- `created`
- `beforeMount`
- `mounted`
- `beforeUpdate`
- `updated`
- `beforeUnmount`
- `unmounted`

## 3. 指令

### v-if vs v-show

- `v-if`: 条件渲染,会真正销毁和重建
- `v-show`: 只是切换 display 属性
