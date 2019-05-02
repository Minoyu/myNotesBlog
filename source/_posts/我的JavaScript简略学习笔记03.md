---
title: 我的JavaScript简略学习笔记03(语法专题)
date: 2019-01-24 21:07:27
tags: [《JavaScript 标准参考教程-阮一峰》,学习笔记]
categories: [学习笔记,JavaScript]
---
# 语法专题
## 数据类型的转换
JavaScript 是一种动态类型语言，变量没有类型限制，可以随时赋予任意值。

虽然变量的数据类型是不确定的，但是各种运算符对数据类型是有要求的。如果运算符发现，运算子的类型与预期不符，就会自动转换类型。

### 强制转换 
强制转换主要指使用Number()、String()和Boolean()三个函数，手动将各种类型的值，分别转换成数字、字符串或者布尔值。
#### Number()
##### 对原始类型值
1. 字符串：如果不可以被解析为数值，返回 NaN
2. 空字符串转为0
3. 布尔值：true 转成 1，false 转成 0
4. undefined：转成 NaN
5. null：转成0

Number函数将字符串转为数值，要比parseInt函数严格很多。基本上，只要有一个字符无法转成数值，整个字符串就会被转为NaN。
```js
parseInt('42 cats') // 42
Number('42 cats') // NaN
```
另外，parseInt和Number函数都会自动过滤一个字符串前导和后缀的空格。
```js
parseInt('\t\v\r12.34\n') // 12
Number('\t\v\r12.34\n') // 12.34
```
##### 对对象
第一步，调用对象自身的valueOf方法。如果返回原始类型的值，则直接对该值使用Number函数，不再进行后续步骤。

第二步，如果valueOf方法返回的还是对象，则改为调用对象自身的toString方法。如果toString方法返回原始类型的值，则对该值使用Number函数，不再进行后续步骤。

第三步，如果toString方法返回的是对象，就报错。

*默认情况下，对象的valueOf方法返回对象本身，所以一般总是会调用toString方法*

#### String() 
String函数可以将任意类型的值转化成字符串，转换规则如下。
##### 原始类型值
+ 数值：转为相应的字符串。
+ 字符串：转换后还是原来的值。
+ 布尔值：true转为字符串"true"，false转为字符串"false"。
+ undefined：转为字符串"undefined"。
+ null：转为字符串"null"。
##### 对象
String方法的参数如果是对象，返回一个类型字符串；如果是数组，返回该数组的字符串形式。

String方法背后的转换规则，与Number方法基本相同，只是互换了valueOf方法和toString方法的执行顺序。

+ 先调用对象自身的toString方法。如果返回原始类型的值，则对该值使用String函数，不再进行以下步骤。
+ 如果toString方法返回的是对象，再调用原对象的valueOf方法。如果valueOf方法返回原始类型的值，则对该值使用String函数，不再进行以下步骤。
+ 如果valueOf方法返回的是对象，就报错。

#### Boolean() 
它的转换规则相对简单：除了以下五个值的转换结果为false，其他的值全部为true。

+ undefined
+ null
+ -0或+0
+ NaN
+ ''（空字符串）

**注意，所有对象（包括空对象）的转换结果都是true，甚至连false对应的布尔对象new Boolean(false)也是true（详见《原始类型值的包装对象》一章）。**

## 错误处理机制
JavaScript 解析或运行时，一旦发生错误，引擎就会抛出一个错误对象。JavaScript 原生提供Error构造函数，所有抛出的错误都是这个构造函数的实例。

JavaScript 语言标准只提到，Error实例对象必须有message属性，表示出错时的提示信息，没有提到其他属性。大多数 JavaScript 引擎，对Error实例还提供name和stack属性，分别表示错误的名称和错误的堆栈，但它们是非标准的，不是每种实现都有。

+ message：错误提示信息
+ name：错误名称（非标准属性）
+ stack：错误的堆栈（非标准属性）
### 原生错误类型 
Error实例对象是最一般的错误类型，在它的基础上，JavaScript 还定义了其他6种错误对象。也就是说，存在Error的6个派生对象。
#### SyntaxError 对象
SyntaxError对象是解析代码时发生的语法错误。
#### ReferenceError 对象
+ ReferenceError对象是引用一个不存在的变量时发生的错误。
+ 另一种触发场景是，将一个值分配给无法分配的对象，比如对函数的运行结果或者this赋值。
#### RangeError 对象
RangeError对象是一个值超出有效范围时发生的错误。主要有几种情况，一是数组长度为负数，二是Number对象的方法参数超出范围，以及函数堆栈超过最大值。
#### TypeError 对象
TypeError对象是变量或参数不是预期类型时发生的错误。比如，对字符串、布尔值、数值等原始类型的值使用new命令，就会抛出这种错误，因为new命令的参数应该是一个构造函数。

第二种情况，调用对象不存在的方法，也会抛出TypeError错误，因为obj.unknownMethod的值是undefined，而不是一个函数。
#### URIError 对象
URIError对象是 URI 相关函数的参数不正确时抛出的错误，主要涉及encodeURI()、decodeURI()、encodeURIComponent()、decodeURIComponent()、escape()和unescape()这六个函数。
#### EvalError 对象
eval函数没有被正确执行时，会抛出EvalError错误。该错误类型已经不再使用了，只是为了保证与以前代码兼容，才继续保留。
### 自定义错误
除了 JavaScript 原生提供的七种错误对象，还可以定义自己的错误对象。
```js
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

UserError.prototype = new Error();
UserError.prototype.constructor = UserError;
```
上面代码自定义一个错误对象UserError，让它继承Error对象。然后，就可以生成这种自定义类型的错误了。
```js
new UserError('这是自定义的错误！');
```
### throw 语句
throw语句的作用是手动中断程序执行，抛出一个错误。

实际上，throw可以抛出任何类型的值。也就是说，它的参数可以是任何值。

对于 JavaScript 引擎来说，遇到throw语句，程序就中止了。引擎会接收到throw抛出的信息，可能是一个错误实例，也可能是其他类型的值。
### try...catch 结构
```js
try {
  throw new Error('出错了!');
} catch (e) {
  console.log(e.name + ": " + e.message);
  console.log(e.stack);
}
// Error: 出错了!
//   at <anonymous>:3:9
//   ...
```
try...catch结构允许在最后添加一个finally代码块，表示不管是否出现错误，都必需在最后运行的语句。