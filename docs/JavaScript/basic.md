---
title: JavaScript 基础
date: 2026-01-14
tags:
  - JavaScript
categories:
  - JavaScript
---

# JavaScript 基础

## 1. 数据类型

### 原始类型

- String
- Number
- Boolean
- Undefined
- Null
- Symbol
- BigInt

### 引用类型

- Object
- Array
- Function

## 2. 作用域和闭包

### 作用域

- 全局作用域
- 函数作用域
- 块级作用域

### 闭包

函数和其词法环境的组合。

```javascript
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}
```

## 3. this 指向

- 默认绑定
- 隐式绑定
- 显式绑定(call, apply, bind)
- new 绑定
- 箭头函数

## 4. 原型和原型链

每个对象都有原型,形成原型链。

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function () {
  console.log(`Hi, I'm ${this.name}`);
};
```

## 5. window 对象上频繁绑定内容，有什么风险

#### 风险分析
- 命名冲突
- 全局污染
- 安全风险
- 性能问题，增加内存开销

#### 解决方案
- 模块化
- 命名空间
- 闭包，独立作用域
- 开启严格模式
