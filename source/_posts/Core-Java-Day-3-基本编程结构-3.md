---
title: Core Java Day 3--基本编程结构3(操作运算、字符串)
date: 2018-10-22 23:06:11
tags: [《Java核心技术》,学习笔记]
categories: [学习笔记,Java]
---
## 1.4 操作运算
Java 与基于Ｃ语言的所有语言使用相似的操作符，接下来我来找一些在java需要注意的地方。
### 基本运算
- 要格外小心`/`操作符，如果两个操作数都是整数，它代表整数。如果有一个操作数为小数则输出结果为浮点型。（类型的自动转换）。
- 整数除以０会导致异常，浮点数除以０会产生无限值或NaN,不会产生异常。
- 若ｎ为奇数，当ｎ为正数时，`n%2` 的值为１； 当ｎ为负数时，`n%2`的值为－１。实际中处理负数是不幸的。当操作数可能为负数时，切记小心使用％。可以考虑使用Math.floorMod方法。

### 数学方法
举个例子，没有操作符可以产生次方，但是你可以调用Math.pow方法，` Math.pow(x,y)`会产生ｘ的ｙ次方。

静态方法不用在对象上调用。像静态常量一样，你只需要在前面加上声明该静态方法/变量的类名

类似的方法还有`Math.sqrt`/`Math.min`/`Math.max`等

此外Math类中提供三角函数和对数方法，以及常量`Math.PI`和`Math.E`。

- Math类中提供一些可以让整数运算更安全的方法，你可以捕获异常或终止程序，而不会是像普通的数学运算符那样使程序以错误的结果默默继续运行。

### 数字类型的转换
在Java中，如果没有信息损失，转换总是合法的。
- 从byte类型　到　short、int、long或double
- 从short和char类型　到　int、long或double
- 从int类型到long、double
- 从整型到浮点型总是合法的

==需要注意的是　下面的转换是合法的，但是他们损失了信息==

- 从int到float
- 从long到float或double。

如果转换不允许，可以使用cast运算符（将目标类型写在括号中）来强制转换，例如：

``` java
double x = 3.75;
int n =(int)x;
//这种情况下，就丢弃了小数部分，将ｎh设置为了3.
```
如果你想获得四舍五入后的整数而不是直接丢弃小数，你可以使用Math.round(x)。

==当使用cast方式强制转换数据时，需要格外注意。==
如果你收到了警告，cast转换会悄悄地丢弃掉数字的部分数据，你可以使用Math.toIntExact方法代替，当无法转换时会抛出异常。

### 大数
如果基本的整数类型和浮点类型精确度还不够满足需求，你可以使用java.math包中的BigInteger和BigDecimal类。这些类的对象代表了数字，该数字有任意长序列的位数。他们实现了对任意精度整数/浮点数的计算。

BigInteger类的静态方法valueOf讲long型转换为BigInteger
``` java
BigInteger n = BigInteger.valueOf(124135431242L);
```
你也可以使用数字字符串构造BigInteger:
``` java
BigInteger m = new BigInteger("124135431242");
```
Java不允许对象使用操作符，因此操作大数时，必须使用方法调用。例如
``` java
BigInteger o = BigInteger.valueOf(5).multiply(n.add(m))
//5*(n+m)
```
记得在之前我们遇到的,使用浮点类型2.0-1.1的结果为0.99999999...吗,在这里我们使用BigDecimal可以计算出准确结果。
BigDecimal.valueOf(n,e) 返回BigDecimal实例，其值为ｎ×10的-e次方
``` java
BigDecimal.valueOf(2,0).substract(BigDecimal.valueOf(11,1))
//2.0-1.1 输出的结果精确为0.9
```

### 字符串
字符串就是一系列的字符，在java中，字符串可以包含任何Unicode字符。
#### 字符串连接
可以使用操作符＋连接两个字符串
如果将一个字符串和另一个值连接起来，另一个值会转换为字符串。

==注意　如果将连接与加写在一起，可能会有意想不到的结果==
例如：
``` java
int age =20;
"your age :" + age + 1;
//结果为　your age : 201
//正确为"your age :" + （age + 1）;
```
也可以使用String类下的join方法讲几个字符串连接起来并以分割符隔开
``` java
String names = String.join(",","Mino","Peter","Paul");
//names的结果为“Mino,Peter,Paul“
//或者第二个参数提供字符串数组
```
如果有大量字符串需要连接，那么上面这种方式是效率低下的，最好使用StringBuilder代替
``` java 
StringBuilder builder = new StringBuilder();
while(isMoreString){
    builder.append(nextString);
}
String result = builder.toString();
```

