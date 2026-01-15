---
title: JavaScript 异步编程
date: 2026-01-15
categories:
  - JavaScript
---

# JavaScript 异步编程

---

## 一、概述

### 什么是异步编程？

异步编程是一种编程模式，允许程序在等待某些操作（如网络请求、文件读取、定时器等）完成时继续执行其他任务，而不是阻塞主线程。

**核心概念：**

- **非阻塞**：不会阻塞主线程的执行
- **事件驱动**：基于事件循环机制
- **回调机制**：通过回调函数处理异步结果
- **Promise**：更优雅的异步处理方式
- **async/await**：同步风格的异步代码

---

## 二、JavaScript 异步机制

### 1. 单线程与事件循环

JavaScript 是单线程语言，通过事件循环（Event Loop）实现异步操作。

**事件循环工作原理：**

```javascript
console.log('1');

setTimeout(() => {
  console.log('2');
}, 0);

console.log('3');

// 输出顺序：1 -> 3 -> 2
```

**执行顺序：**

1. 同步代码立即执行
2. 异步任务进入任务队列
3. 主线程空闲时从队列中取出任务执行

### 2. 宏任务与微任务

**宏任务（Macro Task）：**

- setTimeout
- setInterval
- setImmediate（Node.js）
- I/O 操作
- UI 渲染

**微任务（Micro Task）：**

- Promise.then/catch/finally
- process.nextTick（Node.js）
- MutationObserver

**执行顺序：**

```javascript
console.log('start');

setTimeout(() => {
  console.log('setTimeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('end');

// 输出：start -> end -> Promise -> setTimeout
```

---

## 三、异步编程的发展历程

### 1. 回调函数（Callback）

**基本用法：**

```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = { name: 'John', age: 30 };
    callback(data);
  }, 1000);
}

fetchData((data) => {
  console.log(data);
});
```

**回调地狱问题：**

```javascript
getData((data1) => {
  processData(data1, (data2) => {
    saveData(data2, (data3) => {
      console.log(data3);
    });
  });
});
```

**缺点：**

- 代码嵌套层级深，难以维护
- 错误处理困难
- 无法使用 try/catch

---

### 2. Promise

**基本用法：**

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve('操作成功');
    } else {
      reject('操作失败');
    }
  }, 1000);
});

promise
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.error(error);
  });
```

**Promise 链式调用：**

```javascript
fetchData()
  .then((data1) => processData(data1))
  .then((data2) => saveData(data2))
  .then((data3) => console.log(data3))
  .catch((error) => console.error(error));
```

**Promise 静态方法：**

```javascript
// Promise.all：所有 Promise 都成功才成功
Promise.all([promise1, promise2, promise3])
  .then((results) => console.log(results));

// Promise.race：第一个完成的 Promise 的结果
Promise.race([promise1, promise2])
  .then((result) => console.log(result));

// Promise.allSettled：所有 Promise 都完成（无论成功或失败）
Promise.allSettled([promise1, promise2])
  .then((results) => console.log(results));

// Promise.any：第一个成功的 Promise 的结果
Promise.any([promise1, promise2])
  .then((result) => console.log(result));
```

---

### 3. async/await

**基本用法：**

```javascript
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

fetchData();
```

**async 函数的特点：**

- 返回一个 Promise
- 可以使用 await 等待 Promise 结果
- 可以使用 try/catch 捕获错误

**await 的使用规则：**

```javascript
// ✅ 正确：在 async 函数中使用
async function example() {
  const result = await promise;
}

// ❌ 错误：在普通函数中使用
function example() {
  const result = await promise; // SyntaxError
}
```

**并行执行：**

```javascript
// 串行执行（慢）
async function serial() {
  const result1 = await promise1();
  const result2 = await promise2();
  const result3 = await promise3();
}

// 并行执行（快）
async function parallel() {
  const [result1, result2, result3] = await Promise.all([
    promise1(),
    promise2(),
    promise3()
  ]);
}
```

---

## 四、实际应用场景

### 1. 网络请求

**使用 fetch API：**

```javascript
async function getUserData(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    if (!response.ok) {
      throw new Error('网络请求失败');
    }
    const userData = await response.json();
    return userData;
  } catch (error) {
    console.error('获取用户数据失败:', error);
    throw error;
  }
}
```

**使用 axios：**

```javascript
import axios from 'axios';

async function getUserData(userId) {
  try {
    const response = await axios.get(`/api/users/${userId}`);
    return response.data;
  } catch (error) {
    console.error('获取用户数据失败:', error);
    throw error;
  }
}
```

### 2. 文件操作（Node.js）

```javascript
const fs = require('fs').promises;

async function readFile(filePath) {
  try {
    const content = await fs.readFile(filePath, 'utf8');
    return content;
  } catch (error) {
    console.error('读取文件失败:', error);
    throw error;
  }
}
```

### 3. 定时器

```javascript
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function example() {
  console.log('开始');
  await delay(1000); // 等待 1 秒
  console.log('1 秒后');
}
```

### 4. 防抖与节流

**防抖：**

```javascript
function debounce(func, delay) {
  let timeoutId;
  return function (...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}
```

**节流：**

```javascript
function throttle(func, delay) {
  let lastCall = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastCall >= delay) {
      lastCall = now;
      func.apply(this, args);
    }
  };
}
```

---

## 五、最佳实践

### 1. 错误处理

**统一错误处理：**

```javascript
class ApiError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
  }
}

