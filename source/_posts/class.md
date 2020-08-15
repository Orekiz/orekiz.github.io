---
title: class
date: 2020-08-15 14:20:58
tags:
    - javascript
    - es6
categories:
    - javascript
    - es6
---

## class

### 基本语法

```javascript
class Bar {
  doStuff () {
    console.log('stuff')
  }
}

const b = new Bar()
b.doStuff()  // 'stuff'
```

**类必须使用`new`调用，否则会报错**

### constructor 方法

`constructor`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。
一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。
```javascript
class Point {
}
// 等同于
class Point {
  constructor () {}
}
```
上面代码中，定义了一个空的类`Point`，JavaScript 引擎会自动为它添加一个空的`constructor`方法。

`constructor`方法默认返回实例对象（即`this`），完全可以指定返回另外一个对象。

```javascript
class Foo {
  constructor () {
    return Object.create(null)
  }
}

new Foo() instanceof Foo
// false
```
上面代码中，`constructor`函数返回一个全新的对象，结果导致实例对象不是`Foo`类中的实例。

### 类的实例

类的所有实例共享一个原型对象。

```javascript
const p1 = new Point(1,2)
const p2 = new Point(3,4)

p1.__proto__ === p2.__proto__
//true
```
上面代码中，`p1`和`p2`都是`Point`的实例，原形都是`Point.prototype`，所以`__proto__`属性是相等的。

### 取值函数（getter）和存值函数（setter）

在类的内部可以使用`set`和`get`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```javascript
class MyClass {
  constructor () {
    // ...
  }
  get prop() {
    return 'getter'
  }
  set prop (value) {
    console.log('setter:' + value)
  }
}

const inst = new MyClass()

inst.prop = 123
// setter: 123

isnt.prop
// 'getter'
```
上面代码中，`prop`属性有对应的存值函数和取值函数，因此赋值和读取行为都被自定义了。

### 属性表达式

类的属性名，可以采用表达式。
```javascript
const methodName = 'getArea'

class MyClass {
  constructor () {
    // ...
  }

  [methodName] () {
    // ...
  }
}
```
上面代码中，`MyClass`类的方法名`getArea`，是从表达式得到的。

### Class 表达式

与函数一样，类也可以使用表达式的形式定义。
```javascript
const MyClass = class Me {
  getClassName () {
    return Me.name
  }
}
```
上面代码使用表达式定义了一个类。需要注意的是，这个类的名字是`Me`，但是`Me`只在 Class 的内部可用，指代当前类。在 Class 外部，这个类只能用`MyClass`引用。







