---
title: 我的JavaScript简略学习笔记01(基本语法、数据类型)
date: 2018-11-08 04:12:11
tags: [《JavaScript 标准参考教程-阮一峰》,学习笔记]
categories: [学习笔记,JavaScript]
---
最近准备系统性的过一遍阮一峰的JavaScript教程，因为看的比较快速所以可能只能在这里简略的记录一些关键的地方了，希望这次能对JS的认识更全面一些吧。哈哈加油！那么下面开始咯。

总结自[阮一峰JavaScript教程](https://wangdoc.com/javascript/)

# 基本语法
## 一些概念
最前面的var，是变量声明命令。它表示通知解释引擎，要创建一个变量a。
```javascript 
var a = 1;
```
注意，JavaScript 的变量名区分大小写，A和a是两个不同的变量。

如果只是声明变量而没有赋值，则该变量的值是undefined。undefined是一个特殊的值，表示“无定义”。

如果变量赋值的时候，忘了写var命令，这条语句也是有效的。
```javascript
var a = 1;
// 基本等同
a = 1;
```
但是，不写var的做法，不利于表达意图，而且容易不知不觉地创建全局变量，所以建议总是使用`var`命令声明变量。

如果一个变量没有声明就直接使用，JavaScript 会报错，告诉你变量未定义。

**JavaScript 是一种动态类型语言，也就是说，变量的类型没有限制，变量可以随时更改类型。**

```javascript
var a = 1;
a = 'hello';
```

如果使用var重新声明一个已经存在的变量，是无效的。
但是，如果第二次声明的时候还进行了赋值，则会覆盖掉前面的值。
## 变量提升
JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。

```js
console.log(a);
var a = 1;
```
上面代码首先使用console.log方法，在控制台（console）显示变量a的值。这时变量a还没有声明和赋值，所以这是一种错误的做法，但是实际上不会报错。因为存在变量提升，真正运行的是下面的代码。

```js
var a;
console.log(a);
a = 1;
```
最后的结果是显示undefined，表示变量a已声明，但还未赋值。

## 标识符
简单说，标识符命名规则如下。
- 第一个字符，可以是任意 Unicode 字母（包括英文字母和其他语言的字母），以及美元符号（$）和下划线（_）。
- 第二个字符及后面的字符，除了 Unicode 字母、美元符号和下划线，还可以用数字0-9。

> JavaScript 有一些保留字，不能用作标识符：arguments、break、case、catch、class、const、continue、debugger、default、delete、do、else、enum、eval、export、extends、false、finally、for、function、if、implements、import、in、instanceof、interface、let、new、null、package、private、protected、public、return、static、super、switch、this、throw、true、try、typeof、var、void、while、with、yield。

## 区块
JavaScript 使用大括号，将多个相关的语句组合在一起，称为“区块”（block）。

对于var命令来说，JavaScript 的区块不构成单独的作用域（scope）。
```js
{
  var a = 1;
}

a // 1
```
上面代码在区块内部，使用var命令声明并赋值了变量a，然后在区块外部，变量a依然有效，区块对于var命令不构成单独的作用域，与不使用区块的情况没有任何区别。在 JavaScript 语言中，单独使用区块并不常见，区块往往用来构成其他更复杂的语法结构，比如for、if、while、function等。

## 条件语句
JavaScript 提供if结构和switch结构，完成条件判断，即只有满足预设的条件，才会执行相应的语句。

这部分简略

注意，if后面的表达式之中，不要混淆赋值表达式（`=`）、严格相等运算符（`===`）和相等运算符（`==`）。

至于为什么优先采用“严格相等运算符”（`===`），而不是“相等运算符”（`==`），请参考《运算符》章节。

## 循环语句
`while` `for` `do...while`

`break 语句和 continue 语句`
不再详细介绍
[传送门](https://wangdoc.com/javascript/basic/grammar.html#%E5%8F%98%E9%87%8F)

## 标签（label）
JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下。
```js
label:
  语句
```
标签可以是任意的标识符，但不能是保留字，语句部分可以是任意语句。

标签通常与break语句和continue语句配合使用，跳出特定的循环。

```js
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
```
上面代码为一个双重循环区块，break命令后面加上了top标签（注意，top不用加引号），满足条件时，直接跳出双层循环。如果break语句后面不使用标签，则只能跳出内层循环，进入下一次的外层循环。

标签也可以用于跳出代码块。

```js
foo: {
  console.log(1);
  break foo;
  console.log('本行不会输出');
}
console.log(2);
// 1
// 2
```
上面代码执行到break foo，就会跳出区块。

continue语句也可以与标签配合使用。
```js
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) continue top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
// i=2, j=0
// i=2, j=1
// i=2, j=2
```
上面代码中，continue命令后面有一个标签名，满足条件时，会跳过当前循环，直接进入下一轮外层循环。如果continue语句后面不使用标签，则只能进入下一轮的内层循环。

# 数据类型
## 概述
JavaScript 语言的每一个值，都属于某一种数据类型。JavaScript 的数据类型，共有六种。（ES6 又新增了第七种 Symbol 类型的值
- 数值（number）：整数和小数（比如1和3.14）
- 字符串（string）：文本（比如Hello World）。
- 布尔值（boolean）：表示真伪的两个特殊值，即true（真）和false（假）
- undefined：表示“未定义”或不存在，即由于目前没有定义，所以此处暂时没有任何值
- null：表示空值，即此处的值为空。
- 对象（object）：各种值组成的集合。

通常，**数值**、**字符串**、**布尔值**这三种类型，合称为原始类型（primitive type）的值，即它们是最基本的数据类型，不能再细分了。

对象则称为合成类型（complex type）的值，因为一个对象往往是多个原始类型的值的合成，可以看作是一个存放各种值的容器。

至于undefined和null，一般将它们看成两个特殊值。

对象是最复杂的数据类型，又可以分成三个子类型。

- 狭义的对象（object）
- 数组（array）
- 函数（function）

狭义的对象和数组是两种不同的数据组合方式，除非特别声明，本教程的”对象“都特指狭义的对象。函数其实是处理数据的方法，JavaScript 把它当成一种数据类型，可以赋值给变量，这为编程带来了很大的灵活性，也为 JavaScript 的“函数式编程”奠定了基础。

## typeof 运算符
JavaScript 有三种方法，可以确定一个值到底是什么类型。

- typeof运算符
- instanceof运算符
- Object.prototype.toString方法

```js
typeof 123 // "number"
typeof '123' // "string"
typeof false // "boolean"

function f() {}
typeof f
// "function"

typeof undefined
// "undefined"
```

利用这一点，typeof可以用来检查一个没有声明的变量，而不报错。

```js
v
// ReferenceError: v is not defined

typeof v
// "undefined"
```
上面代码中，变量v没有用var命令声明，直接使用就会报错。但是，放在typeof后面，就不报错了，而是返回undefined。

实际编程中，这个特点通常用在判断语句。

```js
// 错误的写法
if (v) {
  // ...
}
// ReferenceError: v is not defined

// 正确的写法
if (typeof v === "undefined") {
  // ...
}
```
对象返回`object`.
```js
typeof window // "object"
typeof {} // "object"
typeof [] // "object"
```

上面代码中，空数组（[]）的类型也是object，这表示在 JavaScript 内部，数组本质上只是一种特殊的对象。这里顺便提一下，instanceof运算符可以区分数组和对象。instanceof运算符的详细解释，请见《面向对象编程》一章。

```js
var o = {};
var a = [];

o instanceof Array // false
a instanceof Array // true
```

`null`返回`object`。

```js
typeof null // "object"
```
null的类型是object，这是由于历史原因造成的。1995年的 JavaScript 语言第一版，只设计了五种数据类型（对象、整数、浮点数、字符串和布尔值），没考虑null，只把它当作object的一种特殊值。后来null独立出来，作为一种单独的数据类型，为了兼容以前的代码，typeof null返回object就没法改变了。

## null 和 undefined
null与undefined都可以表示“没有”，含义非常相似。将一个变量赋值为undefined或null，老实说，语法效果几乎没区别。

在if语句中，它们都会被自动转为false，相等运算符（==）甚至直接报告两者相等。

```js
if (!undefined) {
  console.log('undefined is false');
}
// undefined is false

if (!null) {
  console.log('null is false');
}
// null is false

undefined == null
// true
```

从上面代码可见，两者的行为是何等相似！谷歌公司开发的 JavaScript 语言的替代品 Dart 语言，就明确规定只有null，没有undefined！

既然含义与用法都差不多，为什么要同时设置两个这样的值，这不是无端增加复杂度，令初学者困扰吗？这与历史原因有关。1995年 JavaScript 诞生时，最初像 Java 一样，只设置了null表示"无"。根据 C 语言的传统，null可以自动转为0。

```js
Number(null) // 0
5 + null // 5
```

但是，JavaScript 的设计者 Brendan Eich，觉得这样做还不够。首先，第一版的 JavaScript 里面，null就像在 Java 里一样，被当成一个对象，Brendan Eich 觉得表示“无”的值最好不是对象。其次，那时的 JavaScript 不包括错误处理机制，Brendan Eich 觉得，如果null自动转为0，很不容易发现错误。

因此，他又设计了一个undefined。区别是这样的：null是一个表示“空”的对象，转为数值时为0；undefined是一个表示"此处无定义"的原始值，转为数值时为NaN。

```js
Number(undefined) // NaN
5 + undefined // NaN
```

### 用法和定义
null表示空值，即该处的值现在为空。调用函数时，某个参数未设置任何值，这时就可以传入null，表示该参数为空。比如，某个函数接受引擎抛出的错误作为参数，如果运行过程中未出错，那么这个参数就会传入null，表示未发生错误。

undefined表示“未定义”。
```js
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于 undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
```

## 布尔值
“真”用关键字true表示，“假”用关键字false表示。布尔值只有这两个值。

下列运算符会返回布尔值：

- 前置逻辑运算符： ! (Not)
- 相等运算符：===，!==，==，!=
- 比较运算符：>，>=，<，<=

如果 JavaScript 预期某个位置应该是布尔值，会将该位置上现有的值自动转为布尔值。转换规则是除了下面六个值被转为false，其他值都视为true。

- undefined
- null
- false
- 0
- NaN
- ""或''（空字符串）

**注意，空数组（[]）和空对象（{}）对应的布尔值，都是true。**

## 数值
### 概述
#### 整数和浮点数
JavaScript 内部，所有数字都是以64位浮点数形式储存，即使整数也是如此。所以，1与1.0是相同的，是同一个数。

```js
1 === 1.0 // true
```

这就是说，JavaScript 语言的底层根本没有整数，所有数字都是小数（64位浮点数）。由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心。

```js
0.1 + 0.2 === 0.3
// false

0.3 / 0.1
// 2.9999999999999996

(0.3 - 0.2) === (0.2 - 0.1)
// false
```
#### 数值精度
根据国际标准 IEEE 754，JavaScript 浮点数的64个二进制位，从最左边开始，是这样组成的。

- 第1位：符号位，0表示正数，1表示负数
- 第2位到第12位（共11位）：指数部分
- 第13位到第64位（共52位）：小数部分（即有效数字）

符号位决定了一个数的正负，指数部分决定了数值的大小，小数部分决定了数值的精度。

指数部分一共有11个二进制位，因此大小范围就是0到2047。IEEE 754 规定，如果指数部分的值在0到2047之间（不含两个端点），那么有效数字的第一位默认总是1，不保存在64位浮点数之中。也就是说，有效数字这时总是1.xx...xx的形式，其中xx..xx的部分保存在64位浮点数之中，最长可能为52位。因此，JavaScript 提供的有效数字最长为53个二进制位。

精度最多只能到53个二进制位，这意味着，绝对值小于2的53次方的整数，即-253到253，都可以精确表示。

```js
Math.pow(2, 53)
// 9007199254740992
 
Math.pow(2, 53) + 1
// 9007199254740992

Math.pow(2, 53) + 2
// 9007199254740994

Math.pow(2, 53) + 3
// 9007199254740996

Math.pow(2, 53) + 4
// 9007199254740996
```
上面代码中，大于2的53次方以后，整数运算的结果开始出现错误。所以，大于2的53次方的数值，都无法保持精度。由于2的53次方是一个16位的十进制数值，所以简单的法则就是**，JavaScript 对15位的十进制数都可以精确处理**。

#### 数值范围
根据标准，64位浮点数的指数部分的长度是11个二进制位，意味着指数部分的最大值是2047（2的11次方减1）。也就是说，64位浮点数的指数部分的值最大为2047，分出一半表示负数，则 JavaScript 能够表示的数值范围为21024到2-1023（开区间），超出这个范围的数无法表示

如果一个数大于等于2的1024次方，那么就会发生“正向溢出”，即 JavaScript 无法表示这么大的数，这时就会返回Infinity。

```js
Math.pow(2, 1024) // Infinity
```
如果一个数小于等于2的-1075次方（指数部分最小值-1023，再加上小数部分的52位），那么就会发生为“负向溢出”，即 JavaScript 无法表示这么小的数，这时会直接返回0。
```js
Math.pow(2, -1075) // 0
```
JavaScript 提供Number对象的MAX_VALUE和MIN_VALUE属性，返回可以表示的具体的最大值和最小值。

```js
Number.MAX_VALUE // 1.7976931348623157e+308
Number.MIN_VALUE // 5e-324
```
### 数值的表示法
JavaScript 的数值有多种表示方法，可以用字面形式直接表示，比如35（十进制）和0xFF（十六进制）。数值也可以采用科学计数法表示.
```js
123e3 // 123000
123e-3 // 0.123
-3.1E+12
.1e-23
```

小数点前的数字多于21位 或 小数点后的零多于5个 两种情况下，JavaScript 会自动将数值转为科学计数法表示，其他情况都采用字面形式直接表示。

### 数值的进制
- 十进制：没有前导0的数值。
- 八进制：有前缀0o或0O的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值。
- 十六进制：有前缀0x或0X的数值。
- 二进制：有前缀0b或0B的数值。

前导0表示八进制，处理时很容易造成混乱。ES5 的严格模式和 ES6，已经废除了这种表示法，但是浏览器为了兼容以前的代码，目前还继续支持这种表示法。

### 特殊数值
JavaScript 提供了几个特殊的数值。

#### 正零和负零
JavaScript 内部实际上存在2个0：一个是+0，一个是-0，区别就是64位浮点数表示法的符号位不同。它们是等价的。
```js
-0 === +0 // true
0 === -0 // true
0 === +0 // true
```
几乎所有场合，正零和负零都会被当作正常的0。唯一有区别的场合是，+0或-0当作分母，返回的值是不相等的。之所以出现这样结果，是因为除以正零得到+Infinity，除以负零得到-Infinity，这两者是不相等的

```js
+0 // 0
-0 // 0
(-0).toString() // '0'
(+0).toString() // '0'
(1 / +0) === (1 / -0) // false
```

#### NaN
##### 含义
NaN是 JavaScript 的特殊值，表示“非数字”（Not a Number），主要出现在将字符串解析成数字出错的场合。

```js
5 - 'x' // NaN
```
另外，一些数学函数的运算结果会出现NaN。
```js
Math.acos(2) // NaN
Math.log(-1) // NaN
Math.sqrt(-1) // NaN
```
0除以0也会得到NaN。
```js
0/0//NaN
```
**需要注意的是，NaN不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于Number。**
```js
typeof NaN // 'number'
```
##### 运算规则
NaN不等于任何值，包括它本身。
```js
NaN === NaN // false
```
数组的indexOf方法内部使用的是严格相等运算符，所以该方法对NaN不成立。
```js
[NaN].indexOf(NaN) // -1
```
NaN在布尔运算时被当作false。
```js
Boolean(NaN) // false
```
NaN与任何数（包括它自己）的运算，得到的都是NaN。
```js
NaN + 32 // NaN
NaN - 32 // NaN
NaN * 32 // NaN
NaN / 32 // NaN
```
#### Infinity
##### 含义
Infinity表示“无穷”，用来表示两种场景。一种是一个正的数值太大，或一个负的数值太小，无法表示；另一种是非0数值除以0，得到Infinity。

Infinity有正负之分，Infinity表示正的无穷，-Infinity表示负的无穷。
```js
Infinity === -Infinity // false

1 / -0 // -Infinity
-1 / -0 // Infinity
```
Infinity大于一切数值（除了NaN），-Infinity小于一切数值（除了NaN）。

Infinity与NaN比较，总是返回false。
```js
Infinity > NaN // false
-Infinity > NaN // false

Infinity < NaN // false
-Infinity < NaN // false
```
##### 运算规则
Infinity的四则运算，符合无穷的数学计算规则。
```js
5 * Infinity // Infinity
5 - Infinity // -Infinity
Infinity / 5 // Infinity
5 / Infinity // 0
```

0乘以Infinity，返回NaN；0除以Infinity，返回0；Infinity除以0，返回Infinity。

Infinity加上或乘以Infinity，返回的还是Infinity。

Infinity减去或除以Infinity，得到NaN。

Infinity与null计算时，null会转成0，等同于与0的计算。
```js
null * Infinity // NaN
null / Infinity // 0
Infinity / null // Infinity
```

Infinity与undefined计算，返回的都是NaN。

### 与数值相关的全局方法
#### parseInt()
用于将字符串转为整数。
[详细用法](https://wangdoc.com/javascript/types/number.html#parseint)

#### parseFloat()
parseFloat方法用于将一个字符串转为浮点数。
[详细用法](https://wangdoc.com/javascript/types/number.html#parsefloat)

#### isNaN()
isNaN方法可以用来判断一个值是否为NaN。
```js
isNaN(NaN) // true
isNaN(123) // false
```
但是，isNaN只对数值有效，如果传入其他值，会被先转成数值。比如，传入字符串的时候，字符串会被先转成NaN，所以最后返回true，这一点要特别引起注意。也就是说，isNaN为true的值，有可能不是NaN，而是一个字符串。

出于同样的原因，对于对象和数组，isNaN也返回true。

但是，对于空数组和只有一个数值成员的数组，isNaN返回false。

[详细用法](https://wangdoc.com/javascript/types/number.html#isnan)

#### isFinite()
isFinite方法返回一个布尔值，表示某个值是否为正常的数值。

除了Infinity、-Infinity、NaN和undefined这几个值会返回false，isFinite对于其他的数值都会返回true。

```js
isFinite(Infinity) // false
isFinite(-Infinity) // false
isFinite(NaN) // false
isFinite(undefined) // false
isFinite(null) // true
isFinite(-1) // true
```

## 字符串
js的字符串可以放在单引号和双引号中

单引号字符串的内部，可以使用双引号。双引号字符串的内部，可以使用单引号。

```js
'key = "value"'
"It's a long journey"
```
上面两个都是合法的字符串。

- 如果要在单引号字符串的内部，使用单引号，就必须在内部的单引号前面加上反斜杠(`\`)，用来转义。
- 字符串默认只能写在一行内，分成多行将会报错。
- 如果长字符串必须分成多行，可以在每一行的尾部使用反斜杠。注意，反斜杠的后面必须是换行符，而不能有其他字符（比如空格），否则会报错。

### 转义
需要用反斜杠转义的特殊字符，主要有下面这些。

- `\0` ：null（\u0000）
- `\b` ：后退键（\u0008）
- `\f` ：换页符（\u000C）
- `\n` ：换行符（\u000A）
- `\r` ：回车键（\u000D）
- `\t` ：制表符（\u0009）
- `\v` ：垂直制表符（\u000B）
- `\'` ：单引号（\u0027）
- `\"` ：双引号（\u0022）
- `\\` ：反斜杠（\u005C）

### 字符串与数组
字符串可以被视为字符数组，因此可以使用数组的方括号运算符，用来返回某个位置的字符（位置编号从0开始）。

但是，字符串与数组的相似性仅此而已。实际上，无法改变字符串之中的单个字符。

### length 属性
length属性返回字符串的长度，该属性也是无法改变的。

### 字符集
JavaScript 使用 Unicode 字符集。JavaScript 引擎内部，所有字符都用 Unicode 表示。
[详细](https://wangdoc.com/javascript/types/string.html#%E5%AD%97%E7%AC%A6%E9%9B%86)

### Base64转码
有时，文本里面包含一些不可打印的符号，比如 ASCII 码0到31的符号都无法打印出来，这时可以使用 Base64 编码，将它们转成可以打印的字符。另一个场景是，有时需要以文本格式传递二进制数据，那么也可以使用 Base64 编码。

所谓 Base64 就是一种编码方法，可以将任意值转成 0～9、A～Z、a-z、+和/这64个字符组成的可打印字符。使用它的主要目的，不是为了加密，而是为了不出现特殊字符，简化程序的处理。

两个 Base64 相关的方法。

- btoa()：任意值转为 Base64 编码
- atob()：Base64 编码转为原来的值

注意，这两个方法不适合非 ASCII 码的字符（如中文），会报错。

要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法。

```js
function b64Encode(str) {
  return btoa(encodeURIComponent(str));
}

function b64Decode(str) {
  return decodeURIComponent(atob(str));
}

b64Encode('你好') // "JUU0JUJEJUEwJUU1JUE1JUJE"
b64Decode('JUU0JUJEJUEwJUU1JUE1JUJE') // "你好"
```

[有关URI编码的详解](https://segmentfault.com/a/1190000010433195)
## 对象
### 概述
#### 生成方法
简单说，对象就是一组“键值对”（key-value）的集合，是一种无序的复合数据集合。
```js
var obj = {
  foo: 'Hello',
  bar: 'World'
};
```

#### 键名
- 对象的所有键名都是字符串（ES6 又引入了 Symbol 值也可以作为键名），所以加不加引号都可以。
- 如果键名是数值，会被自动转为字符串。
- 如果键名不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），且也不是数字，则必须加上引号，否则会报错。
- 对象的每一个键名又称为“属性”（property），它的“键值”可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为“方法”，它可以像函数那样调用。
```js
var obj = {
  p: function (x) {
    return 2 * x;
  }
};

obj.p(1) // 2
```
- 如果属性的值还是一个对象，就形成了链式引用。
```js
var o1 = {};
var o2 = { bar: 'hello' };

o1.foo = o2;
o1.foo.bar // "hello"
```
- 就像上面那样，属性可以动态创建，不必在对象声明时就指定。

#### 对象的引用
- 如果不同的变量名指向同一个对象，那么它们都是这个对象的引用，也就是说指向同一个内存地址。修改其中一个变量，会影响到其他所有变量。

#### 表达式还是语句？
对象采用大括号表示，这导致了一个问题：如果行首是一个大括号，它到底是表达式还是语句？

```js
{ foo: 123 }
```

JavaScript 引擎读到上面这行代码，会发现可能有两种含义。第一种可能是，这是一个表达式，表示一个包含foo属性的对象；第二种可能是，这是一个语句，表示一个代码区块，里面有一个标签foo，指向表达式123。

为了避免这种歧义，V8 引擎规定，如果行首是大括号，一律解释为对象。不过，为了避免歧义，最好在大括号前加上圆括号。

```js
({ foo: 123})
```
### 属性的操作
#### 属性的读取
读取对象的属性，有两种方法，一种是使用点运算符，还有一种是使用方括号运算符。

- 请注意，如果使用方括号运算符，键名必须放在引号里面，否则会被当作变量处理。
- 数字键可以不加引号，因为会自动转成字符串。数值键名不能使用点运算符（因为会被当成小数点），只能使用方括号运算符。

#### 属性的查看
查看一个对象本身的所有属性，可以使用`Object.keys`方法。

#### 属性的删除：delete 命令
delete命令用于删除对象的属性，删除成功后返回true。
```js
var obj = { p: 1 };
Object.keys(obj) // ["p"]

delete obj.p // true
obj.p // undefined
Object.keys(obj) // []
```
- 注意，删除一个不存在的属性，delete不报错，而且返回true。
- 只有一种情况，delete命令会返回false，那就是该属性存在，且不得删除。

```js
var obj = Object.defineProperty({}, 'p', {
  value: 123,
  configurable: false
});

obj.p // 123
delete obj.p // false
```
- 另外，需要注意的是，delete命令只能删除对象本身的属性，无法删除继承的属性。

#### 属性是否存在：in 运算符
in运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回true，否则返回false。它的左边是一个字符串，表示属性名，右边是一个对象。
```js
var obj = { p: 1 };
'p' in obj // true
'toString' in obj // true
```
- in运算符的一个问题是，它不能识别哪些属性是对象自身的，哪些属性是继承的。这时，可以使用对象的hasOwnProperty方法判断一下，是否为对象自身的属性。


#### 属性的遍历：for...in 循环
for...in循环用来遍历一个对象的全部属性。
```js
var obj = {a: 1, b: 2, c: 3};

for (var i in obj) {
  console.log('键名：', i);
  console.log('键值：', obj[i]);
}
// 键名： a
// 键值： 1
// 键名： b
// 键值： 2
// 键名： c
// 键值： 3
```

for...in循环有两个使用注意点。

- 它遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性。
- 它不仅遍历对象自身的属性，还遍历继承的属性。

### with语句
with语句的格式如下：
```js
with (对象) {
  语句;
}
```
它的作用是操作同一个对象的多个属性时，提供一些书写的方便。
```js
// 例一
var obj = {
  p1: 1,
  p2: 2,
};
with (obj) {
  p1 = 4;
  p2 = 5;
}
// 等同于
obj.p1 = 4;
obj.p2 = 5;

// 例二
with (document.links[0]){
  console.log(href);
  console.log(title);
  console.log(style);
}
// 等同于
console.log(document.links[0].href);
console.log(document.links[0].title);
console.log(document.links[0].style);
```
- 注意，如果with区块内部有变量的赋值操作，必须是当前对象已经存在的属性，否则会创造一个当前作用域的全局变量。

## 函数
### 作用域
作用域（scope）指的是变量存在的范围。在 ES5 的规范中，Javascript 只有两种作用域：一种是全局作用域，变量在整个程序中一直存在，所有地方都可以读取；另一种是函数作用域，变量只在函数内部存在。

ES6 又新增了块级作用域

- 函数内部定义的变量，会在该作用域内覆盖同名全局变量。
- 注意，对于var命令来说，局部变量只能在函数内部声明，在其他区块中声明，一律都是全局变量。

#### 函数内部的变量提升
与全局作用域一样，函数作用域内部也会产生“变量提升”现象.var命令声明的变量，不管在什么位置，变量声明都会被提升到函数体的头部。

#### 函数本身的作用域
函数本身也是一个值，也有自己的作用域。它的作用域与变量一样，就是其声明时所在的作用域，与其运行时所在的作用域无关。

```js
var a = 1;
var x = function () {
  console.log(a);
};

function f() {
  var a = 2;
  x();
}

f() // 1
```
上面代码中，函数x是在函数f的外部声明的，所以它的作用域绑定外层，内部变量a不会到函数f体内取值，所以输出1，而不是2。

总之，函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。

同样的，函数体内部声明的函数，作用域绑定函数体内部。

```js
function foo() {
  var x = 1;
  function bar() {
    console.log(x);
  }
  return bar;
}

var x = 2;
var f = foo();
f() // 1
```
上面代码中，函数foo内部声明了一个函数bar，bar的作用域绑定foo。当我们在foo外部取出bar执行时，变量x指向的是foo内部的x，而不是foo外部的x。正是这种机制，构成了下文要讲解的“闭包”现象。

### 参数
#### 参数的省略
- 函数参数不是必需的，Javascript 允许省略参数，不会报错。
- 省略的参数的值就变为undefined。需要注意的是，函数的length属性与实际传入的参数个数无关，只反映函数预期传入的参数个数。

但是，没有办法只省略靠前的参数，而保留靠后的参数。如果一定要省略靠前的参数，只有显式传入undefined。
#### 传递方式
- 函数参数如果是原始类型的值（数值、字符串、布尔值），传递方式是传值传递（passes by value）。这意味着，在函数体内修改参数值，不会影响到函数外部。
- 如果函数参数是复合类型的值（数组、对象、其他函数），传递方式是传址传递（pass by reference）。也就是说，传入函数的原始值的地址，因此在函数内部修改参数，将会影响到原始值。

注意，如果函数内部修改的，不是参数对象的某个属性，而是替换掉整个参数，这时不会影响到原始值。
```js
var obj = [1, 2, 3];

function f(o) {
  o = [2, 3, 4];
}
f(obj);

obj // [1, 2, 3]
```
上面代码中，在函数f内部，参数对象obj被整个替换成另一个值。这时不会影响到原始值。这是因为，形式参数（o）的值实际是参数obj的地址，重新对o赋值导致o指向另一个地址，保存在原地址上的值当然不受影响。

#### 同名参数
如果有同名的参数，则取最后出现的那个值。

```js
function f(a, a) {
  console.log(a);
}

f(1) // undefined
```
调用函数f的时候，没有提供第二个参数，a的取值就变成了undefined。这时，如果要获得第一个a的值，可以使用arguments对象。
```js
function f(a, a) {
  console.log(arguments[0]);
}

f(1) // 1
```

#### arguments 对象
由于 JavaScript 允许函数有不定数目的参数，所以需要一种机制，可以在函数体内部读取所有参数。这就是arguments对象的由来。

arguments对象包含了函数运行时的所有参数，arguments[0]就是第一个参数，arguments[1]就是第二个参数，以此类推。这个对象只有在函数体内部，才可以使用。

- 正常模式下，arguments对象可以在运行时修改。
```js
var f = function(a, b) {
  arguments[0] = 3;
  arguments[1] = 2;
  return a + b;
}

f(1, 1) // 5
```
- 严格模式下，arguments对象是一个只读对象，修改它是无效的，但不会报错。
```js
var f = function(a, b) {
  'use strict'; // 开启严格模式
  arguments[0] = 3; // 无效
  arguments[1] = 2; // 无效
  return a + b;
}

f(1, 1) // 2
```
上面代码中，函数体内是严格模式，这时修改arguments对象就是无效的。

通过arguments对象的length属性，可以判断函数调用时到底带几个参数。

##### 与数组的关系
需要注意的是，虽然arguments很像数组，但它是一个对象。数组专有的方法（比如slice和forEach），不能在arguments对象上直接使用。

如果要让arguments对象使用数组方法，真正的解决方法是将arguments转为真正的数组。下面是两种常用的转换方法：slice方法和逐一填入新数组。

```js
var args = Array.prototype.slice.call(arguments);

// 或者
var args = [];
for (var i = 0; i < arguments.length; i++) {
  args.push(arguments[i]);
}
```

##### callee 属性
arguments对象带有一个callee属性，返回它所对应的原函数。

### 闭包
前面提到，JavaScript 有两种作用域：全局作用域和函数作用域。函数内部可以直接读取全局变量。但是，函数外部无法读取函数内部声明的变量。

如果出于种种原因，需要得到函数内的局部变量。正常情况下，这是办不到的，只有通过变通方法才能实现。那就是在函数的内部，再定义一个函数。

```js
function f1() {
  var n = 999;
  function f2() {
　　console.log(n); // 999
  }
}
```

既然f2可以读取f1的局部变量，那么只要把f2作为返回值，我们不就可以在f1外部读取它的内部变量了吗！

```js
function f1() {
  var n = 999;
  function f2() {
    console.log(n);
  }
  return f2;
}

var result = f1();
result(); // 999
```
上面代码中，函数f1的返回值就是函数f2，由于f2可以读取f1的内部变量，所以就可以在外部获得f1的内部变量了。

闭包就是函数f2，即能够读取其他函数内部变量的函数。由于在 JavaScript 语言中，只有函数内部的子函数才能读取内部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”。

闭包最大的特点，就是它可以“记住”诞生的环境，比如f2记住了它诞生的环境f1，所以从f2可以得到f1的内部变量。在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

闭包的最大用处有两个
- 一个是可以读取函数内部的变量
- 另一个就是让这些变量始终保持在内存中，即闭包可以使得它诞生环境一直存在。
请看下面的例子，闭包使得内部变量记住上一次调用时的运算结果。
```js
function createIncrementor(start) {
  return function () {
    return start++;
  };
}

var inc = createIncrementor(5);

inc() // 5
inc() // 6
inc() // 7
```
为什么会这样呢？原因就在于inc始终在内存中，而inc的存在依赖于createIncrementor，因此也始终在内存中，不会在调用结束后，被垃圾回收机制回收。

- 闭包的另一个用处，是封装对象的私有属性和私有方法。
```js
function Person(name) {
  var _age;
  function setAge(n) {
    _age = n;
  }
  function getAge() {
    return _age;
  }

  return {
    name: name,
    getAge: getAge,
    setAge: setAge
  };
}

var p1 = Person('张三');
p1.setAge(25);
p1.getAge() // 25
```
上面代码中，函数Person的内部变量_age，通过闭包getAge和setAge，变成了返回对象p1的私有变量。

注意，外层函数每次运行，都会生成一个新的闭包，而这个闭包又会保留外层函数的内部变量，所以内存消耗很大。因此不能滥用闭包，否则会造成网页的性能问题。

### 立即调用的函数表达式（IIFE)
在 Javascript 中，圆括号()是一种运算符，跟在函数名之后，表示调用该函数。比如，print()就表示调用print函数。

```js
(function(){ /* code */ }());
// 或者
(function(){ /* code */ })();
```
这就叫做“立即调用的函数表达式”（Immediately-Invoked Function Expression）

上面两种写法都是以圆括号开头，引擎就会认为后面跟的是一个表示式，而不是函数定义语句.

通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。它的目的有两个：一是不必为函数命名，避免了污染全局变量；二是 IIFE 内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。
```js
// 写法一
var tmp = newData;
processData(tmp);
storeData(tmp);

// 写法二
(function () {
  var tmp = newData;
  processData(tmp);
  storeData(tmp);
}());
```
上面代码中，写法二比写法一更好，因为完全避免了污染全局变量。

## 数组
- 任何类型的数据，都可以放入数组。
```js
var arr = [
  {a: 1},
  [1, 2, 3],
  function() {return true;}
];

arr[0] // Object {a: 1}
arr[1] // [1, 2, 3]
arr[2] // function (){return true;}
```
上面数组arr的3个成员依次是对象、数组、函数。

- 如果数组的元素还是数组，就形成了多维数组。

本质上，数组属于一种特殊的对象。typeof运算符会返回数组的类型是object。数组的特殊性体现在，它的键名是按次序排列的一组整数（0，1，2...）
```js
var arr = ['a', 'b', 'c'];

Object.keys(arr)
// ["0", "1", "2"]
```

- 对象有两种读取成员的方法：点结构（object.key）和方括号结构（object[key]）。但是，对于数值的键名，不能使用点结构。

### length属性
数组的length属性，返回数组的成员数量。

但需要注意的是，数组的数字键不需要连续，length属性的值总是比最大的那个整数键大1。
```js
var arr = ['a', 'b'];
arr.length // 2

arr[2] = 'c';
arr.length // 3

arr[9] = 'd';
arr.length // 10

arr[1000] = 'e';
arr.length // 1001

```
length属性是可写的。如果人为设置一个小于当前成员个数的值，该数组的成员会自动减少到length设置的值。清空数组的一个有效方法，就是将length属性设为0。

```js
var arr = [ 'a', 'b', 'c' ];
arr.length // 3

arr.length = 2;
arr // ["a", "b"]
```

### in运算符
检查某个键名是否存在的运算符in，适用于对象，也适用于数组。
```js
var arr = [ 'a', 'b', 'c' ];
2 in arr  // true
'2' in arr // true
4 in arr // false
```

### for...in 循环和数组的遍历
for...in循环不仅可以遍历对象，也可以遍历数组，毕竟数组只是一种特殊对象。
```js
var a = [1, 2, 3];

for (var i in a) {
  console.log(a[i]);
}
// 1
// 2
// 3
```
但是，for...in不仅会遍历数组所有的数字键，还会遍历非数字键。

所以，不推荐使用for...in遍历数组。数组的遍历可以考虑使用for循环或while循环。

数组的forEach方法，也可以用来遍历数组，详见《标准库》的 Array 对象一章

```js
var colors = ['red', 'green', 'blue'];
colors.forEach(function (color) {
  console.log(color);
});
// red
// green
// blue
```
### 空位
length属性不过滤空位。所以，使用length属性进行数组遍历，一定要非常小心。

数组的某个位置是空位，与某个位置是undefined，是不一样的。如果是空位，使用数组的forEach方法、for...in结构、以及Object.keys方法进行遍历，空位都会被跳过。

这就是说，空位就是数组没有这个元素，所以不会被遍历到，而undefined则表示数组有这个元素，值是undefined，所以遍历不会跳过。

### 类似数组的对象
数组的slice方法可以将“类似数组的对象”变成真正的数组。
```js
var arr = Array.prototype.slice.call(arrayLike);
```
“类似数组的对象”还有一个办法可以使用数组的方法，就是通过call()把数组的方法放到对象上面。

```js
function print(value, index) {
  console.log(index + ' : ' + value);
}

Array.prototype.forEach.call(arrayLike, print);
```
上面代码中，arrayLike代表一个类似数组的对象，本来是不可以使用数组的forEach()方法的，但是通过call()，可以把forEach()嫁接到arrayLike上面调用。

下面的例子就是通过这种方法，在arguments对象上面调用forEach方法。
```js
// forEach 方法
function logArgs() {
  Array.prototype.forEach.call(arguments, function (elem, i) {
    console.log(i + '. ' + elem);
  });
}

// 等同于 for 循环
function logArgs() {
  for (var i = 0; i < arguments.length; i++) {
    console.log(i + '. ' + arguments[i]);
  }
}
```

字符串也是类似数组的对象，所以也可以用Array.prototype.forEach.call遍历。

注意，这种方法比直接使用数组原生的forEach要慢，所以最好还是先将“类似数组的对象”转为真正的数组，然后再直接调用数组的forEach方法。
```js
//call
Array.prototype.forEach.call('abc', function (chr) {
  console.log(chr);
});
//slice
var arr = Array.prototype.slice.call('abc');
arr.forEach(function (chr) {
  console.log(chr);
});
```