---
title: Core Java Day 2--基本编程结构2(基本类型、变量)
date: 2018-10-19 19:50:07
tags: [《Java核心技术》,学习笔记]
categories: [学习笔记,Java]
---

## 1.2 基本类型

接下来我们来看一下Java中7的基本类型。最简单的Java类型被称为基本类型，这些与C/C++基本类似，详细的我就不在这里介绍了，主要找一些需要注意的地方吧

### 1.2.1 整型

整型是没有小数部分的数字，java中有四种整型

![java中的四种整型](/img/core_java_post/day2-1.jpg)

1. 我们可以通过这些类的`.MIN_VALUE`,`.MAX_VALUE`常量获取到其可取到的最大值和最小值

如果`long`类型还不够用，则使用`BigInteger`类

1. 与C/C++不同的是，Java中的整型范围不依赖程序所运行的机器。"编写一次 到处运行";
2. 在java中你还可以给数字添加下划线 ，使阅读变得更加容易，Java编译器会删除他们。例如1_000_000。

### 1.2.2 浮点类型

浮点类型是指有分数的数字
![浮点类型](/img/core_java_post/day2-2.jpg)

1. float类有个F后缀（例如 `3.14F`）。没有F后缀的浮点类型被默认为Double 你可以选择D作为double类型的后缀（例如3.14D）
2. 有一个特殊的浮点值 `Double.POSITIVE_INFINITY` 代表正无穷 `Double.NEGATIVE_INFINITY` 代表负无穷。例如，1.0/0.0结果就是无穷大。
3.` Double.NaN `代表非数值， 0例如，.0/0.0或者负数的平方根就会产生无数值NaN。
4. 需要注意的是，所有的NaN 都被认为是彼此不同的所以不能用简单的`if(x == NaN)`来检查x是否是`NaN`。但是，你可以使用`Double.isNaN(x)`来判断。同样 你还可以使用Double.isInfinite来测试是否为正负无穷大 用方法`Double.isFinite`可以检查浮点数既不是无穷也不是NaN

有时浮点数会存在一些舍入误差，并不适用于对精度要求特别高的地方 例如命令

System.out.println(2.0-1.1)的打印结果为0.899999999999999... 而非我们所期望的0.9

这样的原因是浮点数使用了二进制系统进行表示。分数十分之一没法用精确的二进制表示。就好像分时三分之一在十进制下无法精确表示一样。如果你需要精确而无舍入误差的数字计算 你可以使用BigDecimal类

### 1.2.3 char 型
![char类型](/img/core_java_post/day2-3.jpg)

你可能不会经常使用char型，char型描述了Java使用的UTF-16字符编码中的“编码单元”;


### 1.2.4 布尔型

布尔型有两种值 true 和 false

但值得注意的是 在java中布尔类型不是数字类型。布尔值与整数0和1没有关系

## 1.3 变量

关于如何声明与初始化变量与常量

### 1.3.1变量的声明

Java是强类型语言。变量值只能是某个具体类型的值。当你声明变量时，需要指定变量的类型、名称和一个可选的初始值。

如：
```java
int total = 0;
```
这部分基本与C/C++类似

&quot;大多数Java程序员喜欢每个变量有各自单独的声明&quot;

当你声明变量并用构造对象初始化变量时，对象所属类的名称会出现两次：如
``` java
Random generator = new Random();
//第一个变量Random 是变量generator的类型。第二个Random是构造该类对象的表达式的一部分。
```
### 1.3.2 关于变量的名称

允许的变量名格式与C、C++基本类似

根据习惯 变量和方法的名称以小写字母开始，类的名称的第一个字母大写。

Java程序员喜欢使用&quot;驼峰式大小写&quot;，也就是当名称由多个单词组成时，单词首字母大写，例如countOfNums。

### 1.3.3 初始化

1. 和C、C++相同，你必须再使用变量前对声明的变量进行初始化
2. 允许在方法的任何地方声明变量，但尽可能晚地声明变量，刚好在你首次需要变量的前一刻声明，被认为是一种好习惯。

### 1.3.4 常量

关键词final表示一个值，一旦赋值后就不能改变了。
``` java
final int DAYS_PER_WEEK = 7;
```
根据习惯，常量的名称使用大写字母。

你也可以在方法外，使用static关键词声明常量。例如：
``` java
public class Calendar {
    public static final int DAYS_PER_WEEK = 7;
}
```
常量可以用在多个方法中。在Calendar类内部，你可以直接引用常量DAYS_PER_WEEK。而要在其他的类中使用常量，前面要加上类名：Calendar.DAYS_PER_WEEK。

例如我们常用的输出对象

System类中声明了一个out常量
``` java
public static final PrintStream out;
```
于是 你就可以在任何地方愉快的使用System.out对象了。貌似这是极少数几个常量没有大写的情况

1.  延迟final变量的初始化是允许的，只要确保它在使用之前恰好初始化了一次即可，例如：
``` java
final int DAYS_IN_FEBRUARY;

if(oleapYear){
    DAYS_IN_FEBRUARY =29;
}else{
    DAYS_IN_FEBRUARY = 28;
}
```
2. final变量 一旦被赋值 它就是最终的值，且无法被改变。

如果你需要定义一组相关的常量，这时你可以定义枚举类型了。
![枚举类型](/img/core_java_post/day2-4.jpg)