#### 子字符串（字符串拆分）
可以使用substring方法将字符串拆开
``` java
String greeting = "Hello world";
String location = greeting.substring(7,12);
//设置location为"world"(起位置７　结束位置12,字符串的位置从０开始)
```
你也可以用split()方法将含有分隔符分割的字符串提取出子字符串，返回一个子字符串数组。
```java 
String names = "Peter,Mino,Mary";
String[] result = names.split(",");
//返回的结果值为数组["Peter","Mino","Mary"]
```
#### 字符串比较
``` java
location.equals("world");
//若location是字符串”world“,返回真
```
==不要使用`==`操作符来比较字符串，只有当两者在内存中为同一对象时，返回真，（可以看成地址的比较）。在Java虚拟机中，每个文字串只有一个实例，所以”world“`==`"world"为真，但是如果字符串是计算得来的，那么变量中的字符串会单独存放到一个实例中，此时虽然他们的值相等，但`==`返回值为假==

例如：
``` java
"world"=="world";
//返回true
String greeting = "Hello world";
String location = greeting.substring(7,12);
location == "world";
//返回false
```
与其他任何变量一样，String变量可以是null，这表明该变量根本没有引用任何对象，甚至不是一个空字符串。

null与空字符串`""`并不相同。空字符串是个长度为零的字符串，而null根本不是字符串。在null上调用任何方法都会抛出空指针异常。

- 可以使用`==`来测试一个字符串是否为null.` if(location == null)... `
- 比较字符串和文字串时，将文字串放在前面会是一个好主意，英文即使字符串为null，这个语句也可以正常工作
``` java
if("world".equals(location)) ...
```
##### equalsIgnoreCase方法
不考虑大小写的比较两个字符串

##### compareTo方法
可以告诉你两个字符串按照字典中的顺序（依赖于Unicode值）哪个在前。
``` java
first.compareTo(second)
```
- 返回值<0(负整数)，first排前
- 返回值=0 ,两者相等
- 返回值>0(正整数),second排前

对用户可读的字符串排序时，推荐使用Collator对象（它知道关于特定语言的排序规则）。

#### 数字与字符串转换
**整数转换为字符串**　可调用静态方法　Integer.toString(num);
可以传入第二个参数作为转换的进制（2~36）Integer.toString(num , 2); 则返回为num转换为２进制之后的结果
- 将整数转换为字符串的更简单的办法是`""+num`.但是有些人认为这种方式比较丑陋并且效率低下。

相反

**将整数的字符串转为数字**，使用Integer.parseInt方法。
``` java
n = Integer.parseInt("11");
//也可以指定进制数
m = Integer.parseInt("11",2);
//ｍ的值为整数３
```

对于**浮点数**，也有对应的Double.toString和Double.parseDouble方法。

#### String类API
***有用的String类方法***
![有用的String类方法](/img/core_java_post/day3-1.jpg)
注意，在Java 中　String 类是不可变的(immutable)．也就是说任何String类的方法都不能修改自己的字符串。返回的是一个新的字符串，源字符串不会改变。

== 可以在网上下载详细的JavaAPI文档，里面有各个API的详解 ==

#### 编码点和编码单元
Java开始创建时，拥抱了刚出来不久的Unicode标准，目的是解决棘手的字符编码的问题在Unicode之前，有很多不兼容的字符编码方式，每个国家都在做类似的事情，拓展ASCII标准来容纳各自的语言符号，不同编码文件之间的交流成为了一个大问题。

现在，Unicode需要21个bit，而不是设计之初的16bit(因为引入了比实际时估计更多的字符而扩容)。每个有效的Unicode值被称为编码点(code point).

Java 诞生在16bit向21bit过度的时期，深受其苦。在Java中，字符串不是Unicode字符或者编码点的序列，而是编码单元(code unit),UTF-16编码的１６比特序列.
![编码点和编码单元的一些方法](/img/core_java_post/day3-2.jpg)