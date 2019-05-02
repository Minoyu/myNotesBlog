---
title: 我的JavaScript简略笔记04(标准库)
date: 2019-01-25 18:10:51
tags: [《JavaScript 标准参考教程-阮一峰》,学习笔记]
categories: [学习笔记,JavaScript]
---
# 标准库
## Object对象
### 概述
JavaScript 的所有其他对象都继承自Object对象，即那些对象都是Object的实例。
Object对象的原生方法分成两类：Object本身的方法与Object的实例方法。
#### Object对象本身的方法
所谓“本身的方法”就是直接定义在Object对象的方法。
```js
Object.print = function (o) { console.log(o) };
```
#### Object的实例方法
所谓实例方法就是定义在Object原型对象Object.prototype上的方法。它可以被Object实例直接使用。
```js
Object.prototype.print = function () {
  console.log(this);
};

var obj = new Object();
obj.print() // Object
```
### Object()
Object本身是一个函数，可以当作工具方法使用，将任意值转为对象。这个方法常用于保证某个值一定是对象。

> PS. `instanceof` 运算符用来验证，一个对象是否为指定的构造函数的实例。

如果参数是原始类型的值，Object方法将其转为对应的包装对象的实例（参见《原始类型的包装对象》一章）。

```js
var obj = Object(1);
obj instanceof Object // true
obj instanceof Number // true

var obj = Object('foo');
obj instanceof Object // true
obj instanceof String // true

var obj = Object(true);
obj instanceof Object // true
obj instanceof Boolean // true
```
如果Object方法的参数是一个对象，它总是返回该对象，即不用转换。

### Object 构造函数
Object不仅可以当作工具函数使用，还可以当作构造函数使用，即前面可以使用new命令。

Object构造函数的首要用途，是直接通过它来生成新对象。
> 注意，通过`var obj = new Object()`的写法生成新对象，与字面量的写法`var obj = {}`是等价的。或者说，后者只是前者的一种简便写法。
Object构造函数的用法与工具方法很相似，几乎一模一样。使用时，可以接受一个参数，如果该参数是一个对象，则直接返回这个对象；如果是一个原始类型的值，则返回该值对应的包装对象（详见《包装对象》一章）。
### Object 的静态方法
所谓“静态方法”，是指部署在Object对象自身的方法。
#### Object.keys()，Object.getOwnPropertyNames()
`Object.keys`方法和`Object.getOwnPropertyNames`方法都用来遍历对象的属性。

对于一般的对象来说，Object.keys()和Object.getOwnPropertyNames()返回的结果是一样的。只有涉及不可枚举属性时，才会有不一样的结果。Object.keys方法只返回可枚举的属性（详见《对象属性的描述对象》一章），Object.getOwnPropertyNames方法还返回不可枚举的属性名。
```js
var a = ['Hello', 'World'];

Object.keys(a) // ["0", "1"]
Object.getOwnPropertyNames(a) // ["0", "1", "length"]
```
上面代码中，数组的length属性是不可枚举的属性，所以只出现在Object.getOwnPropertyNames方法的返回结果中。
#### 其他方法
除了上面提到的两个方法，Object还有不少其他静态方法，将在后文逐一详细介绍。

##### 对象属性模型的相关方法

+ Object.getOwnPropertyDescriptor()：获取某个属性的描述对象。
+ Object.defineProperty()：通过描述对象，定义某个属性。
+ Object.defineProperties()：通过描述对象，定义多个属性。

##### 控制对象状态的方法
+ Object.preventExtensions()：防止对象扩展。
+ Object.isExtensible()：判断对象是否可扩展。
+ Object.seal()：禁止对象配置。
+ Object.isSealed()：判断一个对象是否可配置。
+ Object.freeze()：冻结一个对象。
+ Object.isFrozen()：判断一个对象是否被冻结。

##### 原型链相关方法
+ Object.create()：该方法可以指定原型对象和属性，返回一个新的对象。
+ Object.getPrototypeOf()：获取对象的Prototype对象。
### Object 的实例方法
除了静态方法，还有不少方法定义在Object.prototype对象。它们称为实例方法，所有Object的实例对象都继承了这些方法。

Object实例对象的方法，主要有以下六个。

+ Object.prototype.valueOf()：返回当前对象对应的值。
+ Object.prototype.toString()：返回当前对象对应的字符串形式。
+ Object.prototype.toLocaleString()：返回当前对象对应的本地字符串形式。
+ Object.prototype.hasOwnProperty()：判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性。
+ Object.prototype.isPrototypeOf()：判断当前对象是否为另一个对象的原型。
+ Object.prototype.propertyIsEnumerable()：判断某个属性是否可枚举。

#### Object.prototype.valueOf()
valueOf方法的作用是返回一个对象的“值”，默认情况下返回对象本身。

valueOf方法的主要用途是，JavaScript 自动类型转换时会默认调用这个方法。
```js
var obj = new Object();
1 + obj // "1[object Object]"

var obj = new Object();
obj.valueOf = function () {
  return 2;
};

1 + obj // 3
```
上面代码将对象obj与数字1相加，这时 JavaScript 就会默认调用valueOf()方法，求出obj的值再与1相加。所以，如果自定义valueOf方法，就可以得到想要的结果。

#### Object.prototype.toString()
toString方法的作用是返回一个对象的字符串形式，默认情况下返回类型字符串。对于一个对象调用toString方法，会返回字符串[object Object]，该字符串说明对象的类型。

字符串[object Object]本身没有太大的用处，但是通过自定义toString方法，可以让对象在自动类型转换时，得到想要的字符串形式。

```js
var obj = new Object();

obj.toString = function () {
  return 'hello';
};

obj + ' ' + 'world' // "hello world"
```

数组、字符串、函数、Date 对象都分别部署了自定义的toString方法，覆盖了Object.prototype.toString方法。

##### toString() 的应用：判断数据类型
由于实例对象可能会自定义toString方法，覆盖掉Object.prototype.toString方法，所以为了得到类型字符串，最好直接使用Object.prototype.toString方法。通过函数的call方法，可以在任意值上调用这个方法，帮助我们判断这个值的类型。

```js
Object.prototype.toString.call(value)
Object.prototype.toString.call(2) // "[object Number]"
Object.prototype.toString.call('') // "[object String]"
Object.prototype.toString.call(true) // "[object Boolean]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(Math) // "[object Math]"
Object.prototype.toString.call({}) // "[object Object]"
Object.prototype.toString.call([]) // "[object Array]"
```

