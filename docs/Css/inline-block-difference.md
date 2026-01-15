---
title: 行内元素与块级元素的区别
date: 2026-01-15 15:30:00
categories:
  - Css
---

# 行内元素与块级元素的区别

---

## 面试题：行内元素和块级元素有什么区别？

**答案：** 行内元素不能设置宽高，取决于内容；块级元素可以设置宽高，默认继承父元素属性

**详细说明：**

### 1. 块级元素（Block-level Elements）

**特点：**

- **独占一行**：每个块级元素都会从新的一行开始
- **可以设置宽高**：width、height、margin、padding 都可以设置
- **默认宽度**：继承父元素的 100% 宽度（除非设置了 width）
- **垂直排列**：默认从上到下垂直排列

**常见块级元素：**

```html
<div>div</div>
<p>段落</p>
<h1>标题</h1>
<ul>列表</ul>
<li>列表项</li>
<table>表格</table>
<form>表单</form>
<header>头部</header>
<footer>底部</footer>
<section>区块</section>
```

**代码示例：**

```css
.block {
  display: block;
  width: 300px;       /* ✅ 可以设置 */
  height: 200px;      /* ✅ 可以设置 */
  margin: 20px;       /* ✅ 可以设置 */
  padding: 15px;      /* ✅ 可以设置 */
  background: lightblue;
}

/* 默认宽度继承父元素 */
.container {
  width: 600px;
}

.block-child {
  /* 不设置 width，默认为 600px */
  background: lightcoral;
}
```

### 2. 行内元素（Inline Elements）

**特点：**

- **不独占一行**：多个行内元素可以在同一行显示
- **不能设置宽高**：width、height 无效
- **宽高由内容决定**：根据内容自动计算
- **水平排列**：默认从左到右水平排列
- **垂直方向 margin 无效**：只有左右 margin 有效

**常见行内元素：**

```html
<span>span</span>
<a>链接</a>
<strong>加粗</strong>
<em>斜体</em>
<img>图片</img>
<input>输入框</input>
<button>按钮</button>
<label>标签</label>
```

**代码示例：**

```css
.inline {
  display: inline;
  width: 300px;       /* ❌ 无效 */
  height: 200px;      /* ❌ 无效 */
  margin: 20px;       /* ⚠️ 上下无效，只有左右有效 */
  padding: 15px;      /* ✅ 有效 */
  background: lightblue;
}

/* 宽高由内容决定 */
```

### 3. 行内块元素（Inline-block Elements）

**特点：**

- **结合两者优点**：既有行内元素的特性，又有块级元素的部分特性
- **可以设置宽高**：width、height 有效
- **不独占一行**：可以在同一行显示多个元素
- **支持垂直 margin**：上下左右 margin 都有效

**常见行内块元素：**

```html
<img>图片</img>
<button>按钮</button>
<input>输入框</input>
<select>下拉框</select>
<textarea>文本域</textarea>
```

**代码示例：**

```css
.inline-block {
  display: inline-block;
  width: 100px;       /* ✅ 可以设置 */
  height: 50px;       /* ✅ 可以设置 */
  margin: 10px;       /* ✅ 上下左右都有效 */
  padding: 5px;       /* ✅ 有效 */
  background: lightcoral;
}

/* 多个元素在同一行 */
```

### 4. display 属性对比表

| 特性 | Block | Inline | Inline-block |
|------|--------|---------|--------------|
| 独占一行 | ✅ | ❌ | ❌ |
| 设置宽高 | ✅ | ❌ | ✅ |
| 垂直 margin | ✅ | ❌ | ✅ |
| 水平排列 | ❌（垂直） | ✅（水平） | ✅（水平） |
| 宽度继承 | ✅（默认 100%） | ❌ | ❌ |
| 内容决定尺寸 | ❌ | ✅ | ❌ |

### 5. 实际应用场景

**场景 1：导航栏（使用 inline-block）**

```css
.nav {
  text-align: center;
}

.nav-item {
  display: inline-block;
  padding: 10px 20px;
  margin: 5px;
  background: #007bff;
  color: white;
}

/* 多个导航项在同一行 */
```

**场景 2：文章段落（使用 block）**

```css
p {
  display: block;
  margin: 20px 0;
  line-height: 1.6;
}

/* 每个段落独占一行 */
```

**场景 3：文本内强调（使用 inline）**

```css
.highlight {
  display: inline;
  background: yellow;
  padding: 2px 5px;
}

/* 在文本中高亮部分内容 */
```
