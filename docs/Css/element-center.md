---
title: 元素快速居中
date: 2026-01-15 15:30:00
categories:
  - Css
---

# 元素快速居中

---

## 面试题：如何实现元素快速居中？

**答案：** 使用 Flexbox 布局

**实现方式：**

```css
/* 父元素设置 */
.parent {
  display: flex;
  justify-content: center; /* 水平居中 */
  align-items: center;     /* 垂直居中 */
  height: 100vh;         /* 可选：设置父元素高度 */
}

/* 子元素设置 */
.child {
  margin: auto;           /* 可选：配合 flex 使用 */
  width: 200px;         /* 设置子元素宽度 */
  height: 100px;        /* 设置子元素高度 */
}
```

**HTML 结构：**

```html
<div class="parent">
  <div class="child">居中内容</div>
</div>
```

**详细说明：**

1. **父元素 `display: flex`**：将父元素设置为弹性容器
2. **`justify-content: center`**：控制主轴（默认水平方向）上的对齐方式，实现水平居中
3. **`align-items: center`**：控制交叉轴（默认垂直方向）上的对齐方式，实现垂直居中
4. **子元素 `margin: auto`**：在 Flex 容器中，子元素的 margin: auto 会自动分配剩余空间

**其他居中方式对比：**

| 方式 | 代码 | 优点 | 缺点 |
|------|------|------|------|
| Flexbox | `display: flex` + `justify/align: center` | 简单、现代、支持任意尺寸 | 需要现代浏览器支持 |
| Grid | `display: grid` + `place-items: center` | 简洁、一行代码 | 需要现代浏览器支持 |
| 绝对位 | `position: absolute` + `top/left: 50%` + `transform: translate(-50%, -50%)` | 兼容性好 | 需要知道元素尺寸 |
| 表格 | `display: table-cell` + `vertical-align: middle` | 兼容老浏览器 | 语义不正确 |

**推荐：** 现代项目优先使用 Flexbox 方案，简洁高效。