async function fetchData() {
  try {
    const response = await fetch('/api/data');
    if (!response.ok) {
      throw new ApiError('请求失败', response.status);
    }
    return await response.json();
  } catch (error) {
    if (error instanceof ApiError) {
      console.error(`API 错误: ${error.message}`);
    } else {
      console.error('未知错误:', error);
    }
    throw error;
  }
}
```

### 2. 超时控制

```javascript
async function fetchWithTimeout(url, timeout = 5000) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeout);

  try {
    const response = await fetch(url, {
      signal: controller.signal
    });
    clearTimeout(timeoutId);
    return await response.json();
  } catch (error) {
    clearTimeout(timeoutId);
    if (error.name === 'AbortError') {
      throw new Error('请求超时');
    }
    throw error;
  }
}
```

### 3. 重试机制

```javascript
async function fetchWithRetry(url, maxRetries = 3, delay = 1000) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }
      return await response.json();
    } catch (error) {
      if (i === maxRetries - 1) {
        throw error;
      }
      await new Promise(resolve => setTimeout(resolve, delay * (i + 1)));
    }
  }
}
```

### 4. 并发控制

```javascript
async function concurrentLimit(tasks, limit = 3) {
  const results = [];
  const executing = [];

  for (const task of tasks) {
    const promise = task().then((result) => {
      executing.splice(executing.indexOf(promise), 1);
      return result;
    });

    results.push(promise);
    executing.push(promise);

    if (executing.length >= limit) {
      await Promise.race(executing);
    }
  }

  return Promise.all(results);
}
```

---

## 六、常见问题与解决方案

### 1. Promise 链中的错误丢失

**问题：**

```javascript
Promise.resolve()
  .then(() => {
    throw new Error('错误');
  })
  .then(() => {
    console.log('不会执行');
  });
```

**解决：**

```javascript
Promise.resolve()
  .then(() => {
    throw new Error('错误');
  })
  .catch((error) => {
    console.error(error);
  });
```

### 2. async 函数中的错误处理

**问题：**

```javascript
async function example() {
  await Promise.reject('错误');
  console.log('不会执行');
}
```

**解决：**

```javascript
async function example() {
  try {
    await Promise.reject('错误');
  } catch (error) {
    console.error(error);
  }
  console.log('会执行');
}
```

### 3. 循环中的异步操作

**问题：**

```javascript
for (let i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 100);
}
// 输出：5, 5, 5, 5, 5
```

**解决：**

```javascript
// 方案 1：使用 let
for (let i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 100);
}

// 方案 2：使用闭包
for (var i = 0; i < 5; i++) {
  ((i) => {
    setTimeout(() => console.log(i), 100);
  })(i);
}

// 方案 3：使用 async/await
for (let i = 0; i < 5; i++) {
  await new Promise(resolve => setTimeout(resolve, 100));
  console.log(i);
}
```

---

## 七、性能优化

### 1. 避免不必要的 await

**优化前：**

```javascript
async function example() {
  const result1 = await promise1();
  const result2 = await promise2();
  const result3 = await promise3();
}
```

**优化后：**

```javascript
async function example() {
  const [result1, result2, result3] = await Promise.all([
    promise1(),
    promise2(),
    promise3()
  ]);
}
```

### 2. 使用缓存

```javascript
const cache = new Map();

async function fetchData(url) {
  if (cache.has(url)) {
    return cache.get(url);
  }

  const data = await fetch(url).then(res => res.json());
  cache.set(url, data);
  return data;
}
```

### 3. 取消未完成的请求

```javascript
const controller = new AbortController();

async function fetchData() {
  try {
    const response = await fetch('/api/data', {
      signal: controller.signal
    });
    return await response.json();
  } catch (error) {
    if (error.name === 'AbortError') {
      console.log('请求已取消');
    }
  }
}

// 取消请求
controller.abort();
```

---

## 八、总结

### 异步编程要点：

1. **理解事件循环**：掌握宏任务和微任务的执行顺序
2. **选择合适的工具**：根据场景选择回调、Promise 或 async/await
3. **错误处理**：始终使用 try/catch 或 .catch() 处理错误
4. **性能优化**：合理使用并行执行、缓存和取消机制
5. **代码可读性**：优先使用 async/await 提高代码可读性

### 推荐实践：

- ✅ 优先使用 async/await
- ✅ 始终处理错误
- ✅ 合理使用 Promise.all 进行并行操作
- ✅ 添加超时和重试机制
- ✅ 避免回调地狱

---

## 九、扩展阅读

- [MDN: Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: async function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function)
- [JavaScript 事件循环详解](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
- [Node.js 事件循环](https://nodejs.org/zh-cn/docs/guides/event-loop-timers-and-nexttick/)
