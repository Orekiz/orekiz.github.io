---
title: let和const
date: 2020-07-26 17:05:58
tags:
    - javascript
    - es6
categories:
    - javascript
    - es6
---

## 1. let

### 基本用法

ES6新增了`let`命令，用来声明变量。它的用法类似于`var`，但是所声明的变量只在`let`命令所在的代码块内有效。

```javascript
{
  let a = 10
  var b = 1
}

a // ReferenceError: a is not defined.
b // 1
```

`for`循环很适合`let`命令。
下面代码`i`只在`for`循环体内有效。

```javascript
for (let i = 0; i < 10; i++){
  // ...
}

console.log(i)
// ReferenceError: a is not defined.
```

另外，`for`循环中，设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

```javascript
for (let i = 0; i < 3; i++){
  let i = 'abc'
  console.log(i)
}
```
上面代码正确运行，输出了3次`abc`。这表明函数内部的变量`i`于循环变量`i`不在同一个作用域，有各自单独的作用域。

### 不存在变量提升

`var`命令会发生“变量提升”现象，即变量可以在声明之前使用，值为`undefined`。这不符合逻辑。

为了纠正这种现象，`let`命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

```javascript
// var
console.log(foo) // undefined

// let
console.log(bar) // 报错ReferenceError
```

### 暂时性死区

只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

```javascript
var temp = 123

if (true) {
  temp = 'abc' // ReferenceError
  let temp
}
```
上面代码中，存在全局变量`temp`，但是块级作用域内`let`了一个局部变量`temp`，所以后者绑定在这个作用域中，所以在声明之前赋值会报错。

> ES6 明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

总之，暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

### 不允许重复声明

`let`不允许在相同作用域内，重复声明同一个变量

```javascript
// 报错
function func() {
  let a = 10
  var a = 1
}

// 报错
function func() {
  let a = 10
  let a = 1
}
```

因此不能在函数内部重新声明参数

```javascript
function func(arg) {
  let arg
}
func() // 报错

function func(arg) {
  {
    let arg
  }
}
func() // 不报错
```

## const

### 基本用法

`const`声明一个只读的常量。一旦声明，常量的值就不能改变。

```javascript
const PI = 3.1415
PI // 3.1415

PI = 3 // TypeError: Assignment to constant variable.
```
上面的代码表明改变常量的值会报错。

`const`声明的变量不得改变值，意味着`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。

```javascript
const foo
// SyntaxError: Missing initializer in const declaration
```
上面代码表示，对于`const`来说，只声明不赋值，就会报错。

`const`和`let`的作用域相同：只在声明所在的块级作用域内有效。

```javascript
if (true) {
  const MAX = 5
}

MAX // Uncaught ReferenceError: MAX is not defined
```

`const`命令声明的常量也是不提升，存在暂时性死区，只能在声明的位置后面使用。

```javascript
if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}
```
上面代码在常量`MAX`声明之前就调用，结果报错。

`const`声明的常量，也与`let`一样不可重复声明。

```javascript
var message = "Hello!";
let age = 25;

// 以下两行都会报错
const message = "Goodbye!";
const age = 30;
```

## 块级作用域

### 为什么需要块级作用域

ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

第一种场景，内层变量可能会覆盖外层变量。

```javascript
var temp = new Date()

function f () {
  console.log(temp)
  if (false) {
    var temp = 'hello world'
  }
}

f() // undefined
```
上面代码的原意是，`if`代码块的外部使用外层的`tmp`变量，内部使用内层的`tmp`变量。但是，函数`f`执行后，输出结果为`undefined`，原因在于变量提升，导致内层的`tmp`变量覆盖了外层的`tmp`变量。

第二种场景，用来计数的循环变量泄露为全局变量。

```javascript
var s = 'hello'

for (var i = 0; i < s.length; i++) {
  console.log(s[i])
}

console.log(i) // 5
```
上面代码中，变量`i`只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。

### ES6 的块级作用域

`let`实际上为 JavaScript 新增了块级作用域。

```javascript
function f1 () {
  let n = 5
  if (true) {
    let n = 10
  }
  console.log(n) // 5
}
```
上面的函数有两个代码块，都声明了变量`n`，运行后输出5。这表示外层代码块不受内层代码块的影响。如果两次都使用`var`定义变量`n`，最后输出的值才是10。

ES6 允许块级作用域的任意嵌套。

```javascript
{{{{
  { let insane = 'Hello world' }
  console.log(insane) // 报错
}}}}
```
上面代码使用了一个五层的块级作用域，每一层都是一个单独的作用域。第四层作用域无法读取第五层作用域的内部变量。

内层作用域可以定义外层作用域的同名变量。

```javascript
{{{{
  let insane = 'Hello World'
  {let insane = 'Hello World'}
}}}}
```

ES6 的块级作用域必须有大括号，如果没有大括号，JavaScript 引擎就认为不存在块级作用域。

```javascript
// 报错
if (true) let x = 1

// 不报错
if (true) {
  let x = 1
}
```

## summary

`let` 创建的是变量
`const` 创建的是常量
