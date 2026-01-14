---
title: TypeScript 最佳实践
date: 2026-01-14
categories:
  - TypeScript
---

# TypeScript 最佳实践

## 1. 配置建议

### tsconfig.json

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "esModuleInterop": true
  }
}
```

## 2. 类型守卫

### typeof

```typescript
function print(value: string | number) {
  if (typeof value === 'string') {
    console.log(value.toUpperCase());
  }
}
```

### instanceof

```typescript
if (obj instanceof MyClass) {
  // obj 是 MyClass 类型
}
```

### 自定义类型守卫

```typescript
function isString(value: any): value is string {
  return typeof value === 'string';
}
```

## 3. 避免使用 any

尽量使用 `unknown` 代替 `any`。

## 4. 合理使用类型断言

```typescript
const input = document.querySelector('input') as HTMLInputElement;
```

## 5. 利用类型推导

让 TypeScript 自动推导类型,减少显式类型注解。
