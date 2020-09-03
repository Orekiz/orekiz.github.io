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

### summary

1. `localStorage`只支持`String`类型的存储，如果存进去的是`Number`，再去取值的时候又会变成`String`
