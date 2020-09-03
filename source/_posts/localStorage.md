---
title: localStorage
date: 2020-09-03 18:48:03
tags:
    - javascript
categories:
    - javascript
---

## localStorage

### 简介

`localStorage`用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去删除。

### 语法

```javascript
window.localStorage
```

#### 存

```javascript
localStorage.setItem('key', 'value')
```

#### 取

```javascript
var nickname = localStorage.getItem('key')
```

#### 删除

```javascript
localStorage.removeItem('key')
```

### localStorage 与 JSON 的配合

```javascript
let value = [
  {
    id: 1,
    name: 'first'
  },
  {
    id: 2,
    name: 'second'
  },
  {
    id: 3,
    name: 'third'
  }
]
// 通过JSON.stringify将js的值序列化成JSON字符串
value = JSON.stringify(value)
// 存入 
localStorage.setItem('key', value)

// 取值
let value = localStorage.getItem('key')
// 通过JSON.parse将JSON字符串解析成js值
value = JSON.parse(value)
console.log(value)
```

### summary

1. `localStorage`只支持`String`类型的存储，如果存进去的是`Number`，再去取值的时候又会变成`String`
